# VPS1 SOP Memory/Wiki Step Addition — 2026-06-02

**Date:** 2026-06-02 MST
**Performed by:** Manus AI
**Triggered by:** VPS1 Phase 1.1 SOPs were missing the memory and wiki setup steps
(same gap that existed in VPS2 SOPs before today's correction)

---

## Background

The VPS1 Phase 1.1 SOPs previously had the memory/wiki steps wiped by a bad `replace_content`
operation. Jack restored the pages from backup, but the memory/wiki steps were not in the backup.
Today those steps were added back using `update_content` (surgical insert only -- no existing
content was modified or deleted).

---

## Changes Made

### VPS1 AI Agent SOP (agent-executable version)
**Notion page:** `ai-agent-base-build-sop` (ID: c3fa3e33-d581-822c-a174-81bd46ad1d4f)
**Insertion point:** Between Step 11 (Final Validation) and the Final Action block

**New step added:** `Step 12: Set Up Agent Memory`
- Sub-step 12-1: Add memory-core, memory-wiki, active-memory plugins to openclaw.json
- Sub-step 12-2: Create wiki vault directory structure (concepts, entities, reports, sources,
  syntheses, _views, _attachments) + seed index.md files
- Sub-step 12-3: Restart container and verify memory/wiki status (3 assertions)

### VPS1 Human SOP
**Notion page:** `human-agent-base-build-sop` (ID: 287a3e33-d581-8376-b316-011ed0809b01)
**Insertion point:** Between Step 12 (LLM Model Dropdown) and Step 13 (Delete BOOTSTRAP.md)

**New step added:** `Step 12b: Set Up Agent Memory and Wiki`
- Sub-step 12b-1: Add memory plugins to openclaw.json
- Sub-step 12b-2: Create wiki vault directory structure + seed index.md files
- Sub-step 12b-3: Restart container and verify memory/wiki status (3 assertions)

---

## Verification

**VPS1 agent SOP:** 8/8 checks passed
- Step 12 memory, 12-1 plugins, 12-2 wiki dirs, 12-3 verify all present
- Step 11, Step 1, Step 8, Final Action all still intact

**VPS1 human SOP:** 10/10 checks passed
- Step 12b memory, 12b-1 plugins, 12b-2 wiki dirs, 12b-3 verify all present
- Steps 12, 13, 14, 1, 8 all still intact
- Phase 1 Done When checklist still intact

---

## Method Note

Used `update_content` (search-and-replace insert) NOT `replace_content`.
This is the safe method for adding steps to existing SOPs without risk of data loss.
**Never use `replace_content` on SOP pages that have existing content.**

---

## Memory/Wiki Plugin Config Reference

The memory stack added to both VPS1 and VPS2 agents uses these three plugins:

| Plugin | Purpose |
|--------|---------|
| `memory-core` | Dreaming engine (nightly at 02:00 America/Edmonton), light/deep/REM phases |
| `memory-wiki` | Wiki vault in isolated mode, bridges memory artifacts to wiki |
| `active-memory` | Real-time memory injection into context during conversations |

Dreaming schedule: `0 2 * * *` (America/Edmonton)
Wiki vault mode: `isolated` (per-agent, not shared)
