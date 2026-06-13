# {{BRAIN_NAME}} — LLM-Wiki Second Brain

A personal, compounding knowledge base built on the **LLM-wiki** pattern. Instead of
re-deriving answers from raw documents on every question (RAG), an LLM agent
**incrementally builds and maintains a persistent, interlinked markdown wiki** that sits
between you and your sources. Knowledge is compiled once and kept current — the wiki gets
richer with every source you add and every question you ask.

- **You** curate sources, direct analysis, and ask questions.
- **The LLM** does the rest — summarizing, cross-referencing, filing, bookkeeping.
- **Obsidian** is the IDE, the LLM is the programmer, the wiki is the codebase.

The agent's full operating manual is **[`CLAUDE.md`](./CLAUDE.md)** — read that for the
detailed rules. This README is the quick start.

---

## Layout

```
CLAUDE.md     The schema — rules the agent follows (read first every session).
README.md     This file.
index.md      Content catalog of the wiki. Updated on every ingest.
log.md        Append-only chronological record (ingests, queries, lints).
lint.md       Standing list of contradictions, orphans, gaps, open questions.
cache.md      Session scratchpad / working memory (not a knowledge store).
raw/          Your immutable source documents. The agent reads, never edits.
  assets/     Images/attachments only (auto-filled by Obsidian; optional).
wiki/         LLM-owned pages:
  overview.md   Top-level map — the front door.
  synthesis.md  The evolving thesis: what we believe, and why.
  sources/      One summary page per ingested source.
  entities/     People, organizations, products, places, tools.
  concepts/     Ideas, topics, themes, methods, frameworks.
```

**Three layers:** raw sources (truth, immutable) → the wiki (LLM-owned) → the schema
(`CLAUDE.md`, co-evolved). See `CLAUDE.md` §1–2.

---

## Commands

These are **natural-language operations** you give the agent — not shell commands. The
agent executes the full workflow defined in `CLAUDE.md` §3.

| Say this | What happens |
|---|---|
| *"Ingest this"* (after adding a file to `raw/`) | Agent reads the source, discusses takeaways, writes `wiki/sources/<slug>.md`, updates every entity/concept it touches, flags contradictions, revises `synthesis.md`/`overview.md`, then updates `index.md` and `log.md`. One source ≈ 10–15 pages touched. |
| *"<your question>"* | Agent reads `index.md`, drills into relevant pages, answers **with citations**, and offers to file good answers back as new wiki pages so explorations compound. |
| *"Lint the wiki"* | Agent health-checks: contradictions, stale claims, orphans, missing cross-references, red links worth promoting, data gaps. Findings go to `lint.md`. Run it after a few ingests, or weekly. |

### Where to put things
- **Text source** (article, paper, transcript, note) → drop in **`raw/`**.
- **Image / attachment** → **`raw/assets/`** (Obsidian fills this automatically when you
  clip an article and hit "Download attachments"). Skip entirely if your sources are text.

---

## Obsidian tips
- **Web Clipper** browser extension converts web articles to markdown into `raw/`.
- **Graph view** shows the shape of the wiki — hubs, clusters, orphans.
- **Dataview** plugin can query the YAML frontmatter (`type`, `tags`, `created`, …) the
  agent writes on every page, for dynamic tables.
- **Marp** plugin renders markdown slide decks the agent can generate from wiki content.

---

## How it grows
1. Curate sources into `raw/`.
2. Ingest them one at a time (stay involved) or batch (less supervision).
3. Ask questions; file the good answers back into `wiki/`.
4. Lint periodically to keep it healthy.

The bookkeeping that makes humans abandon wikis — updating cross-references, keeping
summaries current, flagging contradictions — is exactly what the LLM does for free.

---

_Scaffolded by the `create-second-brain` skill, based on Andrej Karpathy's
[LLM-wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)._
