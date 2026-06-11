# 2026-06-11 -- Edith Memory System Setup

**Agent:** Manus
**Date:** 2026-06-11 (Mountain Standard Time)
**Triggered by:** Jack noting Edith was added as the 11th agent

---

## Issue Found

Edith was added to the fleet on 2026-06-10 by Cody but her memory system was not fully configured:

- Dreaming: OFF (memory-core plugin had no dreaming config)
- Wiki vault: bridge mode but bridge.enabled was false (broken state)
- Wiki vault path: pointing to shared wiki root, not Edith's own vault
- Entities folder: missing
- Models: only 5 models (old short list, not the 64-model fleet standard)
- Active-memory: enabled but no config (using defaults)

---

## Root Cause

Edith's docker-compose.yml mounts `./config -> /home/node/.openclaw` (not `./` directly).
The config file lives at `/opt/openclaw/agents/edith/config/openclaw.json`.
Earlier fix attempts wrote to the wrong path (agent root dir instead of config/ subdir).

---

## Fixes Applied

- Initialized wiki vault at `/opt/openclaw/shared/knowledge/edith/wiki/` with full structure
- Updated `openclaw.json` with:
  - 64-model standard list (matching fleet)
  - memory-core dreaming enabled at 4:30 AM Edmonton (next slot after Victor's 4:15 AM)
  - memory-wiki: isolated mode, vault path `/opt/openclaw/shared/knowledge/edith/wiki`
  - active-memory: full config (queryMode: recent, promptStyle: balanced, qmd.searchMode: search)
- Force-recreated container (old container was blocking restart)
- Notion Phase 2.1 updated to reflect 11 agents

---

## Final Verification

```
Dreaming: 30 4 * * * (America/Edmonton) -- OK
FTS: ready -- OK
Vault: ready (/opt/openclaw/shared/knowledge/edith/wiki) -- OK
Hooks: 5/5 ready -- OK
Gateway: running (port 3011) -- OK
```

Vector store shows "paused until memory is rebuilt" -- this is normal for a new agent with no prior memory files. It will initialize after the first dream cycle.

---

## Fleet Now: 11 Agents

| Agent | Port | Dreaming |
|---|---|---|
| Terry | 3001 | 2:00 AM |
| Amanda | 3002 | 2:15 AM |
| Inga | 3003 | 2:30 AM |
| Wilma | 3004 | 2:45 AM |
| Marsha | 3005 | 3:00 AM |
| Maggie | 3006 | 3:15 AM |
| Vivian | 3007 | 3:30 AM |
| Grogar | 3008 | 3:45 AM |
| Gohzed | 3009 | 4:00 AM |
| Victor | 3010 | 4:15 AM |
| Edith | 3011 | 4:30 AM |
