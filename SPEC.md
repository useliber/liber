# .liber Format Specification

**Version:** 0.4.0-draft  
**Date:** 2026-04-01  
**Status:** DRAFT — invitation for feedback  
**License:** CC BY-SA 4.0  
**Repository:** [github.com/useliber](https://github.com/useliber)

---

## 1. Preamble

### 1.1. What Is .liber?

A `.liber` file is a structured knowledge artifact that maps a **decision landscape** — the options available, the trade-offs between them, and the contexts that shift which option fits best. It answers the question *"which X should I choose?"* rather than *"how do I use X?"*

The format is deliberately simple: a YAML frontmatter block followed by a Markdown body. Any developer who has written a blog post with Jekyll, Hugo, or Astro already knows how to read and write a `.liber` file. Any tool that parses YAML frontmatter (gray-matter, python-frontmatter) can parse `.liber`. No custom tooling required.

### 1.2. Genealogy — Two Chains, One Crossroads

`.liber` stands at the intersection of two independent lineages:

**Chain I: Democratization of knowledge**

```
Christopher Alexander (A Pattern Language, 1977)
  → Ward Cunningham (Wiki, 1995)
    → Wikipedia (2001)
      → Gang of Four (Design Patterns, 1994)
        → Architecture Decision Records (Nygard, 2011)
          → LIBER (2026)
```

Each generation solved a knowledge problem of its era. Alexander gave architecture a shared language. Wiki made it collaborative. Wikipedia made it universal. ADR made it project-local. LIBER makes decision knowledge **structured, machine-readable, cyclically refreshed, and positioned BEFORE the decision** — not after it.

**Chain II: Certification of trust in technology**

```
UL (product safety, 1894)
  → ISO 9001 (quality management, 1987)
    → SOC 2 (data security, 2010)
      → ISO 42001 (AI governance, 2023)
        → AIUC-1 (agent security, 2026)
          → LIBER ADCL (decision quality, 2026)
```

Every generation certified a deeper layer of trust. UL certified that products won't electrocute you. SOC 2 certified that data won't leak. ISO 42001 certified that AI governance exists. But none certify that an AI agent **makes good decisions**. LIBER's ADCL layer (Agent Decision Certification) fills this gap.

**The crossroads:** LIBER simultaneously **delivers** decision knowledge (Chain I) and **certifies** the quality of decisions made on that knowledge (Chain II). The `.liber` format is the shared contract enabling both.

### 1.3. Positioning: "Which X" Not "How to X"

`.liber` occupies a specific niche in the ecosystem of agent context files. It is *not* a replacement for any of them — it is complementary:

| Format | Purpose | Relationship to .liber |
|--------|---------|----------------------|
| **SKILL.md** | How to execute a task ("use pytest with fixtures") | Cousin format. Same architecture (YAML FM + MD body). Different goal. A SKILL.md parser can read `.liber` frontmatter without modification. |
| **AGENTS.md** | Repository-level instructions for coding agents | `.liber` is *referenced by* AGENTS.md. AGENTS.md says "use pytest." `.liber` says "pytest vs unittest vs hypothesis — here are the trade-offs." |
| **llms.txt** | Site-level content map for AI discovery | `.liber` frontmatter serves the same function at file level: a programmatic interface for agents to discover decision knowledge without parsing the body. |
| **ADR (MADR)** | Record of a specific decision taken | `.liber` is *input to* ADR. `.liber` presents the landscape. ADR records the choice made. |

### 1.4. What .liber Is NOT

- **Not an API reference.** If the information lives in the tool's official docs, link to it — don't repeat it.
- **Not a tutorial.** `.liber` does not teach you *how* to use a tool. It helps you decide *whether* to use it.
- **Not a recommendation engine.** A `.liber` file presents options and trade-offs. It does not say "use this one."
- **Not an encyclopedia.** If a topic doesn't involve a decision between alternatives, it doesn't need a `.liber` file.
- **Not an ADR.** ADRs record decisions *taken*. `.liber` presents decisions *available*.

### 1.5. Design Principles

Five principles govern this specification:

1. **Frame, not cage.** The spec defines boundaries for improvisation, not a rigid template. Minimal core + structured freedom. (Jazz analogy: learn the changes, then solo.)
2. **Non-inferable details only.** A `.liber` file earns its existence by containing knowledge an agent *cannot* find in 3 web searches. If it's in the official docs — link, don't repeat. (Gloaguen et al., ETH Zurich, 2026: LLM-generated context files that repeat available information degrade agent performance by 3% while increasing cost by 20%.)
3. **Two audiences, one file.** Every design choice serves both a human reading Markdown on GitHub *and* an agent parsing YAML frontmatter programmatically.
4. **Age loudly.** Knowledge that rots in silence is worse than no knowledge. Freshness is tracked, staleness is visible, and decay triggers action.
5. **Zero learning curve.** `.liber` is Markdown + YAML. If you know how to write a README, you know how to write a `.liber` file.

---

## 2. Conventions

### 2.1. Requirement Levels (RFC 2119)

This specification uses requirement level keywords as defined in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119):

- **MUST / REQUIRED / SHALL** — absolute requirement. Violation makes the file invalid.
- **MUST NOT / SHALL NOT** — absolute prohibition.
- **SHOULD / RECOMMENDED** — valid reasons may exist to ignore, but implications must be understood.
- **SHOULD NOT / NOT RECOMMENDED** — valid reasons may exist to do it, but implications must be understood.
- **MAY / OPTIONAL** — truly optional. Omission requires no justification.

These keywords appear in UPPERCASE throughout this specification.

### 2.2. Versioning

