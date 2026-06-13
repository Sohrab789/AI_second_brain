---
type: overview
title: Cache
created: {{DATE}}
updated: {{DATE}}
tags: [meta]
---

# Cache — recent-conversation memory

Rolling **~500-word** summary of what we've recently been doing/discussing, for fast
cross-session catch-up. Complements [[log]] (structured history). Not a knowledge store;
read at session start, refreshed at the end of meaningful turns. See [[CLAUDE]] §5.

## Recent (as of {{DATE}})
- **Initialized this LLM-wiki second brain** ("{{BRAIN_NAME}}"): schema [[CLAUDE]], navigation
  ([[index]], [[log]], [[lint]], this cache), and the empty `raw/` + `wiki/` layers.
- No sources ingested yet.

## Open threads / likely next
- Add the first source to `raw/` and say "ingest this".
