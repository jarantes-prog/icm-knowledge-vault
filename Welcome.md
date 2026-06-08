# Welcome — An LLM-Maintained Knowledge Vault (ICM-style)

This vault is an **LLM-maintained wiki**. A persistent, compounding knowledge base where you curate sources and direct analysis, and the LLM does the bookkeeping — summarizing, cross-referencing, filing, and maintaining consistency across pages.

You read; the LLM writes. Obsidian is the IDE; the LLM is the programmer; this vault is the codebase.

> **Works for any topic.** Nothing here is AI-specific. Point it at law, biotech, a sport, a company's internal knowledge, a hobby — anything that updates faster than you can remember.

---

## The Three Layers

1. **Raw sources** (`raw/`) — immutable. Articles, PDFs, notes, transcripts, images. The LLM reads but never modifies.
2. **The wiki** (`wiki/`) — LLM-generated markdown. Summaries, entity pages, concept pages, analyses. Fully maintained by the LLM.
3. **The schema** (this file) — rules the LLM follows. Evolves with use.

---

## Folder Conventions

```
vault/
├── Welcome.md              ← this file, the schema
├── CLAUDE.md               ← pointer so Claude Code auto-loads Welcome.md
├── index.md                ← content-oriented catalog (updated on every ingest)
├── log.md                  ← chronological append-only record
├── raw/                    ← IMMUTABLE source documents
│   └── assets/             ← images/attachments downloaded from clipped articles
└── wiki/                   ← LLM-owned output
    ├── overview/           ← top-level syntheses & theses
    ├── entities/           ← people, places, orgs, products
    ├── concepts/           ← topics, themes, ideas, frameworks
    ├── sources/            ← one summary page per ingested source
    └── analysis/           ← comparisons, query answers filed back, deep dives
```

### Naming
- Page names use **Title Case** with spaces (Obsidian-friendly): `Vannevar Bush.md`, `Memex Concept.md`.
- Source summaries mirror the source title with an ingest date prefix: `2026-04-19 — Article Title.md`.
- Use `[[Wiki Links]]` for cross-references; Obsidian resolves them.

### Frontmatter (YAML)
Every wiki page gets frontmatter so Obsidian's Dataview plugin can query it:

```yaml
---
type: entity | concept | source | analysis | overview
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
sources: ["[[Source Page 1]]", "[[Source Page 2]]"]   # which raw sources back this page
---
```

For source pages, add:
```yaml
source_type: article | paper | podcast | book | video | note | conversation
source_url: https://...
source_date: YYYY-MM-DD
```

---

## Operations

### 1. Ingest — "add this source"

When the user drops something in `raw/` and asks to ingest it:

1. **Read** the source fully. If it references images in `raw/assets/`, view the important ones.
2. **Discuss** key takeaways with the user in chat before writing — confirm emphasis and framing.
3. **Write a source summary** in `wiki/sources/YYYY-MM-DD — Title.md` with:
   - Frontmatter (type: source, source_type, source_url, source_date, tags, created, updated)
   - TL;DR (2-4 sentences)
   - Key claims (bulleted, each citable)
   - Entities mentioned → `[[Wiki Links]]` to entity pages
   - Concepts discussed → `[[Wiki Links]]` to concept pages
   - Notable quotes (with line or section reference)
   - Open questions / things worth following up on
4. **Create or update entity pages** for every new person/place/org/product the source introduces or adds info to. Each entity page ends with a `## Appears in` section listing source links.
5. **Create or update concept pages** for every new idea/theme the source introduces or adds info to. Each concept page ends with `## Sources` and `## Related concepts`.
6. **Update the overview** (`wiki/overview/`) only when the new source materially shifts the overall synthesis — not for every ingest. Note contradictions explicitly: "Source X claims Y, but [[Earlier Source]] claimed Z."
7. **Update `index.md`** — add the new source and any newly-created pages to the catalog with one-line summaries.
8. **Append to `log.md`** using the format: `## [YYYY-MM-DD] ingest | Title` followed by a brief note of pages touched.
9. **Report back** in chat: list the pages created/modified, flag any contradictions with existing content, suggest 1-3 follow-up sources or questions worth investigating.

A single ingest typically touches **5-15 wiki pages**. That's normal — it's the compounding value.

### 2. Query — "what do we know about X?"