This specification follows [Semantic Versioning 2.0.0](https://semver.org/):

- **v0.x** (current): Draft. Breaking changes permitted between minor versions.
- **v1.0**: Stable. The four MUST fields become frozen. Backward compatibility guaranteed. New fields added as SHOULD or MAY only. Existing field semantics never changed.
- **v2.0+**: Reserved for fundamental redesign. Expected: never. The core should be frozen like DNA's genetic code — 3.5 billion years and counting.

The `schema_version` frontmatter field tracks which spec version a file targets.

---

## 3. Format Overview

### 3.1. File Structure

A `.liber` file consists of two parts:

```
---
[YAML frontmatter]
---

[Markdown body]
```

The frontmatter is delimited by triple-dash fences (`---`). Everything above the closing fence is YAML. Everything below is Markdown.

### 3.2. File Extension

Files MAY use `.liber` or `.md` as their extension. Both are equally valid.

**Recommendation for Phase 0–1:** Use `.md`. GitHub natively renders `.md` files with YAML frontmatter displayed as a table. `.liber` files do not render in GitHub, VS Code, or Obsidian without configuration. The `.liber` extension has marketing value but causes friction. Adopt it when tooling matures.

### 3.3. Encoding

Files MUST be UTF-8 encoded.

### 3.4. Parsing

Any standard YAML frontmatter parser can read `.liber` files:

| Language | Parser | Install |
|----------|--------|---------|
| JavaScript | gray-matter | `npm install gray-matter` |
| Python | python-frontmatter | `pip install python-frontmatter` |
| Go | gohugoio/hugo/parser | standard library |

No custom parser is required. No custom parser SHOULD be built. If you need a custom parser to work with `.liber`, the format has failed.

### 3.5. Validation

Frontmatter is validated against a JSON Schema (Draft 2020-12). The complete schema is in [Appendix A](#appendix-a-json-schema). Validation tooling:

- **ajv** (JavaScript): `ajv validate -s liber-schema.json -d file.liber`
- **jsonschema** (Python): standard library compatible
- **Zod** (TypeScript): schema can be derived from JSON Schema

### 3.6. Naming Conventions

Consistent naming enables automated tooling, cross-file linking, and human readability.

| Element | Convention | Pattern | Example |
|---------|-----------|---------|---------|
| File names | kebab-case + `.liber.md` (RECOMMENDED) or `.md` | `[a-z0-9-]+` | `auth-patterns.liber.md` |
| YAML field names | snake_case | `[a-z_]+` | `context_filters`, `adoption_data` |
| Option IDs | kebab-case, lowercase | `^[a-z0-9][a-z0-9-]*[a-z0-9]$` | `jwt-stateless`, `server-side-sessions` |
| Domain names | kebab-case, lowercase | same as option IDs | `auth-patterns`, `database-selection` |
| Status values | lowercase single-word enum | `mainstream\|emerging\|declining\|deprecated` | `mainstream` |

**YAML formatting:** 2-space indentation (no tabs), block arrays (one item per line), MUST fields first in frontmatter.

```
✅ domain: auth-patterns            ❌ domain: authPatterns
✅ context_filters:                  ❌ contextFilters:
✅ id: jwt-stateless                 ❌ id: JWT_Stateless
✅ status: mainstream                ❌ status: "Main-Stream"
```

---

## 4. Frontmatter Specification

### 4.1. Stratified Frontmatter

Frontmatter is designed in two tiers to support progressive consumption by agents:

| Level | Token Budget | Contents | Use Case |
|-------|-------------|----------|----------|
| **Level 0** | < 100 tokens | `domain`, `verified`, `sources`, `confidence`, `options` as flat list (id + name + status only) | Listing, indexing, catalog browsing |
| **Level 1** | < 200 tokens | Level 0 + `context_filters`, `critical_exclusions`, `emerging` | Basic filtering and matching |

**Rationale:** An agent deciding *which* `.liber` file to load needs Level 0. An agent filtering by context needs Level 1. Neither needs the full trade-off analysis — that lives in the body. This prevents duplication between frontmatter and body (a 20-30% token waste identified in review).

**Token budget:** Frontmatter SHOULD stay under 200 tokens. Frontmatter MUST NOT exceed 500 tokens. If it does, move content to the body.

#### Runtime Implications

The stratified design enables sub-100ms serving on edge runtimes (Cloudflare Workers, Deno Deploy). Format constraints that make this possible:

| Constraint | Limit | Rationale |
|-----------|-------|-----------|
| YAML nesting depth | MUST NOT exceed 4 levels | Keeps parse time <10ms on V8 isolates |
| `options` array length | SHOULD NOT exceed 30 items | Beyond 30 → split into sub-domain files |
| String values | SHOULD be <500 characters | Extended text belongs in body |
| Total frontmatter size | SHOULD be <50KB | Body has no size limit |

### 4.2. MUST Fields

These four fields are REQUIRED. A file missing any of them is invalid.

---

#### `domain`

- **Type:** `string`
- **Pattern:** `^[a-z0-9][a-z0-9-]*[a-z0-9]$` (lowercase alphanumeric + hyphens, no leading/trailing hyphen)
- **Purpose:** Uniquely identifies the decision landscape this file covers.

Be specific. `auth-patterns` is a good domain name. `security` is too broad — an agent querying "security" would match everything and learn nothing.

```yaml
domain: auth-patterns        # good — specific decision landscape
domain: database-selection    # good
domain: security              # bad — too generic, undertriggers
domain: databases             # bad — not a decision, just a category
```

**Rationale:** Domain is the primary key for discovery. Agents match queries to domain names. A vague domain produces vague matches. (This mirrors SKILL.md's "pushy descriptions" principle: specific names trigger more accurately than generic ones.)

---

#### `verified`

- **Type:** `string` (ISO 8601 date: `YYYY-MM-DD`)
- **Purpose:** Date of last human or automated review of this file's content.

`verified` is the anchor for freshness tracking. It answers: "When did someone last confirm this information is still accurate?"

```yaml
verified: 2026-03-29
```

`verified` applies to the *entire file* — both frontmatter and body. If the body contains temporal claims ("as of Q1 2026"), they MUST be consistent with `verified`.

**Rationale:** Without `verified`, confidence decay cannot function. A file without a date is like a Wikipedia article without "last edited" — concerning, but at least you know to be skeptical. The alternative (making `verified` SHOULD) was considered and rejected: lifecycle enforcement requires a date to operate on.

---

#### `sources`

- **Type:** `array` of source objects (or plain URL strings for minimal files)
- **Minimum items:** 2
- **Purpose:** Where the knowledge in this file comes from. Trust is imported from these sources.

**Source object fields:**

| Field | Type | Required | Description |
|-------|------|:--------:|-------------|
| `url` | `string` (URI) | MUST | Clickable, verifiable URL |
| `type` | `enum` | SHOULD | `standard`, `vendor`, `community`, `academic`, `official_docs` |
| `title` | `string` | MAY | Human-readable source name |
| `accessed` | `date` | MAY | When the source was last checked |

**Source type taxonomy:**

| Type | Definition | Example |
|------|-----------|---------|
| `standard` | Industry standard, RFC, W3C spec, OWASP | RFC 7519, OWASP ASVS |
| `vendor` | Documentation from the technology vendor | Auth0 docs, AWS Cognito |
| `community` | Blog posts, conference talks, tutorials | InfoQ article, Dev.to post |
| `academic` | Peer-reviewed papers, university research | ACM/IEEE paper |
| `official_docs` | Official project documentation | Next.js docs, Redis docs |

```yaml
# Full form (RECOMMENDED for publication-ready files)
sources:
  - url: https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
    type: standard
  - url: https://auth0.com/docs/get-started/architecture-scenarios
    type: vendor
  - url: https://webauthn.guide/
    type: community

# Short form (VALID for drafts and bare-minimum files)
sources:
  - https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
  - https://auth0.com/docs/get-started/architecture-scenarios
```

Every URL MUST be clickable and resolve to a live resource at the time of `verified`. Broken links are detected by CI (lychee or equivalent link checker).

**Anti-gaming rule:** In publication-ready files (files with `options` present), `vendor` sources MUST NOT be the sole source type for any option discussed. At least one non-vendor source (`standard`, `community`, `academic`, or `official_docs`) MUST be present. This prevents vendor documentation from being the only evidence base for a decision landscape.

**Rationale:** A `.liber` file without sources is a blog post without references. Trust is not asserted — it is imported from credible sources. Two is the minimum because a single source is a summary, not a landscape. The `type` field enables automated quality checks — CI can flag files where all sources are `vendor` type.

---

#### `confidence`

- **Type:** `number` (0.0–0.95, step 0.05) OR `string` enum (`high`, `medium`, `low`) OR `null`
- **Purpose:** Research confidence — how well-supported is this knowledge by the cited sources?

```yaml
confidence: 0.85      # numeric — precise
confidence: high       # enum — honest
confidence: null       # valid — author did not assess
```

**Normalization:** Parsers MUST normalize enum values to numbers for computation:

| Enum | Numeric |
|------|---------|
| `high` | 0.85 |
| `medium` | 0.65 |
| `low` | 0.40 |

**confidence MUST NOT be 1.0.** There is always uncertainty. A confidence of 1.0 claims perfection, which no knowledge artifact can guarantee. Maximum is 0.95.

**confidence: null** is VALID. It means "the author did not assess confidence." This is more honest than forcing an arbitrary number. Parsers SHOULD treat `null` as a soft warning, not an error.

**INVARIANT: Two types of confidence exist and MUST NEVER be mixed:**

| Type | Meaning | Source |
|------|---------|--------|
| `confidence` (this field) | Research confidence — how well-sourced is this knowledge? | Sources, expert review |
| `adoption_data` (MAY field) | Empirical adoption data — how are people actually using these options? | Runtime decision receipts |

These are separate measurements with separate semantics. Combining them into a single score would be scientifically meaningless — like averaging temperature and humidity into a single number.

---

### 4.3. SHOULD Fields

These fields significantly improve a file's value. Omitting them produces a valid file but a less useful one.

---

#### `options`

- **Type:** `array` of objects
- **Minimum items:** 2
- **Purpose:** The decision landscape — what choices are available.

In frontmatter, options appear as a **flat list** (Level 0). Full trade-offs live in the body.

```yaml
options:
  - id: jwt-stateless
    name: JWT Stateless Authentication
    status: mainstream
  - id: session-cookies
    name: Server-Side Sessions
    status: mainstream
  - id: passkeys
    name: Passkeys / WebAuthn
    status: emerging
```

Each option object in frontmatter:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | MUST | Unique within this file. Lowercase, hyphenated. |
| `name` | string | MUST | Human-readable label. |
| `status` | enum | MUST | `mainstream`, `emerging`, `declining`, `deprecated` |

Additional option fields (`when_to_use`, `when_not_to_use`, `pros`, `cons`, `confidence`) belong in the body, not the frontmatter. This prevents frontmatter/body duplication.

**Minimum 2 options.** A file with one option is a recommendation, not a landscape. If there is genuinely only one viable option, the file probably shouldn't exist.

---

#### `context_filters`

- **Type:** `object` (keys: strings, values: arrays of strings)
- **Purpose:** Contextual dimensions that change which option fits best.

```yaml
context_filters:
  team_size: [solo, small, medium, large]
  architecture: [monolith, microservices, serverless]
  compliance: [none, SOC2, HIPAA, PCI-DSS]
```

Context filters enable agent queries like *"auth for a small team building microservices with SOC2."* Without them, the agent must read the entire body to determine relevance.

Keys are not standardized across files — each domain defines its own relevant contexts. Values SHOULD be lowercase enums for machine matching.

---

#### `critical_exclusions`

- **Type:** `array` of `string`
- **Purpose:** What you MUST NEVER do. The via negativa.

```yaml
critical_exclusions:
  - NEVER use basic auth over unencrypted HTTP
  - NEVER store passwords in plaintext or reversible encryption
  - NEVER disable CSRF protection on authentication endpoints
```

**Rationale:** Borrowed from aviation's "boldface items" — memory items so critical they're printed in bold in the checklist. In decision-making, knowing what *not* to do is often more valuable than knowing what to do. Research on AI agent instructions confirms: explicit "do not" instructions outperform positive guidance (Gloaguen et al., 2026).

---

#### `version`

- **Type:** `string` (semver recommended)
- **Purpose:** Version of this specific `.liber` file's content.

```yaml
version: 1.2.0
```

---

#### `emerging`

- **Type:** `string`
- **Purpose:** A brief awareness signal about what's gaining traction in this domain.

```yaml
emerging: Passkeys gaining rapid adoption; WebAuthn Level 3 in draft
```

This is a *signal*, not an analysis. Full treatment belongs in the body.

---

#### `pivotal_points`

- **Type:** `array` of objects
- **Purpose:** Discriminating questions that branch the decision tree. Borrowed from medical differential diagnosis (DDx).

```yaml
pivotal_points:
  - question: Do you need to revoke individual sessions?
    if_yes: [session-cookies, token-blacklist]
    if_no: [jwt-stateless, api-keys]
  - question: Is your primary threat phishing?
    if_yes: [passkeys, hardware-keys]
    if_no: [any]
```

---

#### `generation_method`

- **Type:** `enum`: `ai_generated`, `ai_assisted`, `human_authored`
- **Purpose:** Transparency about how this file was created.

```yaml
generation_method: ai_assisted
```

| Value | Meaning |
|-------|---------|
| `ai_generated` | Created by an AI engine without human review |
| `ai_assisted` | AI draft with human review and editing |
| `human_authored` | Written by a human, possibly with AI tools for editing |

**Rationale:** EU AI Act Article 50 transparency obligations take effect August 2, 2026. Machine-readable marking of AI-generated content will be required. This field positions `.liber` as proactively compliant. Transparency over perfection — an `ai_generated` file with honest labeling is more trustworthy than an unlabeled one.

---

#### `schema_version`

- **Type:** `string`
- **Purpose:** Which version of this specification the file targets.

```yaml
schema_version: "0.4"
```

Enables forward compatibility. A parser for v1.0 encountering `schema_version: "0.3"` knows to apply v0.3 validation rules.

---

### 4.4. MAY Fields

These fields are fully optional. Their absence is silent — no warnings, no errors.

---

#### `adoption_data`

- **Type:** `object`
- **Purpose:** Empirical data from runtime decision receipts.

```yaml
adoption_data:
  total_decisions: 1247
  period: 2026-Q1
  disclosure: "Aggregated from 1247 anonymized decisions. Selection bias noted."
  distribution:
    jwt-stateless: 0.45
    session-cookies: 0.32
    passkeys: 0.23
```

**INVARIANT: A `.liber` file is VALID and COMPLETE without `adoption_data`.** The system works without feedback. Decision receipts are optional and never block knowledge delivery. This invariant has been accidentally dropped twice during document evolution — it requires explicit protection.

**Privacy requirement:** When `adoption_data` is present, a `disclosure` field SHOULD be non-empty, describing data scope and known biases. No identifiable organization names in `adoption_data.distribution` keys — use option IDs only.

---

#### `known_failures`

- **Type:** `array` of `string`
- **Purpose:** Documented failure cases. What went wrong, and with which option.

```yaml
known_failures:
  - JWT token size exceeded 8KB causing HTTP 431 errors with AWS ALB (2024)
  - Session fixation vulnerability in Rails < 7.0 default session store
```

Borrowed from aviation incident databases. Learning from failures prevents repetition.

---

#### `uncertainty_sources`

- **Type:** `array` of `string`
- **Purpose:** Why confidence is not higher. Explicit acknowledgment of what we don't know.

```yaml
uncertainty_sources:
  - Passkeys browser support data is < 12 months old
  - No large-scale comparison study of JWT vs session performance exists
```

Borrowed from metrology: a measurement without stated uncertainty is incomplete.

---

#### `pre_decision_checklist`

- **Type:** `array` of `string`
- **Purpose:** Questions to answer BEFORE making a decision. From aviation CRM (Crew Resource Management).

```yaml
pre_decision_checklist:
  - What is your session revocation requirement?
  - What compliance frameworks apply?
  - What is your expected concurrent user count?
```

---

#### `related`

- **Type:** `object` with `see_also` array
- **Purpose:** Cross-references to related `.liber` files with context.

```yaml
related:
  see_also:
    - domain: session-management
      context: "Session store strategy depends on auth choice"
    - domain: api-security
      context: "Token validation middleware"
    - domain: oauth-flows
```

Each `see_also` entry:

| Field | Type | Required | Description |
|-------|------|:--------:|-------------|
| `domain` | string (kebab-case) | MUST | Domain identifier of the linked `.liber` file |
| `context` | string | SHOULD | Why these domains are related |

A `see_also` link is informational — it does not imply dependency or incompatibility. An option or file without related links is fully valid. If a `see_also` references a domain for which no `.liber` file exists, this MUST NOT cause a validation error — the link is a forward reference.

**Roadmap (v2.1+):** Stronger relation types (`requires`, `excludes`) will be introduced as optional fields within the `related` object. Existing `see_also` links remain valid indefinitely.

---

#### `freshness_events`

- **Type:** `array` of `enum`
- **Values:** `source_changed`, `major_version_released`, `community_flagged`, `source_unavailable`
- **Purpose:** Events that have triggered or should trigger a review.

```yaml
freshness_events:
  - major_version_released   # Passkeys WebAuthn Level 3 draft released
```

---

#### `decay_rate`

- **Type:** `number`
- **Purpose:** Override the default freshness decay rate for this domain.
- **Status:** [CONFIGURABLE, NOT NORMATIVE] — this is a per-installation setting, not a specification requirement.

---

### 4.5. Forward Tolerance

Parsers MUST ignore unknown fields without error. A parser built for v0.3 encountering a field from v0.5 MUST NOT break — it silently skips what it doesn't recognize.

This is Postel's Law: *"Be conservative in what you send, be liberal in what you accept."* It is the single most important compatibility rule in this specification. Violating it guarantees adoption failure (cf. XHTML 2.0: draconian error handling killed the format).

### 4.6. Extensions

Custom fields use the `x-` prefix:

```yaml
x-lukardi-sap-module: FI-CO
x-internal-review-status: approved
```

Extensions MUST NOT change the semantics of core fields. Parsers MUST ignore extensions they don't recognize. Extensions are pass-through — the spec does not validate them.

This pattern has 10+ years of proven success in OpenAPI.

---

## 5. Body Specification

### 5.1. Recommended Pattern

The body is Markdown. It is NOT schema-enforced. The following is a RECOMMENDED structure, not a requirement. Files with a different body layout are valid.

The body follows a **progressive disclosure** pattern — four layers of increasing depth:

| Layer | Name | Time | Status |
|-------|------|------|--------|
| **L0** | Frontmatter | Instant | Schema-enforced (§4) |
| **L1** | Quick Decision | ~30 seconds | RECOMMENDED |
| **L2** | Full Landscape | ~15 minutes | RECOMMENDED |
| **L3** | Deep Reference | Unbounded | OPTIONAL |

---

#### L1: Quick Decision

The first thing a reader sees after the frontmatter. Two subsections:

**Quick Pick** — a 3–5 sentence summary. "If you're in a hurry, here's the lay of the land." Name the default choice for the most common context, then immediately name the exception.

**Critical Exclusions** — the NEVER list, expanded from frontmatter with brief rationale.

```markdown
## Quick Decision

Most teams building stateless microservices should start with **JWT**. 
If you need per-session revocation or operate under strict compliance 
requirements (HIPAA, PCI-DSS), use **server-side sessions** instead.

### Critical Exclusions

- **NEVER** use basic auth over unencrypted HTTP — credentials are sent 
  in plaintext on every request.
- **NEVER** store passwords in plaintext or reversible encryption.
```

---

#### L2: Full Landscape

The core of the file. Contains:

**Pivotal Points** — discriminating questions that branch the decision tree. Each question names the options it leads to.

**Options** — each option gets a subsection with: `when_to_use`, `when_not_to_use`, `pros`, `cons`, and optional `confidence`. One paragraph per option, 3–5 sentences maximum. Link to source documentation for details — don't repeat it.

**What This File Adds** — a RECOMMENDED subsection explicitly articulating what knowledge this file provides *beyond* the official documentation of the tools discussed. This is the structural answer to the "sole source test": if you can't articulate what's new here, the file may be redundant.

```markdown
## Full Landscape

### Pivotal Points

**Do you need to revoke individual sessions?**
- Yes → session-cookies, token-blacklist
- No → jwt-stateless, api-keys

### Options

#### JWT Stateless Authentication

**When to use:** Microservices architectures, cloud-native deployments, 
horizontal scaling requirements.

**When NOT to use:** Applications requiring instant session revocation. 
Monoliths with server-side rendering where session cookies are simpler.

**Pros:** Horizontal scaling without shared state. Works across domains. 
Broadly supported.

**Cons:** Token size can cause issues with header limits. Revocation 
requires additional infrastructure (blacklists, short expiry + refresh).

#### Passkeys / WebAuthn

[...]

### What This File Adds

Official OWASP and Auth0 docs cover individual implementation guides. 
This file adds: cross-cutting trade-off comparison, context-dependent 
recommendations (team size × compliance × architecture), and the 
critical exclusions list synthesized from incident databases.
```

---

#### L3: Deep Reference

Optional. Links, citations, related patterns, extended analysis for the reader who wants to go deeper.

```markdown
## Deep Reference

### Related Patterns

- [session-management](./session-management.md) — session lifecycle, 
  timeout strategies, concurrent session limits
- [api-security](./api-security.md) — API key management, rate limiting, 
  OAuth scopes

### Sources

1. OWASP Authentication Cheat Sheet — https://cheatsheetseries.owasp.org/...
2. Auth0 Architecture Scenarios — https://auth0.com/docs/...
```

---

### 5.2. Anti-Bloat Guidelines

Seven rules to keep `.liber` files lean. These apply to both human authors and AI generators. Each rule includes a concrete before/after example.

**Rule 1: Non-inferable details only.** Don't repeat information available in the tool's official documentation. Link instead. Test: *"Could an AI agent find this in 3 web searches?"* If yes → don't repeat it.

```markdown
# ❌ BAD — repeats official docs
#### Kafka
Kafka is a distributed event streaming platform. It uses a publish-subscribe 
model with topics, partitions, and consumer groups. Messages are stored on 
disk and replicated across brokers for fault tolerance. You can install it 
via `brew install kafka` or download from kafka.apache.org...

# ✅ GOOD — adds what docs DON'T cover
#### Kafka
**When to use:** Event-driven architectures needing >50K msgs/sec sustained 
throughput, event sourcing, real-time stream processing pipelines.
**When NOT to use:** Simple task queues (<1K msgs/sec) — Kafka's operational 
overhead (ZooKeeper/KRaft, partition management) isn't justified. Use RabbitMQ.
```

**Rule 2: Zero verbose rationale in .liber files.** The body explains *what* and *when*, not *why the format works this way*. Rationale lives here in the spec (for authors), not in `.liber` files (for consumers).

```markdown
# ❌ BAD — explains the format inside the file
This file uses progressive disclosure because research shows that developers 
make better decisions when information is layered. The confidence score of 
0.85 reflects our assessment methodology based on source credibility...

# ✅ GOOD — just delivers the knowledge
**Confidence:** 0.85 — based on OWASP (2026) and Auth0 architecture guides.
```

**Rule 3: Frontmatter ≤ 200 tokens.** Flat options (id + name + status). Full trade-offs in body only.

```yaml
# ❌ BAD — bloated frontmatter (~320 tokens)
options:
  - id: jwt-stateless
    name: JWT Stateless Authentication
    status: mainstream
    confidence: 0.85
    when_to_use: [Microservices, Cloud-native, Horizontal scaling]
    when_not_to_use: [Monolith with SSR, Apps needing instant revocation]
    pros: [Horizontal scaling, Stateless, Cross-domain]
    cons: [Token size, Revocation complexity, Token theft window]

# ✅ GOOD — flat frontmatter (~40 tokens), details in body
options:
  - id: jwt-stateless
    name: JWT Stateless Authentication
    status: mainstream
```

**Rule 4: Body ≤ 5,000 tokens (SHOULD).** If you exceed this, consider splitting into two files by subdomain. A file that covers everything covers nothing well.

```markdown
# ❌ BAD — one file covering all of "security"
domain: security
# 12,000 tokens covering auth, encryption, network, compliance, logging...

# ✅ GOOD — split by decision landscape
domain: auth-patterns          # ~3,000 tokens
domain: encryption-at-rest     # ~2,500 tokens
domain: api-security           # ~2,800 tokens
```

**Rule 5: One option = one paragraph.** Maximum 3–5 sentences per option in the Full Landscape. Deep details → link to the source.

```markdown
# ❌ BAD — essay per option
#### JWT Stateless Authentication
JSON Web Tokens were introduced in RFC 7519 and have since become the 
dominant approach for stateless authentication in modern web applications.
The token consists of three parts: header, payload, and signature. The 
header typically specifies the algorithm (HS256 or RS256) and token type.
The payload contains claims such as sub, iat, exp, and custom claims...
[continues for 500 more words]

# ✅ GOOD — decision-relevant summary, link for details
#### JWT Stateless Authentication
**When to use:** Microservices, cloud-native, horizontal scaling.
**When NOT to use:** Instant session revocation. Monoliths with SSR.
**Pros:** Scales without shared state. Cross-domain. Broadly supported.
**Cons:** Token size (HTTP 431 risk). Revocation needs blacklist infra.
See: [RFC 7519](https://tools.ietf.org/html/rfc7519), [Auth0 JWT guide](https://auth0.com/docs/secure/tokens/json-web-tokens).
```

**Rule 6: No frontmatter↔body duplication.** What's in the frontmatter (options list, context_filters) is NOT repeated verbatim in the body. The body *expands*, it doesn't *echo*.

```markdown
# ❌ BAD — body echoes frontmatter
## Context Filters
- team_size: solo, small, medium, large
- architecture: monolith, microservices, serverless
- compliance: none, SOC2, HIPAA, PCI-DSS

# ✅ GOOD — body USES the filters, doesn't restate them
## Context-Dependent Recommendations
**Small team + monolith + no compliance:** Session cookies. Simplest path.
**Large team + microservices + SOC2:** JWT with token blacklist + audit log.
**Any team + HIPAA:** Server-side sessions with encrypted store. No exceptions.
```

**Rule 7: "Would I tweet this?" test.** Every sentence in the body should carry decision value. If you wouldn't share it as an insight — cut it.

```markdown
# ❌ BAD — filler sentence
Authentication is an important aspect of modern web applications that 
developers need to consider carefully when building their systems.

# ✅ GOOD — insight worth sharing  
Session cookies are simpler than JWT for monoliths — but the moment you 
add a second service, you pay for shared session state anyway.
```

---

## 6. Confidence & Freshness

### 6.1. Confidence Model

Confidence measures how well-supported the file's knowledge is by its cited sources. It is a *research quality* metric, not a prediction of correctness.

**Accepted formats:**

| Format | Example | Precision | Recommended for |
|--------|---------|-----------|-----------------|
| Number | `0.85` | ±0.05 | AI engines, automated generation |
| Enum | `high` | ±0.15 | Human authors (more honest) |
| Null | `null` | N/A | Unassessed (valid, treated as warning) |

Numbers use a step of 0.05 (0.50, 0.55, 0.60, ... 0.90, 0.95). Finer granularity is pseudoprecision — nobody can distinguish 0.72 from 0.78. The instrument (expert judgment or AI assessment) has precision of ±0.1 at best.

### 6.2. Freshness: Event-Driven Primary, Time-Based Fallback

Knowledge does not decay by calendar. It decays when the world changes.

**Primary mechanism — event-driven:**

| Event | Effect | Detection |
|-------|--------|-----------|
| Source URL returns 404 | Immediate confidence concern | lychee link checker in CI |
| Source content changed significantly | Review triggered | RSS, webhooks, periodic fetch + diff |
| Major version of a discussed tool released | Options landscape may have shifted | GitHub release monitoring |
| Community flag (GitHub issue filed) | Review triggered | Issue tracker |

**Secondary mechanism — time-based fallback:**

When event monitoring is unavailable, time since `verified` serves as a fallback signal:

| Time Since Verified | Signal | Action |
|--------------------|--------|--------|
| < 180 days | Fresh | None |
| 180–365 days | ⚠️ Aging | Warning displayed to consumers |
| > 365 days | 🟡 Review required | File flagged for re-verification |

**Decay formula:** This specification intentionally does NOT define a normative decay formula. No empirical research has measured the rate at which decision-point knowledge becomes stale. Proposed formulas (exponential decay, domain-tiered rates) are placeholders without data. Implementations MAY use configurable decay rates per domain. The `decay_rate` field exists for this purpose. Values are [CONFIGURABLE, NOT NORMATIVE].

### 6.3. Invariants

Five rules that MUST NEVER be violated, regardless of spec version:

1. **Two confidences never mix.** `confidence` (research) and `adoption_data` (empirical) are separate measurements with separate semantics.
2. **Maximum 0.95.** Confidence MUST NOT equal 1.0. There is always uncertainty.
3. **Feedback is optional.** `adoption_data` is MAY. A file is VALID and COMPLETE without it. The system works without feedback.
4. **Privacy by default.** When `adoption_data` is present, `disclosure` SHOULD describe data scope and known biases. No identifiable organization or person names in adoption data — use option IDs and anonymized aggregates only. The format never trades knowledge access for personal data.
5. **Orient, not decide.** No YAML field and no Markdown section MUST contain directives such as "recommended," "best option," or "you should use." Permitted framing: "safe starting point," "most common choice," "default for 80% of cases." A `.liber` file presents a landscape — the actor decides.

### 6.4. ADCL Compatibility — How TRUST Reads FORMAT

The ADCL layer (Agent Decision Certification) computes a Decision IQ Score by reading specific fields from `.liber` files. This table documents which spec fields feed which certification metrics:

| # | Decision IQ Metric | Weight | Reads from `.liber` field(s) |
|:-:|-------------------|:------:|------------------------------|
| ① | **Option Coverage** | 20% | `options` (array length) |
| ② | **Trade-off Awareness** | 20% | Body: `pros`, `cons` per option |
| ③ | **Context Fit** | 20% | `context_filters` |
| ④ | **Source Verification** | 15% | `sources` |
| ⑤ | **Emerging Awareness** | 10% | `options` where `status: emerging` |
| ⑥ | **Critical Exclusion** | 15% | `critical_exclusions` |

**Decision IQ = 100 × (0.20×① + 0.20×② + 0.20×③ + 0.15×④ + 0.10×⑤ + 0.15×⑥)**

Weights are [DRAFT] — pending empirical validation from ADCL pilots (Q3 2026). The field-to-metric mapping is stable.

**Implication for authors:** Files with `context_filters`, `critical_exclusions`, and options with honest trade-offs enable full Decision IQ scoring. Files with only MUST fields enable partial scoring (metrics ①②④).

---

## 7. Validation

### 7.1. Schema Validation

Frontmatter is validated against a JSON Schema (Draft 2020-12). The complete schema is in [Appendix A](#appendix-a-json-schema).

Key validation behaviors:

| Condition | Result |
|-----------|--------|
| Missing MUST field | ❌ Error — file is invalid |
| Missing SHOULD field | ⚠️ Warning — file is valid but diminished |
| Missing MAY field | ✅ Silent — no message |
| Unknown field | ✅ Ignored — forward tolerance |
| `confidence: 1.0` | ❌ Error — violates max 0.95 invariant |
| `confidence: null` | ⚠️ Warning — valid but flagged |
| `sources` with < 2 items | ❌ Error — minimum 2 required |
| `options` with < 2 items | ⚠️ Warning — SHOULD have ≥ 2 |

### 7.2. CI/CD Quality Gates

Recommended pipeline using generic tooling only (no custom tools required):

```yaml
# .github/workflows/liber-validate.yml
name: Validate .liber files
on: [push, pull_request]
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node
        uses: actions/setup-node@v4
        with: { node-version: 20 }
      
      - name: Setup Python
        uses: actions/setup-python@v5
        with: { python-version: '3.12' }
      
      # Schema validation (generic: ajv)
      - name: Validate frontmatter
        run: |
          npm install -g ajv-cli ajv-formats
          pip install python-frontmatter --break-system-packages
          python3 scripts/extract-frontmatter.py
          for f in .liber-fm/*.json; do
            ajv validate -s liber-schema.json -d "$f" --spec=draft2020 -c ajv-formats
          done
      
      # Link checking (generic: lychee)
      - name: Check source URLs
        uses: lycheeverse/lychee-action@v2
        with:
          args: --no-progress --accept 200,204,301,302 'knowledge/**/*.md'
          fail: true
      
      # Freshness check (generic script — see below)
      - name: Check freshness
        run: python3 scripts/check-freshness.py --warn-days 180 --fail-days 365

      # Token budget check (generic script — see below)
      - name: Check token budget  
        run: |
          pip install tiktoken --break-system-packages
          python3 scripts/check-tokens.py --fm-warn 200 --fm-fail 500
```

#### Companion Scripts

**`scripts/extract-frontmatter.py`** — Extracts YAML frontmatter into JSON for ajv validation:

```python
#!/usr/bin/env python3
"""Extract YAML frontmatter from .liber/.md files into JSON for ajv validation."""
import frontmatter, json, pathlib, sys, os

os.makedirs(".liber-fm", exist_ok=True)
files = list(pathlib.Path("knowledge").rglob("*.md"))
if not files:
    print("No .md files found in knowledge/"); sys.exit(0)

for path in files:
    post = frontmatter.load(path)
    if not post.metadata:
        continue
    out = pathlib.Path(".liber-fm") / f"{path.stem}.json"
    out.write_text(json.dumps(post.metadata, default=str, indent=2))
    print(f"  extracted: {path} → {out}")
```

**`scripts/check-freshness.py`** — Flags stale files based on `verified` date:

```python
#!/usr/bin/env python3
"""Check freshness of .liber files. Exit 1 if any file exceeds --fail-days."""
import frontmatter, pathlib, argparse, sys
from datetime import date, timedelta

parser = argparse.ArgumentParser()
parser.add_argument("--warn-days", type=int, default=180)
parser.add_argument("--fail-days", type=int, default=365)
args = parser.parse_args()

today = date.today()
failures = []

for path in pathlib.Path("knowledge").rglob("*.md"):
    post = frontmatter.load(path)
    verified = post.get("verified")
    if not verified:
        continue
    if isinstance(verified, str):
        verified = date.fromisoformat(verified)
    age = (today - verified).days
    
    if age > args.fail_days:
        print(f"  ❌ STALE ({age}d): {path}")
        failures.append(path)
    elif age > args.warn_days:
        print(f"  ⚠️  AGING ({age}d): {path}")
    else:
        print(f"  ✅ FRESH ({age}d): {path}")

if failures:
    print(f"\n{len(failures)} file(s) exceed {args.fail_days}-day threshold.")
    sys.exit(1)
```

**`scripts/check-tokens.py`** — Enforces token budgets on frontmatter and body:

```python
#!/usr/bin/env python3
"""Check token budgets for .liber files. Uses tiktoken (cl100k_base)."""
import frontmatter, pathlib, argparse, sys, tiktoken

parser = argparse.ArgumentParser()
parser.add_argument("--fm-warn", type=int, default=200)
parser.add_argument("--fm-fail", type=int, default=500)
parser.add_argument("--body-warn", type=int, default=5000)
args = parser.parse_args()

enc = tiktoken.get_encoding("cl100k_base")
failures = []

for path in pathlib.Path("knowledge").rglob("*.md"):
    post = frontmatter.load(path)
    if not post.metadata:
        continue
    
    import yaml
    fm_text = yaml.dump(post.metadata, default_flow_style=False)
    fm_tokens = len(enc.encode(fm_text))
    body_tokens = len(enc.encode(post.content))
    
    status = "✅"
    if fm_tokens > args.fm_fail:
        status = "❌"
        failures.append(f"{path}: FM={fm_tokens} tokens (max {args.fm_fail})")
    elif fm_tokens > args.fm_warn:
        status = "⚠️ "
    
    body_flag = " ⚠️  BODY" if body_tokens > args.body_warn else ""
    print(f"  {status} {path}: FM={fm_tokens}tok, Body={body_tokens}tok{body_flag}")

if failures:
    print(f"\n{len(failures)} file(s) exceed FM token hard limit:")
    for f in failures:
        print(f"  {f}")
    sys.exit(1)
```

All four scripts are generic Python using standard packages (python-frontmatter, tiktoken, pyyaml). No custom `.liber`-specific tooling. Copy them into `scripts/`, install deps, done.

---

## 8. Examples

### 8.1. Example 1: Bare Minimum

The simplest valid `.liber` file. Four MUST fields, minimal body.

```yaml
---
domain: message-queue-selection
verified: 2026-03-15
sources:
  - https://docs.confluent.io/platform/current/platform-quickstart.html
  - https://www.rabbitmq.com/docs/tutorials
confidence: medium
---

## Quick Decision

For event-driven microservices needing high throughput, start with **Kafka**.
For task queues with routing flexibility, use **RabbitMQ**.

## Options

### Kafka
**When to use:** High-throughput event streaming, log aggregation, real-time pipelines.
**When NOT to use:** Simple task queues, low-volume applications where operational overhead isn't justified.

### RabbitMQ
**When to use:** Task distribution, complex routing patterns, request-reply workflows.
**When NOT to use:** High-throughput event streaming (>100K msgs/sec sustained).
```

This file is **15 lines of frontmatter + 15 lines of body**. It is valid, useful, and complete. Everything else is enhancement.

---

### 8.2. Example 2: Recommended

A well-developed `.liber` file using SHOULD fields.

```yaml
---
domain: auth-patterns
verified: 2026-03-29
sources:
  - url: https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
    type: standard
  - url: https://auth0.com/docs/get-started/architecture-scenarios
    type: vendor
  - url: https://webauthn.guide/
    type: community
confidence: 0.85
schema_version: "0.4"
version: 1.0.0
generation_method: ai_assisted
options:
  - id: jwt-stateless
    name: JWT Stateless Authentication
    status: mainstream
  - id: session-cookies
    name: Server-Side Sessions
    status: mainstream
  - id: passkeys
    name: Passkeys / WebAuthn
    status: emerging
  - id: api-keys
    name: API Keys
    status: mainstream
context_filters:
  team_size: [solo, small, medium, large]
  architecture: [monolith, microservices, serverless]
  compliance: [none, SOC2, HIPAA, PCI-DSS]
critical_exclusions:
  - NEVER use basic auth over unencrypted HTTP
  - NEVER store passwords in plaintext or reversible encryption
  - NEVER disable CSRF protection on authentication endpoints
emerging: Passkeys gaining rapid traction; WebAuthn Level 3 draft in progress
pivotal_points:
  - question: Do you need to revoke individual sessions?
    if_yes: [session-cookies, token-blacklist]
    if_no: [jwt-stateless, api-keys]
  - question: Is phishing your primary authentication threat?
    if_yes: [passkeys, hardware-keys]
    if_no: [any]
related:
  see_also:
    - domain: session-management
      context: "Session store strategy depends on auth choice"
    - domain: api-security
      context: "Token validation middleware"
    - domain: oauth-flows
---

## Quick Decision

Most teams building stateless microservices should start with **JWT**. If you need 
per-session revocation or operate under HIPAA/PCI-DSS, use **server-side sessions**. 
Watch **passkeys** — they're the strongest phishing defense and adoption is accelerating.

### Critical Exclusions

- **NEVER** use basic auth over unencrypted HTTP — credentials sent in plaintext.
- **NEVER** store passwords in plaintext or reversible encryption.
- **NEVER** disable CSRF protection on authentication endpoints.

## Full Landscape

### Pivotal Points

**Do you need to revoke individual sessions?**  
Yes → server-side sessions or JWT with blacklist.  
No → JWT stateless or API keys.

**Is phishing your primary threat?**  
Yes → passkeys or hardware security keys.  
No → any option fits.

### Options

#### JWT Stateless Authentication
**Status:** Mainstream | **Confidence:** 0.85

**When to use:** Microservices, cloud-native, horizontal scaling.  
**When NOT to use:** Applications requiring instant session revocation. Monoliths 
where session cookies are simpler.

**Pros:** Scales horizontally without shared state. Cross-domain. Broadly supported.  
**Cons:** Token size (risk of HTTP 431 with ALB). Revocation requires blacklist 
infrastructure. Token theft window equals token lifetime.

#### Server-Side Sessions
**Status:** Mainstream | **Confidence:** 0.85

**When to use:** Monoliths, apps needing instant revocation, strict compliance.  
**When NOT to use:** Microservices without shared session store. Cross-domain SSO.

**Pros:** Instant revocation. Small cookie size. Server controls all state.  
**Cons:** Requires shared store for horizontal scaling. Sticky sessions or Redis cluster.

#### Passkeys / WebAuthn
**Status:** Emerging | **Confidence:** 0.75

**When to use:** Consumer apps prioritizing UX and phishing resistance.  
**When NOT to use:** Internal tools where browser support gaps matter. Systems 
requiring programmatic auth (service-to-service).

**Pros:** Phishing-resistant by design. Better UX than passwords. No shared secrets.  
**Cons:** Browser support still uneven. Account recovery complexity. Requires 
resident credential storage on user device.

#### API Keys
**Status:** Mainstream | **Confidence:** 0.80

**When to use:** Service-to-service auth, developer APIs, webhook callbacks.  
**When NOT to use:** User-facing authentication. Any context where key rotation 
isn't automated.

**Pros:** Simple. No session state. Easy to generate and revoke per consumer.  
**Cons:** No identity federation. Key sprawl. Must be transmitted securely.

### What This File Adds

OWASP and Auth0 docs cover individual implementation. This file adds: cross-cutting 
comparison across four auth patterns, context-dependent filtering (team size × 
compliance × architecture), critical exclusion list from incident data, and the 
passkeys emerging signal that isn't in any single tool's documentation.

## Deep Reference

See `sources` in frontmatter. Related: [session-management](./session-management.md), 
[api-security](./api-security.md), [oauth-flows](./oauth-flows.md).
```

---

### 8.3. Example 3: Edge Case — High Uncertainty

A file showing the format under stress: low confidence, deprecated options, explicit uncertainty.

```yaml
---
domain: frontend-meta-framework-selection
verified: 2026-02-10
sources:
  - url: https://2025.stateofjs.com/en-US/libraries/meta-frameworks/
    type: community
  - url: https://docs.astro.build/en/concepts/why-astro/
    type: official_docs
  - url: https://survey.stackoverflow.co/2025/#most-popular-technologies-webframe
    type: community
confidence: 0.45
schema_version: "0.4"
generation_method: ai_generated
options:
  - id: nextjs
    name: Next.js (App Router)
    status: mainstream
  - id: remix
    name: Remix
    status: declining
  - id: astro
    name: Astro
    status: emerging
  - id: sveltekit
    name: SvelteKit
    status: emerging
uncertainty_sources:
  - Next.js App Router stability perception varies widely across community
  - Remix merger with React Router blurs category boundaries
  - Framework benchmarks are unreliable across real-world workloads
known_failures:
  - Next.js App Router migration caused 6-month delay at Company X (2025, public postmortem)
  - Remix v2 breaking changes lost significant community trust
emerging: Astro and SvelteKit growing fastest in 2025 State of JS survey
---

## Quick Decision

This is a **fast-moving domain with low confidence** (0.45). The landscape shifted 
significantly in 2025 and may shift again. Treat all recommendations as provisional.

If building a content-heavy site: **Astro**. If building a full-stack app with React: 
**Next.js**, but budget for App Router learning curve. If using Svelte: **SvelteKit**.

### Critical Exclusions

- **NEVER** migrate a production app to Next.js App Router mid-sprint — budget a 
  dedicated migration cycle. Breaking changes between Pages Router and App Router 
  are substantial.
- **NEVER** choose a framework based solely on benchmarks — real-world performance 
  depends on your data fetching patterns, not synthetic tests.

## Full Landscape

### Pivotal Points

**Is your site primarily content or application?**  
Content (blog, docs, marketing) → Astro (island architecture, zero JS by default).  
Application (dashboard, SaaS, interactive) → Next.js or SvelteKit.

**Are you committed to React?**  
Yes → Next.js (largest ecosystem, most hiring options).  
No → SvelteKit (better DX scores in surveys, smaller bundle).

### Options

#### Next.js (App Router)
**Status:** Mainstream | **Confidence:** 0.50

**When to use:** Full-stack React apps, projects needing largest ecosystem.  
**When NOT to use:** Content-heavy sites (Astro ships less JS). Teams unable to 
invest in App Router learning curve.

**Pros:** Largest ecosystem. Vercel deployment integration. RSC support.  
**Cons:** App Router mental model shift. Frequent breaking changes between 
minor versions. Vendor coupling concerns with Vercel-specific optimizations.

#### Remix
**Status:** Declining | **Confidence:** 0.35

**When to use:** Projects already invested in Remix. Teams valuing web standards.  
**When NOT to use:** New projects — Remix merged into React Router v7, 
blurring its identity. Future direction uncertain.

**Pros:** Web-standards-first (loaders, actions). Progressive enhancement.  
**Cons:** Identity crisis post-React Router merge. Shrinking community. 
Core team bandwidth split across projects.

#### Astro
**Status:** Emerging | **Confidence:** 0.55

**When to use:** Content sites, documentation, marketing, blogs. Any project 
where most pages don't need client-side interactivity.  
**When NOT to use:** Highly interactive applications (dashboards, editors).

**Pros:** Island architecture — zero JS by default, hydrate only what needs it. 
Framework-agnostic (use React, Svelte, Vue components together). Fast builds.  
**Cons:** Not designed for complex application state. Smaller ecosystem than Next.js.

#### SvelteKit
**Status:** Emerging | **Confidence:** 0.50

**When to use:** Teams open to non-React. Projects prioritizing DX and bundle size.  
**When NOT to use:** Projects requiring large React component ecosystem. 
Enterprise teams where React hiring pool matters.

**Pros:** Compiled framework — smaller bundles. Highest DX satisfaction in surveys. 
Built-in form actions and progressive enhancement.  
**Cons:** Smaller ecosystem. Fewer third-party components. Runes migration (Svelte 5) 
adds transition cost.

### What This File Adds

Individual framework docs sell their own product. This file adds: honest cross-framework 
comparison including declining options, uncertainty acknowledgment (confidence 0.45), 
known failure cases from production postmortems, and the critical exclusion about 
mid-sprint migration that no framework's docs will mention.

## Deep Reference

- State of JS 2025: https://2025.stateofjs.com/en-US/libraries/meta-frameworks/
- Stack Overflow Survey 2025: https://survey.stackoverflow.co/2025/
- Astro Island Architecture: https://docs.astro.build/en/concepts/islands/
```

This example demonstrates:
- `confidence: 0.45` — explicitly low, with `uncertainty_sources` explaining why.
- `status: declining` — Remix is flagged honestly.
- `generation_method: ai_generated` — transparent about AI authorship.
- `known_failures` — learning from what went wrong.
- The Quick Decision opens with a caveat about low confidence.

---

## 9. Anti-Patterns

Explicit list of what NOT to do. Learning from failures is faster than learning from examples.

### 9.1. File Anti-Patterns

| Anti-Pattern | Why It's Wrong | Fix |
|-------------|---------------|-----|
| **Confidence 1.0** | Claims impossible certainty. Violates spec. | Use max 0.95. If you're tempted to write 1.0, you haven't thought about uncertainty hard enough. |
| **Single option** | A file with one option is a recommendation, not a landscape. | Add alternatives. If none exist, this topic doesn't need a `.liber` file. |
| **Missing when_not_to_use** | Every option has contexts where it's wrong. Omitting this is dishonest. | Add at least one when_not_to_use per option. |
| **Stale without flag** | `verified: 2024-01-15` with no freshness warning = silent rot. | CI/CD catches this. Fix: re-verify or flag as aging. |
| **Mixing confidences** | Averaging research confidence with adoption data. | Keep `confidence` and `adoption_data` in separate fields with separate semantics. |
| **Repeating official docs** | Copying the getting-started guide into the body. | Link to docs. Write only what the docs don't cover: trade-offs, comparisons, when_not_to_use. |
| **Frontmatter↔body duplication** | Full pros/cons in both frontmatter AND body. | Flat options in frontmatter (id + name + status). Full trade-offs in body only. |
| **Generic domain name** | `domain: security` — matches everything, helps nothing. | Be specific: `domain: auth-patterns`, `domain: encryption-at-rest`. |

#### Anti-Pattern Examples: Before → After

**AP1: Confidence 1.0**

```yaml
# ❌ INVALID — will fail schema validation
confidence: 1.0

# ✅ VALID — honest, leaves room for uncertainty
confidence: 0.90
# or even better for human authors:
confidence: high
```

**AP2: Single option file**

```yaml
# ❌ NOT A LANDSCAPE — this is a recommendation
domain: container-orchestration
options:
  - id: kubernetes
    name: Kubernetes
    status: mainstream

# ✅ LANDSCAPE — presents actual choices
domain: container-orchestration
options:
  - id: kubernetes
    name: Kubernetes
    status: mainstream
  - id: docker-swarm
    name: Docker Swarm
    status: declining
  - id: ecs
    name: AWS ECS
    status: mainstream
  - id: nomad
    name: HashiCorp Nomad
    status: emerging
```

**AP3: Mixing two confidences**

```yaml
# ❌ INVARIANT VIOLATION — blended score is meaningless
confidence: 0.78  # "average of research (0.85) and adoption (0.71)"

# ✅ CORRECT — separate measurements, separate fields
confidence: 0.85
adoption_data:
  total_decisions: 412
  period: 2026-Q1
  distribution:
    kubernetes: 0.71
    ecs: 0.22
    nomad: 0.07
```

**AP4: Stale file without signal**

```yaml
# ❌ SILENT ROT — 14 months old, no one knows
domain: llm-provider-selection
verified: 2024-11-15
confidence: 0.80
# body still recommends GPT-4 as "latest"...

# ✅ AGING VISIBLE — CI flags it, freshness events tracked
domain: llm-provider-selection
verified: 2024-11-15
confidence: 0.80
freshness_events:
  - major_version_released   # GPT-5 launched since verified
  - source_changed           # OpenAI pricing page updated
# CI/CD: ⚠️ AGING (497 days since verified) — review required
```

**AP5: Repeating official docs**

Same principle as Anti-Bloat Rule 1 (§5.2). See the Kafka before/after example there. The test: *"Could an agent find this in 3 web searches?"* If yes — link, don't repeat.

### 9.2. Specification Anti-Patterns (Avoided by Design)

| Anti-Pattern | How We Avoided It | Reference |
|-------------|-------------------|-----------|
| **Draconian error handling** (XHTML 2.0) | Missing SHOULD field = warning, not error | X1 |
| **Backward incompatibility** (XHTML 2.0 → HTML5) | Migration contract: new parser MUST read old files | A9 |
| **Over-engineering** (DITA) | ≤15 top-level fields. 4 MUST. | X3 |
| **Custom parser requirement** (proprietary formats) | gray-matter + ajv = generic, zero custom tooling | X5 |
| **Vendor lock-in** (platform-dependent formats) | Files on disk. CC BY-SA 4.0. Any YAML parser works. | X9 |
| **Content sprawl** (Confluence wikis) | `verified` + freshness events + confidence decay | M1 |

---

## 10. Schema Versioning & Migration

### 10.1. Compatibility Contract

**Backward compatibility:** A parser for version N MUST read files from version N-1 without error. This is non-negotiable.

**Forward tolerance:** A parser for version N encountering a field from version N+1 MUST ignore it without error. (§4.5)

### 10.2. Breaking Changes

Breaking changes (removing a MUST field, changing field semantics) are permitted ONLY in major version increments and ONLY during v0.x. After v1.0, the four MUST fields are frozen.

### 10.3. Migration Path

Adding a new SHOULD field is NOT a breaking change — old files remain valid.

Promoting a MAY field to SHOULD is NOT a breaking change — old files get a new warning, not an error.

Promoting a SHOULD field to MUST IS a breaking change — only in major versions.

---

## 11. Regulatory Compatibility

`.liber` format naturally supports EU AI Act (Regulation EU 2024/1689) transparency requirements through:

- **`sources`** — traceable provenance of knowledge claims.
- **`confidence`** — explicit uncertainty quantification.
- **`generation_method`** — machine-readable labeling of AI-generated content (Art. 50 alignment).
- **`options` with trade-offs** — explainable decision support.

EU AI Act Article 50 transparency obligations become enforceable on **August 2, 2026**. The Code of Practice (final version expected June 2026) will establish technical standards for marking AI-generated content. `.liber` files with `generation_method` set are proactively aligned with these requirements.

This is awareness, not legal advice. Consult legal counsel for compliance obligations specific to your deployment.

---

## Appendix A: JSON Schema

JSON Schema Draft 2020-12 for `.liber` frontmatter validation.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://useliber.com/schema/liber-v0.4.json",
  "title": ".liber Frontmatter Schema",
  "description": "Validates YAML frontmatter of .liber decision knowledge files. Version 0.4.0-draft.",
  "type": "object",

  "required": ["domain", "verified", "sources", "confidence"],

  "properties": {

    "domain": {
      "type": "string",
      "pattern": "^[a-z0-9][a-z0-9-]*[a-z0-9]$",
      "minLength": 3,
      "maxLength": 64,
      "description": "Unique identifier for the decision landscape. Lowercase, hyphenated."
    },

    "verified": {
      "type": "string",
      "format": "date",
      "description": "ISO 8601 date of last content verification."
    },

    "sources": {
      "type": "array",
      "items": {
        "oneOf": [
          {
            "type": "string",
            "format": "uri",
            "description": "Short form — plain URL string."
          },
          {
            "type": "object",
            "required": ["url"],
            "properties": {
              "url": {
                "type": "string",
                "format": "uri"
              },
              "type": {
                "type": "string",
                "enum": ["standard", "vendor", "community", "academic", "official_docs"]
              },
              "title": {
                "type": "string"
              },
              "accessed": {
                "type": "string",
                "format": "date"
              }
            },
            "additionalProperties": false,
            "description": "Full form — source object with type taxonomy."
          }
        ]
      },
      "minItems": 2,
      "description": "Knowledge sources. Min 2. Accepts plain URLs or source objects with type."
    },

    "confidence": {
      "oneOf": [
        {
          "type": "number",
          "minimum": 0.0,
          "maximum": 0.95,
          "multipleOf": 0.05
        },
        {
          "type": "string",
          "enum": ["high", "medium", "low"]
        },
        {
          "type": "null"
        }
      ],
      "description": "Research confidence. Number (0.0-0.95, step 0.05), enum (high/medium/low), or null."
    },

    "schema_version": {
      "type": "string",
      "description": "Version of the .liber specification this file targets."
    },

    "version": {
      "type": "string",
      "description": "Version of this file's content."
    },

    "generation_method": {
      "type": "string",
      "enum": ["ai_generated", "ai_assisted", "human_authored"],
      "description": "How this file was created. EU AI Act Art.50 alignment."
    },

    "options": {
      "type": "array",
      "minItems": 2,
      "items": {
        "type": "object",
        "required": ["id", "name", "status"],
        "properties": {
          "id": {
            "type": "string",
            "pattern": "^[a-z0-9][a-z0-9-]*[a-z0-9]$"
          },
          "name": {
            "type": "string"
          },
          "status": {
            "type": "string",
            "enum": ["mainstream", "emerging", "declining", "deprecated"]
          }
        },
        "additionalProperties": false
      },
      "description": "Decision landscape options. Flat in frontmatter (id + name + status). Full trade-offs in body."
    },

    "context_filters": {
      "type": "object",
      "additionalProperties": {
        "type": "array",
        "items": { "type": "string" }
      },
      "description": "Contextual dimensions for filtering (team_size, architecture, compliance, etc.)."
    },

    "critical_exclusions": {
      "type": "array",
      "items": { "type": "string" },
      "description": "NEVER list — things you must not do."
    },

    "emerging": {
      "type": "string",
      "description": "Brief awareness signal about emerging trends in this domain."
    },

    "pivotal_points": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["question"],
        "properties": {
          "question": { "type": "string" },
          "if_yes": {
            "type": "array",
            "items": { "type": "string" }
          },
          "if_no": {
            "type": "array",
            "items": { "type": "string" }
          }
        }
      },
      "description": "Discriminating questions that branch the decision tree."
    },

    "related": {
      "type": "object",
      "properties": {
        "see_also": {
          "type": "array",
          "items": {
            "type": "object",
            "required": ["domain"],
            "properties": {
              "domain": {
                "type": "string",
                "pattern": "^[a-z0-9][a-z0-9-]*[a-z0-9]$"
              },
              "context": {
                "type": "string"
              }
            },
            "additionalProperties": false
          },
          "description": "Informational cross-references to related .liber domains."
        }
      },
      "additionalProperties": false,
      "description": "Cross-references to related .liber files. v1: see_also only. v2.1+: requires, excludes."
    },

    "freshness_events": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": ["source_changed", "major_version_released", "community_flagged", "source_unavailable"]
      },
      "description": "Events that trigger or have triggered a review."
    },

    "adoption_data": {
      "type": "object",
      "properties": {
        "total_decisions": { "type": "integer", "minimum": 0 },
        "period": { "type": "string" },
        "disclosure": { "type": "string" },
        "distribution": {
          "type": "object",
          "additionalProperties": {
            "type": "number",
            "minimum": 0,
            "maximum": 1
          }
        }
      },
      "description": "Empirical adoption data from decision receipts. OPTIONAL — file is valid without it."
    },

    "known_failures": {
      "type": "array",
      "items": { "type": "string" },
      "description": "Documented failure cases."
    },

    "uncertainty_sources": {
      "type": "array",
      "items": { "type": "string" },
      "description": "Why confidence is not higher."
    },

    "pre_decision_checklist": {
      "type": "array",
      "items": { "type": "string" },
      "description": "Questions to answer before making a decision."
    },

    "decay_rate": {
      "type": "number",
      "minimum": 0,
      "maximum": 1,
      "description": "[CONFIGURABLE, NOT NORMATIVE] Per-domain decay rate override."
    }
  },

  "allOf": [
    {
      "$comment": "Low confidence (≤0.45) demands explanation — require uncertainty_sources.",
      "if": {
        "properties": {
          "confidence": { "type": "number", "maximum": 0.45 }
        },
        "required": ["confidence"]
      },
      "then": {
        "required": ["uncertainty_sources"]
      }
    },
    {
      "$comment": "If any option has status:deprecated, known_failures SHOULD be present. Enforced as MUST for files with deprecated options.",
      "if": {
        "properties": {
          "options": {
            "type": "array",
            "contains": {
              "type": "object",
              "properties": { "status": { "const": "deprecated" } },
              "required": ["status"]
            }
          }
        },
        "required": ["options"]
      },
      "then": {
        "required": ["known_failures"]
      }
    },
    {
      "$comment": "If generation_method is ai_generated, sources must have at least 3 items — AI-only files need stronger sourcing.",
      "if": {
        "properties": {
          "generation_method": { "const": "ai_generated" }
        },
        "required": ["generation_method"]
      },
      "then": {
        "properties": {
          "sources": { "minItems": 3 }
        }
      }
    }
  ],

  "dependentRequired": {
    "adoption_data": ["confidence"],
    "decay_rate": ["verified"]
  },

  "additionalProperties": true
}
```

**Notes on the schema:**

- `additionalProperties: true` enforces forward tolerance — unknown fields are allowed.
- The `allOf` conditional rules enforce context-sensitive requirements: if confidence is ≤ 0.45, `uncertainty_sources` becomes required; if any option has `status: deprecated`, `known_failures` becomes required; if `generation_method` is `ai_generated`, at least 3 sources are required.
- `dependentRequired`: `adoption_data` requires `confidence` (to keep the two measurements co-present); `decay_rate` requires `verified` (decay needs an anchor date).
- `options` items have `additionalProperties: false` in frontmatter — full option details belong in the body, not here.
- **`related`** uses a nested object structure with `see_also` array. Each entry requires `domain` (kebab-case). `context` is optional. `additionalProperties: false` on the `related` object reserves the namespace for future relation types (`requires`, `excludes`).
- **Not expressible in schema:** The vendor-only anti-gaming rule (§4.2 — `vendor` sources MUST NOT be the sole source type) cannot be expressed in JSON Schema 2020-12 and SHOULD be enforced by CI/CD linting script. The neutrality invariant (§6.3 #5) requires content analysis beyond schema validation.

---

## Appendix B: Quick Reference Card

One page. Print it. Keep it next to your editor.

```
┌─────────────────────────────────────────────────────────────┐
│                    .liber QUICK REFERENCE                    │
│                     v0.4.0-draft                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  MUST (4 fields — file invalid without these):              │
│                                                             │
│    domain:      lowercase-hyphenated     # what this covers │
│    verified:    2026-03-29               # last check date  │
│    sources:     [{url, type}, ...]       # min 2, clickable │
│                 or [url1, url2]          # short form OK    │
│    confidence:  0.85 | high | null       # never 1.0        │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  SHOULD (add these for a good file):                        │
│                                                             │
│    options:             min 2, flat: id + name + status      │
│    context_filters:     keys → value arrays                  │
│    critical_exclusions: NEVER list                           │
│    pivotal_points:      branching questions                  │
│    version:             semver of this file                  │
│    emerging:            what's gaining traction               │
│    generation_method:   ai_generated | ai_assisted | human   │
│    schema_version:      "0.4"                                │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  MAY (optional — silence on absence):                       │
│                                                             │
│    related:             see_also: [{domain, context}]        │
│    adoption_data:       empirical data + disclosure           │
│    known_failures:      documented failure cases              │
│    uncertainty_sources: why confidence isn't higher           │
│    pre_decision_checklist: questions before deciding          │
│    freshness_events:    events triggering review              │
│    decay_rate:          per-domain override                   │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  BODY PATTERN (recommended, not enforced):                  │
│                                                             │
│    ## Quick Decision           (~30 sec read)                │
│       Quick Pick + Critical Exclusions                       │
│                                                             │
│    ## Full Landscape           (~15 min read)                │
│       Pivotal Points + Options + What This File Adds         │
│                                                             │
│    ## Deep Reference           (as needed)                   │
│       Sources, related patterns, extended analysis           │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  INVARIANTS (never violate):                                 │
│                                                             │
│    ✗ confidence = 1.0          (max 0.95)                    │
│    ✗ mixing research + adoption confidence                   │
│    ✗ requiring adoption_data   (always optional)             │
│    ✗ identifiable names in adoption_data (privacy)           │
│    ✗ "recommended" / "best option" language (neutrality)     │
│    ✗ requiring custom parser   (gray-matter + ajv only)      │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  TOKEN BUDGETS:                                              │
│                                                             │
│    Frontmatter:  SHOULD < 200 tokens, MUST NOT > 500         │
│    Body:         SHOULD < 5,000 tokens                       │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  NAMING CONVENTIONS:                                         │
│                                                             │
│    Files:   kebab-case.liber.md   YAML fields: snake_case    │
│    Options: kebab-case            Domains: kebab-case         │
│    Status:  lowercase enum        Indent: 2 spaces, no tabs  │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  FILE EXTENSION: .md (recommended) or .liber (optional)      │
│  ENCODING: UTF-8                                             │
│  PARSER: gray-matter (JS) | python-frontmatter (Py)         │
│  VALIDATION: ajv (JSON Schema Draft 2020-12)                 │
│  LICENSE: CC BY-SA 4.0                                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Appendix C: Design Decisions

