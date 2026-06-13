# Log

Append-only chronological record of everything that happens to the wiki — ingests,
queries, lints. Newest entries at the bottom. Greppable: `grep "^## \[" log.md | tail -5`.
Entry formats are defined in [[CLAUDE]] §4.

---

## [{{DATE}}] setup
- Initialized the LLM-wiki second brain "{{BRAIN_NAME}}" via the `create-second-brain` skill.
- Created CLAUDE.md (schema), index.md, log.md, lint.md, cache.md, README.md.
- Created folders: raw/, raw/assets/, wiki/{sources,entities,concepts}/, plus
  wiki/overview.md and wiki/synthesis.md.
- Ready for first ingest: drop a source into raw/ and say "ingest this".
