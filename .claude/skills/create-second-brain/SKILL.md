---
name: create-second-brain
description: Scaffold a complete LLM-wiki "second brain" (Andrej Karpathy's LLM-wiki pattern) in a named subfolder — CLAUDE.md schema, index/log/lint/cache, README, and the raw/ + wiki/ folder structure. Use when the user types /create-second-brain, asks to set up / initialize a second brain or LLM wiki, or clones this repo and wants to bootstrap their own knowledge base. After scaffolding, they just drop sources into raw/ and say "ingest this".
---

# create-second-brain

Scaffold a fresh, ready-to-use **LLM-wiki second brain** from the bundled templates. This
implements Andrej Karpathy's LLM-wiki pattern: a persistent, compounding markdown knowledge
base where an LLM agent ingests sources, maintains cross-referenced wiki pages, answers
queries with citations, and runs periodic health-checks (lint). The reference
implementation is the sibling `AI_connect/` directory.

The output is **empty and generic** — the human supplies knowledge afterward by dropping
documents into `raw/` and saying "ingest this". Your job here is only to lay down the
structure correctly.

## Templates

The files to copy live in the `templates/` directory next to this `SKILL.md` (i.e.
`.claude/skills/create-second-brain/templates/`). Read them from there; do not regenerate
their content from memory. Each contains placeholders you must substitute on write:

| Placeholder | Replace with |
|---|---|
| `{{WIKI_PATH}}` | The absolute path of the new brain's root folder (the target dir). |
| `{{BRAIN_NAME}}` | The brain's display name (the folder name, or a name the user gives). |
| `{{DATE}}` | Today's date as `YYYY-MM-DD`. |

## Steps

1. **Resolve the brain name.**
   - If the user passed an argument (e.g. `/create-second-brain research`), use it as the
     folder name. Slugify to lowercase kebab-case for the folder (`My Research` → `my-research`);
     keep a readable display name for `{{BRAIN_NAME}}`.
   - If no argument was given, **ask** what to name the brain. Do not invent one.

2. **Resolve the target path.** The brain is created as a subfolder of the **current working
   directory**: `<cwd>/<slug>`. Compute its absolute path — that becomes `{{WIKI_PATH}}`.

3. **Check for collisions.** If the target folder already exists and is non-empty, **stop and
   ask** before doing anything — never overwrite an existing brain. If it doesn't exist or is
   empty, proceed.

4. **Get today's date** (`YYYY-MM-DD`) for `{{DATE}}`. Use the real current date.

5. **Create the folder tree:**
   ```
   <target>/
     raw/
     raw/assets/
     wiki/
     wiki/sources/
     wiki/entities/
     wiki/concepts/
   ```
   Add an empty `.gitkeep` file in each of `raw/`, `raw/assets/`, `wiki/sources/`,
   `wiki/entities/`, and `wiki/concepts/` so the empty folders survive a git clone.

6. **Copy each template into place, substituting all placeholders:**
   | Template | Destination |
   |---|---|
   | `templates/CLAUDE.md` | `<target>/CLAUDE.md` |
   | `templates/README.md` | `<target>/README.md` |
   | `templates/index.md` | `<target>/index.md` |
   | `templates/log.md` | `<target>/log.md` |
   | `templates/lint.md` | `<target>/lint.md` |
   | `templates/cache.md` | `<target>/cache.md` |
   | `templates/wiki/overview.md` | `<target>/wiki/overview.md` |
   | `templates/wiki/synthesis.md` | `<target>/wiki/synthesis.md` |

   Read each template, replace every `{{WIKI_PATH}}`, `{{BRAIN_NAME}}`, and `{{DATE}}`
   occurrence, and write the result to the destination. (`CLAUDE.md` uses only
   `{{WIKI_PATH}}`; `README.md` uses only `{{BRAIN_NAME}}`; `log.md` and `cache.md` use
   `{{BRAIN_NAME}}` and `{{DATE}}`; `index.md`, `lint.md`, `wiki/overview.md`, and
   `wiki/synthesis.md` use only `{{DATE}}`.) Verify no `{{...}}` placeholder remains in any
   written file.

7. **Report what was created** and give the user their next steps:
   - Show the folder tree that now exists.
   - Tell them: drop a source document into `<target>/raw/`, then open that folder in Claude
     Code (or `cd` into it) and say **"ingest this"** to populate the wiki.
   - Remind them the agent's rules live in `<target>/CLAUDE.md`, and they can run
     **"lint the wiki"** after a few ingests (or weekly) to keep it healthy.

## Notes
- This skill only scaffolds. It does **not** ingest sources or write knowledge pages — that
  happens later, inside the new brain, governed by its `CLAUDE.md`.
- The new brain's `CLAUDE.md` is self-contained: once created, opening that folder in Claude
  Code gives the agent its full operating manual. No dependency on this skill at runtime.
- Lint is a manual, on-request operation ("lint the wiki"). If the user wants it automated,
  point them at `/loop` or `/schedule` — but the default is on-request.