Why the spec is designed the way it is — for future contributors who might ask "why not do X instead?"

**Why `confidence` max 0.95, not 1.0?** From metrology (NIST): every measurement has uncertainty. From epistemology: the map is not the territory. From hard experience: files with confidence 1.0 are never updated because "they're already perfect." The 0.95 ceiling forces every file to acknowledge residual uncertainty and remain open to revision.

**Why `sources` requires min 2, not 1?** A single source is one step from hallucination. Two independent sources make a claim defensible. Inspired by journalism's "two-source rule" — not sufficient for high confidence, but necessary for any confidence.

**Why Markdown body sections are ordered but free-form?** The order implements progressive disclosure (30 seconds → 5 minutes → 15 minutes). The free-form content preserves research quality. A rigid template ("exactly 3 pros, exactly 3 cons") kills nuance. The spec defines frames; content is jazz.

**Why `adoption_data` is MAY, not SHOULD?** The left side of the LIBER wheel (knowledge) works independently of the right side (feedback). A file without adoption data is complete and useful. Making it SHOULD would imply files without it are deficient — they're not.

**Why `additionalProperties: true` at the top level?** Postel's Law. Forward compatibility. A file created by spec v1.5 with new fields should be parseable by a v0.3 validator — unknown fields are silently ignored. This is the foundation of the migration contract.

