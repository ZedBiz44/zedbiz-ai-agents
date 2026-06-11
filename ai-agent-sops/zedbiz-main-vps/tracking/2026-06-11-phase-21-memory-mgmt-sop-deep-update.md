# 2026-06-11 -- Phase 2.1 Memory-Mgmt-SOP Deep Update

**Agent:** Manus
**Date:** 2026-06-11 (Mountain Standard Time)
**Session type:** Documentation deep-dive + fleet audit

---

## What Was Done

### OpenClaw Memory Docs Reviewed

Read all official OpenClaw memory documentation:
- https://docs.openclaw.ai/concepts/memory
- https://docs.openclaw.ai/concepts/memory-qmd
- https://docs.openclaw.ai/concepts/active-memory
- https://docs.openclaw.ai/concepts/session
- https://docs.openclaw.ai/cli/hooks/
- https://docs.openclaw.ai/automation/hooks

### Fleet Audit Results (All 10 Agents)

Audited all 10 agents for:
- Active Memory + QMD Engine status
- Session-Memory hook configuration
- Dreaming schedules
- Memory backend
- Daily memory file counts

**Results: All 10 agents fully operational across all 4 memory layers.**

| Agent | Active Memory | Session-Memory Hook | Dreaming | Memory Files |
|---|---|---|---|---|
| Inga | OK | OK (25 msgs, llmSlug) | 2:30 AM | 13 |
| Amanda | OK | OK | 2:15 AM | 15 |
| Victor | OK | OK | 4:15 AM | 27 |
| Wilma | OK | OK | 2:45 AM | 14 |
| Marsha | OK | OK | 3:00 AM | 25 |
| Gohzed | OK | OK | 4:00 AM | 14 |
| Grogar | OK | OK | 3:45 AM | 15 |
| Maggie | OK | OK | 3:15 AM | 17 |
| Vivian | OK | OK | 3:30 AM | 14 |
| Terry | OK | OK | 2:00 AM | 28 |

### Phase 2.1 Notion Page Updated (v4)

Notion page: https://app.notion.com/p/449a3e33d581839c920201e06670f434

New sections added:
- Full 4-layer memory architecture explanation
- Session-Memory Hook details (how daily files are created, naming, accumulation)
- Dreaming system deep-dive (3 phases, all settings, staggered schedule table)
- Active Memory + QMD Engine explanation (why agents remember things automatically)
- Hooks reference table (all 5 hooks, triggers, who benefits)
- Memory storage backend (SQLite, why Mem0 is not used)
- Agent instructions (what to put in MEMORY.md for agents)
- Verification checklist for new agent setup
- Fleet status table (current as of 2026-06-11)
- Notes and known issues (Victor quirk, file archiving, QMD ingestion state, OAuth expiry)

---

## Key Findings

1. **All 4 memory layers are working on all 10 agents** -- no fixes needed
2. **Hooks are all configured and running** -- 5/5 ready on every agent
3. **QMD is active** -- Vector store: ready, FTS: ready on all agents
4. **Active Memory is enabled** -- queryMode: recent, promptStyle: balanced, qmd.searchMode: search
5. **Session memory files accumulate indefinitely** -- no auto-archiving, manual cleanup may be needed after months
6. **QMD ingestion state absent** is normal (not an error) -- created after first dream cycle with new files
7. **Memory backend is SQLite** (built-in) -- Mem0 is documented but not in use

---

## Questions Answered for Jack

- How daily memory files are stored: /home/node/.openclaw/workspace/memory/YYYY-MM-DD-slug.md
- How often they are updated: Every time /new or /reset is typed (end of session)
- Are they archived: No -- they accumulate. Manual cleanup needed eventually.
- Session-memory and hooks: Explained in detail. All working. Hooks are operator-managed, not agent-managed.
- Is QMD turned on: Yes, on all 10 agents.
- Is Active Memory working: Yes, on all 10 agents.
- What instructions do agents need: Documented in the "What Agents Need to Know" section.

---

## Files Changed

- Notion: Phase 2.1 Memory-Mgmt-SOP (v4) -- https://app.notion.com/p/449a3e33d581839c920201e06670f434
- GitHub: this tracking log
