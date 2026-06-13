# AI Second Brain — `create-second-brain` skill

A Claude Code skill that scaffolds a complete **LLM-wiki "second brain"** in one command,
based on [Andrej Karpathy's LLM-wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

Instead of re-deriving answers from raw documents on every question (RAG), an LLM agent
**incrementally builds and maintains a persistent, interlinked markdown wiki** that sits
between you and your sources. Knowledge is compiled once and kept current — the wiki grows
richer with every source you add and every question you ask.

- **You** curate sources and ask questions.
- **The LLM** does the bookkeeping — summarizing, cross-referencing, filing, linting.

This repo ships the skill plus two reference brains built with it: [`AI_connect/`](./AI_connect)
and [`knowledge/`](./knowledge).

---

## Requirements

- **[Claude Code](https://claude.com/claude-code)** (CLI, desktop, or IDE extension).
- That's it. No API keys to configure for the skill itself, no Python, no build step.
- *(Optional)* [Obsidian](https://obsidian.md) to browse/graph the wiki, and `git` to
  version-control each brain.

---

## Install

The skill lives at `.claude/skills/create-second-brain/`. Because it's a **project-level**
skill, it's available automatically to any Claude Code session started in this repo.

```bash
git clone <this-repo-url>
cd AI_Second_brain
claude            # start Claude Code here
```

Verify it loaded by typing `/` in the Claude Code prompt — `create-second-brain` should
appear in the slash-command list. (Skills are picked up on session start; if you added it
to a running session, restart Claude Code.)

> **Want it available in every project, not just this repo?**
> Copy the folder to your user-level skills directory:
> ```bash
> cp -r .claude/skills/create-second-brain ~/.claude/skills/
> ```

---

## Initiate it

In the Claude Code terminal, run the slash command with a name for your brain:

```
/create-second-brain research
```

- **With a name** → creates a `./research/` subfolder containing the full brain.
- **Without a name** (`/create-second-brain`) → the skill asks you what to name it.
- Names are slugified to kebab-case for the folder (`My Research` → `my-research/`).
- If a non-empty folder of that name already exists, the skill **stops and asks** — it
  never overwrites an existing brain.

### What gets created

```
research/
├── CLAUDE.md      The schema — rules the agent follows (read first every session).
├── README.md      Quick-start for this specific brain.
├── index.md       Content catalog of the wiki. Updated on every ingest.
├── log.md         Append-only chronological record (ingests, queries, lints).
├── lint.md        Standing list of contradictions, orphans, gaps, open questions.
├── cache.md       Recent-conversation memory (not a knowledge store).
├── raw/           ← YOU put source documents here. The agent reads, never edits.
│   └── assets/    Images / attachments.
└── wiki/          LLM-owned pages:
    ├── overview.md   Top-level map — the front door.
    ├── synthesis.md  The evolving thesis: what we believe, and why.
    ├── sources/      One summary page per ingested source.
    ├── entities/     People, organizations, products, places, tools.
    └── concepts/     Ideas, topics, themes, methods, frameworks.
```

The brain starts **empty and generic**. The skill only lays down the structure — you
supply the knowledge next.

---

## Use it

Once scaffolded, the brain is **self-contained**: its own `CLAUDE.md` is the agent's full
operating manual. You no longer need the skill at runtime — just work inside the brain
folder.

1. **Open the brain folder** in Claude Code (start a session in `research/`, or `cd` into
   it). The agent reads `CLAUDE.md` + `cache.md` and knows its role.

2. **Add a source.** Drop any document into `research/raw/` — an article, paper, PDF,
   transcript, or note. (Images go in `raw/assets/`.)

3. **Ingest it.** Say:
   > **"ingest this"**

   The agent reads the source, discusses the takeaways, writes a `wiki/sources/<slug>.md`
   summary, then updates every entity/concept page it touches, flags contradictions,
   revises `synthesis.md`/`overview.md`, and updates `index.md` + `log.md`. One source
   typically touches 10–15 pages.

4. **Ask questions.** Say anything:
   > **"How do X and Y compare?"**

   The agent reads `index.md`, drills into the relevant pages, answers **with citations**,
   and offers to file good answers back into the wiki so explorations compound.

5. **Lint periodically.** After a few ingests (or weekly), say:
   > **"lint the wiki"**

   The agent health-checks for contradictions, stale claims, orphan pages, missing
   cross-references, valuable red links, and data gaps — writing findings to `lint.md` and
   logging the pass. *(Lint is a manual, on-request operation. If you want it automatic,
   wrap it with Claude Code's `/loop` or `/schedule`.)*

These are **natural-language operations**, not shell commands — the full workflow for each
is defined in the brain's `CLAUDE.md` §3.

---

## How it grows

1. Curate sources into `raw/`.
2. Ingest them — one at a time to stay involved, or batch for less supervision.
3. Ask questions; file the good answers back into `wiki/`.
4. Lint periodically to keep it healthy.

The bookkeeping that makes people abandon wikis — updating cross-references, keeping
summaries current, flagging contradictions — is exactly what the LLM does for free.

---

## Tips

- **Multiple brains.** Run `/create-second-brain <name>` again with a different name to keep
  separate brains side by side (this repo's `AI_connect/` and `knowledge/` are examples).
- **Obsidian.** Open a brain folder as an Obsidian vault to browse wikilinks and the graph
  view. The Web Clipper extension drops web articles straight into `raw/`.
- **Version control.** `git init` inside a brain to track how the wiki evolves; commit after
  each ingest (`git commit -m "ingest: <source>"`).
- **The schema is yours to evolve.** `CLAUDE.md` is co-owned — as your needs change, edit the
  rules and the agent follows the new ones next session.

---

## Repo layout

```
AI_Second_brain/
├── README.md                              ← this file
├── .claude/skills/create-second-brain/    ← the skill (SKILL.md + templates/)
├── AI_connect/                            ← reference brain (harness engineering)
└── knowledge/                             ← reference brain
```