**Why `sources` accept both string and object form?** Backward compatibility with v0.3.0 files that used plain URL strings. The object form (with `type` taxonomy) was added in v0.3.2. Accepting both means existing files don't break, and new files get richer metadata. The schema uses `oneOf` to validate either form.

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 0.4.0-draft | 2026-04-01 | **Surgical transplantation from 3 donor variants.** T1: §1.2 Genealogy rewritten as two-chain format (Chain I: knowledge democratization, Chain II: trust certification) with crossroads paragraph. T2: New §6.4 ADCL Compatibility — Decision IQ field mapping table connecting FORMAT to TRUST pillar. T3: §6.3 expanded from 3 to 5 invariants — added #4 Privacy by Default (adoption data anonymization + disclosure) and #5 Orient Not Decide (neutrality constraint). T4: `related_patterns` (flat string array) replaced with `related` field using `see_also` array with `{domain, context}` objects — enables knowledge graph construction. Schema updated. T5: New Appendix C — 6 Design Decisions explaining WHY key spec choices were made. T6: New §3.6 Naming Conventions — consolidated table for file names, YAML fields, option IDs, domain names, status values, YAML formatting. T7: Runtime Implications added to §4.1 — performance constraints (nesting depth, options limit, string length, FM size) enabling sub-100ms edge serving. Quick Reference Card updated with MAY fields section, naming conventions, 5 invariants. Example 2 updated with `related` field and `schema_version: "0.4"`. |
| 0.3.3-draft | 2026-03-31 | Three consistency fixes: (1) Quick Reference Card updated to v0.3.3 — sources line now shows both full form `{url, type}` and short form. (2) Schema Notes corrected — stale "if/then block" reference replaced with accurate "allOf conditional rules" description covering all three conditionals. (3) Added explicit note that vendor-only anti-gaming rule (§4.2) is not expressible in JSON Schema and SHOULD be enforced by CI/CD linting. |
| 0.3.2-draft | 2026-03-31 | Three surgical fixes: (1) Removed duplicate standalone `if/then` in JSON Schema — `allOf` covers the same rule. (2) Replaced redundant Redis anti-pattern example (AP5) with cross-reference to Anti-Bloat Rule 1 — eliminated copy-paste duplication. (3) **Sources upgraded from plain URL strings to source objects** with `type` enum (`standard`, `vendor`, `community`, `academic`, `official_docs`). Added anti-gaming rule: `vendor` MUST NOT be sole source type. Schema supports both short form (URL string) and full form (object with type) for backward compatibility. Examples 2 and 3 updated to full form; Example 1 retains short form to demonstrate minimum. |
| 0.3.1-draft | 2026-03-31 | Quality expansion from 0.3. Anti-bloat rules: added concrete good/bad examples for all 7 rules. Examples: completed edge case (Example 3) with full body including 4 options, pivotal points, and known failures. CI/CD: replaced placeholder script names with complete, copy-paste-ready Python scripts (extract-frontmatter, check-freshness, check-tokens). Anti-patterns: added 5 before/after code snippets. JSON Schema: added conditional validation — deprecated options require known_failures, ai_generated requires ≥3 sources. |
| 0.3-draft | 2026-03-31 | Complete rewrite from Living Pattern (4 rounds: R1 + PbP + R2 + R3). Zero anchoring from v0.1. Key changes: stratified frontmatter (Level 0/1), event-driven freshness (primary) + time-based fallback (secondary), confidence accepts number/enum/null, `generation_method` field for EU AI Act Art.50, anti-bloat guidelines (7 rules), 3 examples (bare/recommended/edge case), JSON Schema with conditional validation (`if/then` for low confidence). |
| 0.1 | 2026-03-28 | Initial draft. 1,467 lines. |

---

*This specification is a living document. File issues at [github.com/useliber](https://github.com/useliber). Pull requests welcome.*

*LIBER — an operating system for decisions in the AI era.*