1. **Read `index.md` first** to find candidate pages.
2. **Drill into relevant pages**. Follow `[[links]]` as needed.
3. **Synthesize** an answer with explicit citations: "According to [[Source Name]]..." or "[[Concept Page]] argues...".
4. **Format** based on the question:
   - Direct question → markdown answer in chat
   - Comparison → table
   - Timeline → chronological list
   - Presentation → Marp slide deck (`wiki/analysis/<topic>.md` with Marp frontmatter)
   - Data → chart (matplotlib) + notes
5. **File valuable answers back** into `wiki/analysis/` when the answer produced a new insight, comparison, or synthesis worth keeping. Ask the user if unsure. Update index + log if filed.

### 3. Lint — "health check the wiki"

When asked (or proactively every ~20 ingests), scan for:
- **Contradictions** between pages (flag which source backs which claim).
- **Stale claims** — newer sources superseding older ones without the old page being updated.
- **Orphan pages** — no inbound `[[links]]`.
- **Missing pages** — concepts/entities mentioned in bodies but lacking their own page.
- **Missing cross-references** — page A should link to page B but doesn't.
- **Data gaps** — topics mentioned repeatedly where a targeted web search or new source would help. Suggest these.

Output a lint report as chat output; if the user approves fixes, apply them and log a `lint` entry.

---

## Index & Log — Conventions

### `index.md` (content-oriented)
Organized by category: Overview → Entities → Concepts → Sources → Analysis. Each line is:

```
- [[Page Name]] — one-line summary. *(N sources)*
```

Never let `index.md` go stale. It is the LLM's primary navigation tool when answering queries.

### `log.md` (chronological, append-only)
Every entry starts with a date-prefixed H2 so it's unix-greppable:

```
## [2026-04-19] ingest | Source Title
- Created: [[Source Page]], [[New Entity]]
- Updated: [[Existing Concept]], [[Existing Entity]]
- Flagged: contradiction with [[Older Source]] re: X

## [2026-04-19] query | "what do we know about X?"
- Read: [[Page A]], [[Page B]]
- Filed back: [[Analysis Page]] (if applicable)
```

Quick recall command: `grep "^## \[" log.md | tail -10`.

---

## Rules of Engagement

1. **The LLM owns `wiki/` and the index/log.** The user can read and edit freely, but the LLM maintains them.
2. **`raw/` is immutable.** Never overwrite or delete. Add only.
3. **Always cite.** Every claim in the wiki traces to a `[[Source]]`. No orphan claims.
4. **Flag contradictions, don't resolve them silently.** Write both views; mark which source supports which.
5. **Prefer updating existing pages over creating new ones.** Check `index.md` first.
6. **Update `index.md` and `log.md` in the same pass as content changes.** Never let them drift.
7. **When unsure about scope, ask.** "Should this be a new concept page or a section in [[Existing]]?"
8. **Discuss before writing** during ingests — confirm angle and emphasis with the user.
9. **Keep summaries tight.** TL;DRs are 2-4 sentences. Entity pages are scannable, not exhaustive.
10. **The schema evolves.** If a workflow improvement emerges, propose an edit to this file.

---

## Claude Code Defaults

- **Model:** default to `/model opus-plan`. Opus runs during plan mode (architecting, cross-referencing, synthesis); Sonnet executes routine edits. This materially extends the 5-hour rate-limit window on subscription and often produces better plans than Opus-only.

---

## Obsidian Graph View

Obsidian draws graph edges only from explicit `[[Wiki Links]]`, never from plain-text name matches. The raw transcripts in `raw/` are immutable and contain no wiki-links, so they appear as orphan nodes.

**Filter them out.** Open Graph View → Filters → add `-path:raw` to the search box. The graph then shows only the LLM-maintained wiki — entities, concepts, and source summaries with their actual cross-references.

> Obsidian is **optional** — it's just a viewer for the graph and backlinks. The vault is plain markdown in folders; any text editor (or GitHub) opens it. The actual engine is the LLM agent (e.g. Claude Code) that reads and writes the files.

---

## First-Time Setup Checklist

- [x] `Welcome.md` created (this file)
- [x] `CLAUDE.md` pointer created
- [x] `index.md` created (empty skeleton)
- [x] `log.md` created (with bootstrap entry)
- [x] Folder structure: `raw/`, `raw/assets/`, `wiki/{overview,entities,concepts,sources,analysis}/`
- [ ] First source ingested

When you're ready, drop a source into `raw/` and say "ingest it" — or paste a link and I'll help you capture it.
