---
# 🚧 PRECURSOR DRAFT — born 2026-06-01. Demonstrates the .liber format.
# The full, expert-reviewed landscape is in active development. Confidence is
# deliberately modest. Trade-offs are real but not yet expert-signed-off.
domain: database-design-patterns
verified: 2026-06-01
schema_version: "0.4"
generation_method: ai_assisted
confidence: medium
sources:
  - url: https://learn.microsoft.com/en-us/sql/relational-databases/tables/primary-and-foreign-key-constraints
    type: official_docs
    title: "Microsoft Learn — Primary and Foreign Key Constraints (SQL Server)"
  - url: https://www.rfc-editor.org/rfc/rfc9562
    type: standard
    title: "RFC 9562 — Universally Unique IDentifiers (UUID), incl. UUIDv7"
  - url: https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/
    type: standard
    title: "Kimball Group — Dimensional Modeling Techniques (surrogate keys in DW)"
  - url: https://www.sqlskills.com/blogs/kimberly/guids-as-primary-keys-andor-the-clustering-key/
    type: community
    title: "SQLskills (Kimberly Tripp) — GUIDs as primary and/or clustering key"
options:
  - id: surrogate-key-bigint-identity
    name: "Surrogate key — monotonic BIGINT IDENTITY / sequence"
    status: mainstream
  - id: natural-key-composite
    name: "Natural / composite business key"
    status: mainstream
  - id: surrogate-key-uuid-v7
    name: "Surrogate key — time-ordered UUID (UUIDv7) / NEWSEQUENTIALID"
    status: emerging
  - id: surrogate-key-guid-random
    name: "Surrogate key — random GUID / UUIDv4 as clustering key"
    status: declining
context_filters:
  workload: [oltp, olap, hybrid]
  write_topology: [single-writer, distributed-multi-master, offline-first]
  scale: [small, medium, large]
critical_exclusions:
  - "NEVER cluster a high-insert OLTP table on a random GUID / UUIDv4 — random insert order causes page splits, fragmentation, and buffer-pool churn."
  - "NEVER use a mutable business attribute as a primary key — when it changes, the cascade through every foreign key is the bug you'll be hunting for a week."
  - "NEVER expose a sequential surrogate key in a public URL if row-count or enumeration leakage matters — add an opaque external identifier instead."
known_failures:
  - option: surrogate-key-guid-random
    failure: "Random GUID as the clustered key on an insert-heavy table → severe index fragmentation, frequent page splits, larger I/O, and cache pressure. The canonical SQL Server anti-pattern."
    mitigation: "Use a time-ordered identifier (UUIDv7, or NEWSEQUENTIALID on SQL Server) if you need GUID-shaped keys, or keep a BIGINT IDENTITY clustering key and store the GUID as a non-clustered unique column."
related:
  see_also:
    - domain: database-normalization-strategy
      context: "3NF vs dimensional/star schema vs one-big-table — the OLTP/OLAP axis"
    - domain: temporal-history-tracking
      context: "system-versioned temporal tables vs CDC vs trigger audit vs SCD Type 2"
uncertainty_sources:
  - "Precursor draft, not yet reviewed by a domain expert."
  - "adoption_data intentionally omitted — no runtime decision receipts exist yet (the right side of the wheel is not live)."
  - "UUIDv7 native engine support is still uneven; in practice it is often application-generated rather than database-generated."
---

# Database Design Patterns — Decision Landscape

> ## 🚧 Precursor draft — born June 1, 2026
> This file exists to show the `.liber` format **breathing**, not to be the finished
> reference. The trade-offs below are real and carefully chosen, but they have **not yet
> been signed off by a domain expert**. The full, expert-reviewed database-design landscape
> — and a split into focused files (normalization, temporal history, indexing) — is in
> active development. Treat this as a working draft. Corrections welcome via issue or PR.
>
> **Scope of this draft:** one foundational decision — **how to choose a primary / clustering
> key** for a relational table. The broader `database-design-patterns` domain will expand and
> split into focused landscapes; see `related.see_also` in the frontmatter.

