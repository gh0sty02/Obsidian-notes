# LLM Wiki — Schema & Workflow

This file tells Claude Code how to maintain this wiki. Read it at the start of every session before touching any wiki files.

---

## What This Is

This vault contains an LLM-maintained wiki. You (Claude) write and maintain all files inside `wiki/`. The user reads, browses, and guides via Obsidian. Raw source documents live in `raw/` — you read them but never modify them.

**Your role:** disciplined wiki maintainer, not a generic chatbot. Every answer you give should either update the wiki or explicitly decide not to.

---

## Directory Layout

```
wiki/
├── index.md        — Content catalog. Update after every ingest.
├── log.md          — Append-only record. Append after every operation.
├── overview.md     — Evolving high-level synthesis of everything in the wiki.
├── concepts/       — One page per technical concept (transformers, attention, RAG…)
├── entities/       — One page per tool, paper, person, or framework
├── sources/        — One summary page per ingested source
└── synthesis/      — Filed analyses, comparisons, Q&A outputs

raw/                — Immutable source documents. You read; never write.
```

---

## File Conventions

**Naming:** `kebab-case.md` for all wiki files. No spaces.

**Frontmatter** (required on every wiki page):
```yaml
---
title: Human-Readable Title
tags: [concept, llm, attention]
sources: [sources/vaswani-2017.md]
last-updated: YYYY-MM-DD
---
```

**Cross-links:** Use Obsidian wiki-link syntax: `[[page-name]]` or `[[page-name|Display Text]]`. Always link to related pages — this is what makes the graph view valuable.

**Page length:** Prefer dense, well-organized pages over short stubs. A concept page should be comprehensive enough to answer questions about that concept without needing to re-read the original source.

---

## Operations

### Ingest

Triggered when user says: `ingest <source>` or drops a file in `raw/` and asks you to process it.

Steps (do not skip any):
1. **Read** the source fully (from `raw/` path or URL).
2. **Discuss** with the user: what are the key takeaways? Any surprises? What connects to existing wiki content?
3. **Write** `wiki/sources/<slug>.md` — a structured summary with: abstract, key claims, methodology (if applicable), connections to existing wiki pages.
4. **Update** all relevant concept and entity pages. A single source may touch 5–15 pages. For each:
   - Add new information under appropriate headings
   - Note contradictions with existing claims (use `> [!warning] Contradiction:` callout)
   - Add the source to the page's `sources:` frontmatter list
   - Update `last-updated`
5. **Update** `wiki/index.md` — add the new source page and any new concept/entity pages created.
6. **Append** to `wiki/log.md`:
   ```
   ## [YYYY-MM-DD] ingest | Source Title
   - Summary: one-sentence description
   - Pages updated: list of wiki pages touched
   - New pages created: list of new pages
   ```

### Query

Triggered when user asks a factual question or requests analysis.

Steps:
1. **Read** `wiki/index.md` to identify relevant pages.
2. **Read** those pages in full.
3. **Synthesize** an answer with inline citations like `([[concepts/attention-mechanism]])`.
4. **Ask** the user: "Should I file this as a page in `wiki/synthesis/`?" File it if yes.
5. If filed, append to `wiki/log.md`:
   ```
   ## [YYYY-MM-DD] query | Question Title
   - Filed: wiki/synthesis/<slug>.md
   ```

### Lint

Triggered when user says: `lint the wiki` or `wiki health check`.

Check for and report:
1. **Orphan pages** — pages with no inbound links from other wiki pages (use Grep across `wiki/`)
2. **Contradictions** — claims on different pages that conflict (scan for `[!warning]` callouts; also look for numeric claims that might have been revised)
3. **Missing pages** — concepts mentioned inline (`[[concept]]`) but without their own page
4. **Stale sources** — pages whose `last-updated` is old relative to newer sources that might supersede them
5. **Data gaps** — important topics with thin coverage; suggest new sources to look for

After linting, append to `wiki/log.md`:
```
## [YYYY-MM-DD] lint | Wiki Health Check
- Orphans found: N
- Contradictions flagged: N
- Missing pages: list
- Recommendations: ...
```

---

## Index Format

`wiki/index.md` is a catalog organized by category. Each entry: `- [[page-link]] — one-line description`.

Categories: Sources | Concepts | Entities | Synthesis

Keep it sorted alphabetically within each category. The LLM reads this file first when answering queries.

---

## Log Format

`wiki/log.md` is append-only. Never edit existing entries. Each entry starts with `## [YYYY-MM-DD] operation | Title` so it's greppable:

```bash
grep "^## \[" wiki/log.md | tail -10   # last 10 operations
grep "ingest" wiki/log.md              # all ingests
```

---

## Existing Vault Content

The vault has pre-existing folders (`AI/`, `Study Plan/`, `Second Brain.md`) that are NOT part of the wiki. Do not modify them. When ingesting content from them, treat them as read-only sources and create proper wiki pages from them.

Notable pre-existing content to bootstrap from:
- `AI/Understanding foundation Models.md` — deep notes on transformers, attention, tokenization
- `AI/Introduction.md` — LLM overview and model taxonomy
- `AI/Roadmap.md` — 12-week LLM engineering study plan

---

## Session Start Checklist

At the start of every session:
1. Read this file (CLAUDE.md)
2. Read `wiki/index.md` to orient yourself
3. Read the last 5 entries of `wiki/log.md` to understand recent activity
4. Ask the user what they want to do if it's not clear from context
