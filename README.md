<div align="center">

# LIBER

### Warstwa wiedzy decyzyjnej dla ery AI · The decision knowledge layer for the AI era

> **LIBER zamienia rozproszoną, starzejącą się wiedzę o tym, *którą* opcję wybrać, w żywy, ustrukturyzowany artefakt — czytelny dla człowieka i dla agenta AI w chwili decyzji.**
>
> *LIBER turns the scattered, decaying knowledge of *which* option to choose into a living, structured artifact — readable by a human and by an AI agent at the moment of decision.*

[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Spec](https://img.shields.io/badge/spec-v0.4.0--draft-8B6FCC.svg)](./SPEC.md)
[![Version](https://img.shields.io/badge/version-0.1.0-3FA9B8.svg)](https://github.com/useliber/liber/releases)
[![Status](https://img.shields.io/badge/status-pre--final-D4A538.svg)](#status--wydanie-pre-final--status--pre-final-edition)

**PL:** Pierwsza otwarta *warstwa wiedzy decyzyjnej* dla ekosystemu AI. Nie dokumentacja „jak", lecz krajobraz „którą opcję i dlaczego".
**EN:** The first open *decision knowledge layer* for the AI ecosystem. Not "how-to" documentation, but a landscape of "which option, and why."

</div>

---

## Czym jest LIBER / What LIBER is

**PL:** Świat pęka w szwach od dokumentacji, która mówi **jak** użyć narzędzia. Niemal nic nie mówi, **którą** opcję wybrać, **jakie** są alternatywy i **kiedy** każda z nich jest złą odpowiedzią. Ta wiedza — *krajobraz decyzyjny* — leży rozsypana po blogach, wątkach Stack Overflow, prelekcjach i w głowach seniorów, którzy zaraz odejdą na emeryturę.

LIBER porządkuje ją w jeden żywy, ustrukturyzowany, czytelny maszynowo artefakt: **plik `.liber`**. Plik `.liber` mapuje krajobraz decyzyjny — opcje, kompromisy między nimi i konteksty, które przesuwają to, która opcja pasuje najlepiej. Napisany raz, odświeżany cyklicznie, czytany przez człowieka na GitHub i parsowany przez agenta AI dokładnie w chwili decyzji.

**EN:** The world is full of documentation that tells you **how** to use a tool. Almost nothing tells you **which** option to choose, **what** the alternatives are, and **when** each one is the wrong answer. That knowledge — the *decision landscape* — lies scattered across blogs, Stack Overflow threads, conference talks, and the heads of senior engineers about to retire.

LIBER organizes it into one living, structured, machine-readable artifact: the **`.liber` file**. A `.liber` file maps a decision landscape — the options, the trade-offs between them, and the contexts that shift which option fits best. Written once, refreshed on a cycle, read by a human on GitHub and parsed by an AI agent at the exact moment of decision.

> **PL:** Context7 mówi agentowi **jak** użyć Next.js. LIBER mówi, **czy** go użyć — i jakie są alternatywy.
> **EN:** Context7 tells an agent **how** to use Next.js. LIBER tells it **whether** to — and what the alternatives are.

---

## Dlaczego teraz / Why now

**PL:** Prawo Liebiga z agronomii mówi, że wzrost ogranicza nie suma zasobów, lecz ten najrzadszy. Dla agentów AI w 2026 limitującym składnikiem przestała być *zdolność* — stał się nim *osąd*.

**EN:** Liebig's law of the minimum, from agronomy, says growth is limited not by total resources but by the scarcest one. For AI agents in 2026, the limiting nutrient is no longer *capability* — it is *judgment*.

Trzy siły zbiegły się w jednym punkcie / Three forces converged on the same point:

1. **PL — Agenci potrafią działać, ale nie potrafią dobrze decydować.** Stanford AI Index 2026: skuteczność agentów w realnych zadaniach skoczyła z 12% do 66% w rok — a ten sam czołowy model poprawnie odczytuje zegar analogowy ledwie w połowie przypadków. To *poszarpana granica* (jagged frontier): wykonanie rozwiązano szybciej niż wybór tego, co wykonać.
   **EN — Agents can act, but they can't decide well.** Stanford AI Index 2026: agent task-success on real-world tasks jumped from 12% to 66% in a year — yet the same frontier model reads an analog clock correctly barely half the time. The *jagged frontier*: execution got solved faster than choosing what to execute.

2. **PL — Rury są ustandaryzowane; wiedza nie.** MCP (Model Context Protocol) stał się de facto sposobem łączenia agentów z narzędziami i danymi. Brakuje wysokiej jakości wiedzy *decyzyjnej*, którą można przez te rury przesłać.
   **EN — The pipes are standardized; the knowledge isn't.** MCP became the de facto way agents reach tools and data. The missing piece is high-quality *decision* knowledge to send through them.

3. **PL — Regulacja zaraz wymusi udokumentowane decyzje.** Obowiązki transparentności z Artykułu 50 EU AI Act wchodzą w życie **2 sierpnia 2026** — a porozumienie Digital Omnibus z maja 2026, które przesunęło terminy dla systemów wysokiego ryzyka na lata 2027–2028, pozostawiło Artykuł 50 **nietknięty**.
   **EN — Regulation is about to require documented decisions.** EU AI Act Article 50 transparency obligations take effect **August 2, 2026** — and the May 2026 Digital Omnibus agreement, which deferred high-risk deadlines to 2027–2028, left Article 50 **untouched**.

---

## Jak wygląda plik .liber / What a .liber file looks like

**PL:** Plik `.liber` to po prostu **Markdown z blokiem YAML frontmatter**. Jeśli pisałeś kiedyś post w Jekyll, Hugo czy Astro — albo `SKILL.md` — znasz już ten kształt. Żadnego własnego parsera. `gray-matter` (JS) lub `python-frontmatter` (Python) czyta go od ręki. Frontmatter to połowa **maszynowa** (agent parsuje, filtruje po kontekście, działa). Treść Markdown to połowa **ludzka** (czytasz na GitHub, od góry, progresywnie). Jeden plik, dwie publiczności, zero transformacji.

**EN:** A `.liber` file is just **Markdown with a YAML frontmatter block**. If you've ever written a Jekyll, Hugo, or Astro post — or a `SKILL.md` — you already know the shape. No custom parser. `gray-matter` (JS) or `python-frontmatter` (Python) reads it as-is. The frontmatter is the **machine** half (an agent parses, filters by context, acts). The Markdown body is the **human** half (you read it on GitHub, top down, progressively). One file, two audiences, zero transformation.

```yaml
---
domain: database-design-patterns
verified: 2026-06-01
sources:
  - url: https://learn.microsoft.com/en-us/sql/relational-databases/tables/primary-and-foreign-key-constraints
    type: official_docs
  - url: https://www.rfc-editor.org/rfc/rfc9562
    type: standard
confidence: medium
options:
  - id: surrogate-key-bigint-identity
    name: "Surrogate key — monotonic BIGINT IDENTITY"
    status: mainstream
  - id: surrogate-key-guid-random
    name: "Surrogate key — random GUID / UUIDv4"
    status: declining
critical_exclusions:
  - "NEVER cluster a high-insert OLTP table on a random GUID — page splits and fragmentation will eat you alive."
---

# Database Design Patterns — Decision Landscape

## ⚡ Quick Pick (30 sec)
For most relational OLTP schemas: a monotonic surrogate key (BIGINT IDENTITY /
sequence). Reach for UUIDs only with a real distributed-generation need — and
then prefer a time-ordered variant (UUIDv7) over random.
...
```

**PL:** Treść układa się jako *progresywne odsłanianie* — pięć warstw głębi, by junior w pośpiechu i główny architekt czytający całość dostali to, czego potrzebują.
**EN:** The body is organized as *progressive disclosure* — five layers of depth, so a junior in a hurry and a principal architect reading the whole thing both get what they need.

| Warstwa / Layer | Dla kogo / For whom | Czas / Time | Zawiera / Contains |
|---|---|---|---|
| ⚡ **Quick Pick** | Junior, w pośpiechu / in a hurry | 30 s | Bezpieczny default dla 80% / the safe default for 80% |
| 🔀 **Pivotal Points** | Mid, potrzebuje kontekstu / needs context | 5 min | Pytania, które zawężają wybór / questions that narrow the choice |
| 📊 **Full Landscape** | Senior | 15 min | Wszystkie opcje, kompromisy, źródła, confidence / every option, trade-off, source, confidence |
| 🚫 **Critical Exclusions** | Każdy / Everyone | 1 min | Czego **nigdy** nie wybierać / what to *never* choose (via negativa) |
| 🔄 **Contrarian Corner** | Ekspert / Expert | 3 min | Wschodzące alternatywy / emerging alternatives |

Pełna specyfikacja / Full specification: **[SPEC.md](./SPEC.md)** (`v0.4.0-draft`).

---

## Cztery filary / The four pillars

**PL:** LIBER stoi na czterech filarach. Każdy konieczny; żaden samowystarczalny.
**EN:** LIBER stands on four pillars. Each necessary; none sufficient alone.

| Filar / Pillar | Co to jest / What it is | Co definiuje / What it defines |
|---|---|---|
| **FORMAT** | Specyfikacja `.liber` — otwarty standard / the `.liber` spec — an open standard | **Jak** wygląda wiedza / **how** knowledge looks · *CC BY-SA 4.0* |
| **ENGINE** | Generowanie, utrzymanie, serwowanie, analityka / generation, maintenance, serving, analytics | **Jak** wiedza powstaje, żyje i uczy się / **how** knowledge is born, lives, learns · *Apache 2.0* |
| **LIBRARY** | Rosnący zbiór plików `.liber` / a growing collection of `.liber` files | **Co** system wie / **what** the system knows |
| **TRUST** | Certyfikacja jakości decyzji agentów / agent decision-quality certification | **Ile** systemowi można ufać / **how much** to trust it |

To repozytorium jest domem filarów **FORMAT** i **LIBRARY**. **ENGINE** i **TRUST** są na mapie drogowej. / This repository is home to **FORMAT** and **LIBRARY**. **ENGINE** and **TRUST** are on the roadmap.

---

## Zamknięte koło / The closed loop

**PL:** LIBER to jedno koło, nie dwa produkty.

**Lewa strona generuje wiedzę.** Pipeline researchu czyta świat — standardy, RFC, dokumentacje, prace naukowe, prelekcje — i kompiluje plik `.liber` z opcjami, kompromisami i poziomami pewności. Cykl utrzymania re-skanuje świat i nakłada *chirurgiczne* aktualizacje z changelogiem.

**Prawa strona zbiera feedback.** Gdy aktor — człowiek albo agent — podejmuje decyzję na bazie pliku `.liber`, system może zebrać lekki, zanonimizowany **paragon decyzji** (decision receipt): którą opcję wybrano, w jakim kontekście. Zagregowane paragony pokazują, co świat faktycznie *robi*, nie tylko co dokumentacja *mówi*. Z każdym obrotem koła wiedza jest lepsza, bardziej obronna i trudniejsza do skopiowania.

**EN:** LIBER is one wheel, not two products.

**The left side generates knowledge.** A research pipeline reads the world — standards, RFCs, docs, papers, talks — and compiles a `.liber` file with options, trade-offs, and confidence levels. A maintenance cycle re-scans the world and applies *surgical* updates with a changelog.

**The right side collects feedback.** When an actor — human or agent — decides using a `.liber` file, the system can capture a lightweight, anonymized **decision receipt**: which option, in what context. Aggregated, these reveal what the world actually *does*, not just what the docs *say*. With every turn of the wheel the knowledge gets better, more defensible, and harder to copy.

> **PL — Każde twierdzenie ma okres półrozpadu.** Jak w fizyce jądrowej, pewność rozpada się w czasie (`confidence(t) = c₀ × 0.97^miesiące`). Plik `.liber` mierzy swój rozpad i **starzeje się głośno** — przeterminowana wiedza sama o tym mówi. Plik trzyma **dwa osobne sygnały pewności, których nigdy się nie miesza**: `confidence` (jak dobrze udokumentowana jest wiedza) i `adoption_data` (jak ludzie realnie wybierają). Uśrednianie ich byłoby równie bezsensowne, co uśrednianie temperatury z wilgotnością.
>
> **EN — Every claim has a half-life.** As in nuclear physics, confidence decays over time (`confidence(t) = c₀ × 0.97^months`). A `.liber` file measures its own decay and **ages loudly** — stale knowledge says so. The file keeps **two separate confidence signals that are *never* mixed**: `confidence` (how well-sourced the knowledge is) and `adoption_data` (how people actually choose). Averaging them would be as meaningless as averaging temperature and humidity.

---

## Co wyróżnia LIBER / What makes it different

**PL:** Wiele formatów dotyka *części* tego problemu. Żaden nie łączy ośmiu wymiarów naraz — większość pokrywa najwyżej trzy.
**EN:** Plenty of formats touch *part* of this. None combine all eight dimensions — most cover at most three.

| # | Wymiar / Dimension | Znaczenie / Meaning |
|---|---|---|
| 1 | **Auto-generowany / Auto-generated** | Tworzony przez pipeline, nie ręcznie / built by a pipeline, not by hand |
| 2 | **Cyklicznie odświeżany / Cyclically refreshed** | Re-skanowany i chirurgicznie aktualizowany / re-scanned and surgically updated |
| 3 | **Ustrukturyzowany / Structured** | Markdown + YAML — pola, schema, walidacja / fields, schema, validation |
| 4 | **Domenowo-agnostyczny / Domain-agnostic** | Auth, caching, bazy, API — dowolny krajobraz / any decision landscape |
| 5 | **Czytelny maszynowo / Machine-readable** | Agent parsuje YAML i filtruje po kontekście / an agent parses YAML and filters by context |
| 6 | **Krajobraz, nie werdykt / Landscape, not verdict** | Pełen zbiór opcji z kompromisami / a full set of options with trade-offs |
| 7 | **Poziomy pewności / Confidence levels** | Każdy blok ma confidence 0.0–0.95 / every block carries 0.0–0.95 confidence |
| 8 | **Zorientowany na decyzję / Decision-oriented** | Wokół *decyzji*, nie *tematu* / around a *decision*, not a *topic* |

**PL:** Jest komplementarny, nie konkurencyjny: `SKILL.md` mówi **jak** wykonać zadanie — `.liber` mówi **którą** opcję wybrać. ADR zapisuje decyzję *podjętą* — `.liber` przedstawia decyzje *dostępne* (jest wejściem do ADR).
**EN:** It is complementary, not a replacement: `SKILL.md` tells an agent **how** to execute — `.liber` tells it **which** option to choose. An ADR records a decision *taken* — `.liber` presents decisions *available* (it is the input to the ADR).

---

## Rodowód / Lineage

**PL:** Nie wymyśliliśmy strukturyzowania wiedzy decyzyjnej. Jesteśmy kolejnym ogniwem w łańcuchu, który zaczął się w 1977:

**EN:** We didn't invent structuring decision knowledge. We're the next link in a chain that began in 1977:

```
Christopher Alexander (A Pattern Language, 1977)
  → Ward Cunningham (Wiki, 1995)
    → Wikipedia (2001)
      → Architecture Decision Records (Nygard, 2011)
        → LIBER (2026)
```

**PL:** Każde pokolenie rozwiązało problem wiedzy swojej epoki: Alexander dał wspólny język, Wiki — współedycję, Wikipedia — skalę, ADR — kontekst decyzji lokalnej. LIBER czyni wiedzę decyzyjną *ustrukturyzowaną, czytelną maszynowo, cyklicznie odświeżaną i umieszczoną PRZED decyzją*. A za Ranganathanem (1931) traktujemy plik jak **żywy organizm**: rodzi się, rośnie, starzeje, jest certyfikowany lub wycofany.

**EN:** Each generation solved its era's knowledge problem: Alexander gave a shared language, Wiki gave collaboration, Wikipedia gave scale, ADR gave local decision context. LIBER makes decision knowledge *structured, machine-readable, cyclically refreshed, and positioned BEFORE the decision*. And after Ranganathan (1931), we treat each file as a **living organism**: it is born, grows, ages, is certified or deprecated.

---

## Status — wydanie pre-final / Status — pre-final edition

**PL:** To wydanie **pre-final** (`v0.1.0`). Format i pierwszy krajobraz zbiegają się ku `v1.0` — pozostały dystans jest krótki. Wolimy opublikować coś rzetelnego i skromnego dziś niż dopracowanego i spóźnionego.

**EN:** This is a **pre-final** edition (`v0.1.0`). The format and the first landscape are converging toward `v1.0` — the remaining distance is short. We'd rather publish something honest and modest today than polished and late.

| Element / Piece | Status |
|---|---|
| **Specyfikacja `.liber` / `.liber` spec** | 📄 `v0.4.0-draft` — otwarta na feedback / open for feedback |
| **Pierwszy plik / First file** (`database-design-patterns`) | 🚧 **Draft prekursorski / precursor draft** — demonstruje format; pełny krajobraz recenzowany przez eksperta w toku / demonstrates the format; full expert-reviewed landscape in progress |
| **ENGINE** (generowanie / odświeżanie / runtime) | 🛠️ W budowie / in development |
| **TRUST** (certyfikacja decyzji / decision certification) | 🔭 Zaprojektowany, na mapie drogowej / designed, on the roadmap |

---

## Autorstwo i pochodzenie / Authorship & origin

**PL:** LIBER buduje tandem **Przemysław Zieliński + Krzysztof Luty**.
- **Przemysław Zieliński** — twórca i architekt rozwiązania (technologia, produkt, jakość).
- **Krzysztof Luty** — współzałożyciel; model komercjalizacji i dystrybucji.

Pierwsze publiczne wydanie LIBER nosi datę **1 czerwca 2026, Warszawa**. Oryginalność i datę pierwszeństwa utrwala podpisana **[DEKLARACJA](./DECLARATION.md)** oraz cytowalny, zarchiwizowany rekord (DOI). Misja: jako pierwsi wprowadzamy *otwartą warstwę wiedzy decyzyjnej* — infrastrukturę, która podnosi jakość decyzji, czyni je rozpoznawalnymi, monitoruje je w sposób zanonimizowany i certyfikuje agenty AI pod kątem transparentności. Docelowo LIBER ma stać się standardem chronionym przez fundację jako dobro publiczne.

**EN:** LIBER is built by the **Przemysław Zieliński + Krzysztof Luty** tandem.
- **Przemysław Zieliński** — creator and architect of the solution (technology, product, quality).
- **Krzysztof Luty** — co-founder; commercialization and distribution model.

LIBER's first public edition is dated **1 June 2026, Warsaw**. Originality and priority are anchored by the signed **[DECLARATION](./DECLARATION.md)** and a citable, archived record (DOI). The mission: to be the first to introduce an *open decision knowledge layer* — infrastructure that raises decision quality, makes decisions recognizable, monitors them anonymously, and certifies AI agents for decision transparency. In time, LIBER is intended to become a standard stewarded by a foundation as a public good.

---

## Struktura repozytorium / Repository structure

```
liber/
├── README.md          ← ten plik / this file
├── DECLARATION.md      ← deklaracja autorstwa i zasad / declaration of authorship & principles
├── SPEC.md             ← specyfikacja formatu .liber / the .liber format specification
├── CITATION.cff        ← metadane cytowania / citation metadata
├── CONTRIBUTING.md     ← jak współtworzyć / how to contribute
├── LICENSE             ← CC BY-SA 4.0
└── library/
    └── database-design-patterns.liber.md   ← pierwszy .liber (draft prekursorski / precursor draft)
```

---

## Mapa drogowa / Roadmap

1. **PL:** Wyostrzyć pierwszy krajobraz — `database-design-patterns` z draftu do pełnego, recenzowanego pliku. **EN:** Sharpen the first landscape — `database-design-patterns` from draft to a complete, expert-reviewed file.
2. **PL:** Rozbudować bibliotekę (`eu-ai-act-risk-classification` priorytetem wobec terminu 2 sierpnia 2026). **EN:** Grow the library (`eu-ai-act-risk-classification` a priority given the August 2, 2026 deadline).
3. **PL:** Ustabilizować spec do `v1.0`. **EN:** Stabilize the spec to `v1.0`.
4. **PL:** Wypuścić runtime (serwer MCP). **EN:** Ship the runtime (MCP server).
5. **PL:** Zamknąć koło (paragony → dane adopcji → mądrzejsze odświeżenia). **EN:** Close the loop (receipts → adoption data → smarter refreshes).

---

## Współtworzenie / Contributing

**PL:** LIBER jest na wczesnym etapie — to najlepszy moment, by go kształtować. Najcenniejszy jest **rzetelny feedback**: czy format `.liber` ma sens w *Twojej* dziedzinie? Którego krajobrazu brakuje? Coś jest błędne lub przesadzone? Otwórz issue. Zobacz **[CONTRIBUTING.md](./CONTRIBUTING.md)**.

**EN:** LIBER is early — the best time to shape it. The most valuable thing is **honest feedback**: does the `.liber` format make sense for *your* domain? Which landscape is missing? Is something wrong or overclaimed? Open an issue. See **[CONTRIBUTING.md](./CONTRIBUTING.md)**.

---

## Nazwa / The name

**PL:** Z łaciny — podwójne znaczenie: **liber** = *księga* (rdzeń: *library*) i **liber** = *wolny* (rdzeń: *liberty*). Jedno słowo mówi „księga wiedzy" i „otwarta". Rozszerzenie plików: `.liber`.

**EN:** From Latin — a double meaning: **liber** = *a book* (root of *library*) and **liber** = *free* (root of *liberty*). One word that says "a book of knowledge" and "open." File extension: `.liber`.

---

## Cytowanie / Citation

```
Zieliński, P., & Luty, K. (2026). LIBER: The Decision Knowledge Layer for the AI Era (v0.1.0).
https://github.com/useliber/liber
```

---

## Licencja / License

**PL:** Format `.liber` i treść biblioteki w tym repozytorium są na licencji **[Creative Commons Attribution-ShareAlike 4.0](./LICENSE)** (CC BY-SA 4.0). Kod silnika (gdy powstanie) będzie na **Apache 2.0**.

**EN:** The `.liber` format and the library content in this repository are licensed under **[Creative Commons Attribution-ShareAlike 4.0](./LICENSE)** (CC BY-SA 4.0). Engine code, when released, will be licensed under **Apache 2.0**.

---

<div align="center">

*LIBER — wiedza jest po to, by jej używać. Zwłaszcza w erze agentów.*
*LIBER — knowledge is for use. Especially in the age of agents.*

</div>