---

## ⚡ Quick Pick (30 sec)

For most relational **OLTP** schemas with a single writer: a **monotonic surrogate key**
(`BIGINT IDENTITY` on SQL Server, or a sequence). It's narrow (8 bytes), insert-friendly as
a clustering key, and carries no business meaning that can change underneath you.

Reach for a **UUID only when you have a genuine distributed- or offline-generation
requirement** — and if you do, prefer a **time-ordered variant (UUIDv7 / `NEWSEQUENTIALID`)**
over a random GUID, so you keep insert locality.

Add a separate `UNIQUE` constraint on the real-world **natural key** regardless of which
surrogate you choose — the surrogate identifies the *row*; the natural key protects against
*duplicate business facts*.

---

## 🔀 Pivotal Points (5 min)

Answer these, in order — each one narrows the landscape:

1. **Who generates the key?**
   - One database / one writer → `BIGINT IDENTITY` is hard to beat.
   - Many writers / distributed or multi-master → you need globally-unique generation → UUID family.
   - Offline-first clients that create rows before syncing → app-generated UUID.

2. **Is the key clustered (the physical row order)?**
   - If yes, **insert order matters enormously.** Monotonic = good; random = fragmentation.
   - This single question is what kills random GUIDs as clustering keys.

3. **Is the identifier exposed publicly (URLs, APIs)?**
   - Sequential integers leak row counts and invite enumeration.
   - If that's a concern, keep an internal `BIGINT` key **and** add an opaque external ID.

4. **Does a stable, immutable natural key already exist?**
   - For small, stable reference/dimension tables, a natural key can be the simplest honest choice.
   - For anything where the business value can change, do **not** make it the key.

5. **OLTP or analytical (DW)?**
   - OLTP: optimize for insert/point-lookup → narrow monotonic surrogate.
   - Dimensional / star schema: surrogate keys on dimensions are standard practice (decouples the
     warehouse from source-system keys and enables slowly-changing-dimension history).

---

## 📊 Full Landscape (15 min)

### `surrogate-key-bigint-identity` — monotonic `BIGINT IDENTITY` / sequence · *mainstream* · research confidence ~0.85

The default workhorse for relational OLTP.

- **Strengths:** Narrow (8 bytes) → smaller non-clustered indexes and foreign keys, faster joins, less I/O. Monotonic insert order → minimal clustered-index fragmentation. Simple, well-understood, no business meaning to drift.
- **Trade-offs:** Not portable across systems (identity reseed and gaps are real operational quirks). Reveals approximate row counts if exposed. Doesn't, by itself, enforce business uniqueness — you still need a `UNIQUE` constraint on the natural key. Not safe for distributed or offline generation. Merge replication needs deliberate handling.
- **Fits:** single-writer OLTP, internal identifiers, the large majority of relational tables.
- **Avoid when:** distributed multi-master writes, public enumeration is a threat, offline clients must mint IDs.

### `natural-key-composite` — natural / composite business key · *mainstream* · research confidence ~0.70

Let the real-world identifier *be* the key.

- **Strengths:** Enforces genuine business uniqueness directly. No surrogate-to-natural lookup hop. Meaningful and self-documenting. Often the right call for junction/bridge tables (the composite of the two foreign keys).
- **Trade-offs:** Business keys **change** — and a changing key cascades through every foreign key referencing it (painful, error-prone). Wide composite keys bloat every dependent index and FK. Join predicates get verbose. Nullability of parts complicates uniqueness.
- **Fits:** small, stable reference/dimension tables; truly immutable identifiers (e.g., standardized codes); pure junction tables.
- **Avoid when:** the "natural" attribute is anything a human or upstream system might revise.

