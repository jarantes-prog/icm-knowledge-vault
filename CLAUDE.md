# Claude Code — Project Instructions

This is an **LLM-maintained knowledge vault**. Read `Welcome.md` at the root of this directory before taking any action — it is the authoritative schema defining folder conventions, ingest/query/lint workflows, frontmatter rules, and rules of engagement.

In short:
- `raw/` is immutable source material.
- `wiki/` is your (LLM) workspace — summaries, entity pages, concept pages, analyses, overviews.
- `index.md` is the content catalog; `log.md` is the append-only timeline.
- Every ingest touches multiple wiki pages, updates the index, and appends to the log.
- Every claim cites a `[[Source]]`.

Always follow `Welcome.md`. Update it when a better workflow emerges (with the user's agreement).
