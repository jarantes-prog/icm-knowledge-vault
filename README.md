# ICM Knowledge Vault

**An LLM-maintained knowledge base. Folders over agents.**

A starter template for a *compounding* knowledge vault that an AI agent (e.g. [Claude Code](https://www.anthropic.com/claude-code)) reads and writes for you. You curate the sources and direct the analysis; the agent does the bookkeeping — summarizing, cross-referencing, filing, and keeping every page consistent and cited.

Inspired by Jake Van Clief's **Interpretable Context Methodology (ICM)** ("folders over agents") and Andrej Karpathy's LLM-wiki idea. Nothing here is AI-specific — point it at **any** fast-moving topic: law, biotech, a market, a company's internal knowledge, a hobby.

> **You read; the LLM writes.** Obsidian is the IDE, the LLM is the programmer, this vault is the codebase.

---

## Why this instead of a notes app or a vector "second brain"

- **Glass-box, not black-box.** Every fact lives in a plain markdown file you can open, read, and edit. When the agent gets something wrong, you see exactly which file and fix it.
- **Position-addressed memory.** The folder path *is* the index. No embeddings, no RAG, no retrieval step — and it stays legible to a human.
- **Portable + no lock-in.** It's a folder. Clone it, zip it, commit it. Any editor opens it; any capable agent can run it.
- **It compounds.** One source ingest cascades across many linked pages. The value grows with every add.

## How it's structured

```
.
├── Welcome.md     ← the schema the agent follows (read this first)
├── CLAUDE.md      ← pointer so Claude Code auto-loads Welcome.md
├── index.md       ← the catalog, updated on every ingest
├── log.md         ← append-only timeline of what changed
├── raw/           ← IMMUTABLE source documents (the agent reads, never edits)
└── wiki/          ← the agent's workspace
    ├── overview/  ← top-level syntheses & theses
    ├── entities/  ← people, places, orgs, products
    ├── concepts/  ← topics, themes, ideas, frameworks
    ├── sources/   ← one summary page per ingested source
    └── analysis/  ← comparisons, deep dives, filed-back answers
```

The full rules (frontmatter, naming, ingest/query/lint workflows, citation discipline) live in **[`Welcome.md`](./Welcome.md)**.

## Quickstart

1. Click **“Use this template” → Create a new repository** (or just clone this repo).
2. Open the folder in **Claude Code** (or any agent that can read files). It auto-loads `CLAUDE.md`, which points at `Welcome.md`.
3. Drop a source into `raw/` — a PDF, an article, a pasted transcript, your notes.
4. Say **“ingest it.”** The agent writes a cited source summary, creates/updates entity and concept pages, and updates `index.md` + `log.md`.
5. Later, ask **“what do we know about X?”** It reads the index, follows the links, and answers with citations.

That's the whole loop: **drop → "ingest it" → ask.**

## Do I need Obsidian?

No. Obsidian is **optional** — a nice viewer for the graph and `[[backlinks]]`. The vault is plain markdown in folders, so any text editor (or GitHub) opens it. If you do use Obsidian, add `-path:raw` in Graph View → Filters to hide the immutable raw sources and see only the linked wiki.

## Credits

- **ICM / "folders over agents"** — Jake Van Clief ([Clief Notes community](https://www.skool.com/cliefnotes), [ICM paper](https://arxiv.org/abs/2603.16021)).
- **LLM-compiled wiki pattern** — Andrej Karpathy.

## License

MIT — see [`LICENSE`](./LICENSE). Use it, fork it, make it yours.