### `surrogate-key-uuid-v7` — time-ordered UUID (UUIDv7) / `NEWSEQUENTIALID` · *emerging* · research confidence ~0.65

The modern attempt to get GUID-shaped keys without the fragmentation tax.

- **Strengths:** Globally unique and generatable anywhere (distributed, offline). Time-ordered, so insert locality is far better than random GUIDs — fragmentation is dramatically reduced. Harder to enumerate than sequential integers.
- **Trade-offs:** Still 16 bytes — double a `BIGINT`, so every dependent index and FK is wider. UUIDv7 **native database support is uneven** (often app-generated in practice). `NEWSEQUENTIALID` is SQL-Server-specific and its values are somewhat predictable, which can be a privacy consideration.
- **Fits:** distributed writes where you also want index-friendly keys; systems that want one ID scheme across services.
- **Avoid when:** a narrow `BIGINT` would do and you have no distributed-generation need (you'd be paying the width cost for nothing).

### `surrogate-key-guid-random` — random GUID / UUIDv4 as the **clustering** key · *declining* · research confidence ~0.75

Included precisely because it's a common, costly mistake. (See `known_failures`.)

- **Where it's fine:** as a **non-clustered**, app-generated unique identifier, or in low-insert tables.
- **Where it hurts:** as the **clustered** key of a high-insert table — random insert order forces page splits, fragments the index, and thrashes the buffer pool. 16-byte width compounds the cost across every index and FK.
- **The fix:** if you need GUID-shaped keys, use the time-ordered variant above; otherwise keep a monotonic `BIGINT` clustering key and store the random GUID as a separate non-clustered unique column.

---

## 🚫 Critical Exclusions

- **Never** cluster a high-insert OLTP table on a random GUID / UUIDv4.
- **Never** use a mutable business attribute as the primary key.
- **Never** expose a sequential surrogate in a public URL when enumeration or row-count leakage is a concern.

---

## 🔄 Contrarian Corner

- **"Always use a surrogate key" is dogma, not law.** For small, stable reference tables, a clean natural key removes a whole layer of indirection. The honest rule is "surrogate by default, natural when the key is genuinely immutable," not "surrogate always."
- **The GUID-vs-identity war is mostly about *clustering order*, not the type itself.** A GUID stored as a non-clustered unique column alongside a monotonic clustering key sidesteps almost the entire fragmentation argument. The framing "GUIDs are bad" is a proxy for "random clustering keys are bad."
- **UUIDv7 may quietly retire this debate.** As time-ordered UUIDs gain native engine support, the historical trade-off between "globally unique" and "index-friendly" weakens — watch this space.

---

## What this file adds (that 3 web searches won't)

The web will happily tell you *how* to declare an `IDENTITY` column or *what* a UUID is. What it
scatters across a dozen conflicting blog posts is the **decision shape**: that the real axis is
*who generates the key* × *is it the clustering order* × *is it publicly exposed*, that the
GUID-vs-identity flame war is really an argument about random clustering order, and that UUIDv7
is actively collapsing the old trade-off. That synthesis — and the explicit `known_failures` —
is what a `.liber` file is for.

---

## Sources

1. Microsoft Learn — *Primary and Foreign Key Constraints* (SQL Server). *official_docs.*
2. RFC 9562 — *Universally Unique IDentifiers (UUID)*, including UUIDv7. IETF. *standard.*
3. Kimball Group — *Dimensional Modeling Techniques* (surrogate keys in the data warehouse). *standard.*
4. SQLskills / Kimberly Tripp — *GUIDs as primary and/or clustering key.* *community.*

*adoption_data is intentionally absent: no runtime decision receipts exist yet. Once the right
side of the LIBER wheel is live, this section will carry anonymized, aggregated data on how teams
actually choose — kept strictly separate from the research `confidence` above.*

---

*A `.liber` file · format spec v0.4.0-draft · part of [LIBER](https://github.com/useliber/liber).
Precursor draft · first edition 2026-06-01.*
