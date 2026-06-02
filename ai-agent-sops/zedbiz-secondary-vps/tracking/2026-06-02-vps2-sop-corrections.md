# VPS2 SOP Corrections — 2026-06-02

**Date:** 2026-06-02 MST
**Performed by:** Manus AI
**Triggered by:** Edith Phase 1 install revealed two SOP gaps

---

## Issue 1: Missing Curated Model List in openclaw.json Template

### Problem
The Phase 1.1 SOP `openclaw.json` template only contained a bare `"model": "openrouter/..."` string.
When OpenClaw sees no explicit `agents.defaults.models` allowlist, it pulls the full raw catalogue
from every enabled provider (OpenAI + OpenRouter), populating the dropdown with 100+ junk models
(GPT-5.4 Pro, o1, o3, o4-mini, etc.).

### Fix Applied
- Updated `ai-agent-base-build-sop-phase11.md` (GitHub) to include the full `agents.defaults` block
  copied from Harry at install time
- Updated `ai-agent-base-build-sop` Notion page (ID: 3e4a3e33) with the same correction
- Updated `human-agent-base-build-sop` Notion page (ID: 04ea3e33) with the same correction
- Applied fix to Edith's live `openclaw.json` on VPS2
- Verified: Harry=63 models, Suzy=63 models, Edith=63 models, zero diff

### Root Cause Note
This was a template gap, not a code bug. The SOP was written before the curated model list
pattern was established. Any agent installed from the old template would have the same issue.

---

## Issue 2: Missing Steps in VPS2 Phase 1.1 SOP

### Problem
During a previous SOP update session (memory/wiki additions), the update operation wiped out
the majority of both the agent and human Phase 1.1 SOP pages in Notion, leaving only 2 steps.
Jack restored the pages from a backup but the memory/wiki steps were missing.

### Fix Applied
The following steps were added or corrected in both `ai-agent-base-build-sop` and
`human-agent-base-build-sop` Notion pages (VPS2 database: 109a3e33):

| Step | What Was Added/Fixed |
|------|---------------------|
| Step 5b | Install @openclaw/codex and @openclaw/discord plugins + create codex-home dir |
| Step 6c | Pre-seed workspace with IDENTITY.md, USER.md, SOUL.md, AGENTS.md |
| Step 12 | Verify curated model list in openclaw.json (not browser UI) |
| Step 12b | Full memory/wiki setup (3 sub-steps: plugins, vault dirs, verify) |
| Step 16 | Create GitHub tracking note and push |
| Done-When checklist | Updated to reflect actual model config (openai/gpt-5.5 primary) |

### Verification
All 6 key checks passed on both Notion pages after update.

---

## Affected Files

| File | Change |
|------|--------|
| `ai-agent-sops/zedbiz-secondary-vps/ai-agent-base-build-sop-phase11.md` | Added curated models, Step 5b, Step 6c, Step 12b, Step 16 |
| `ai-agent-sops/zedbiz-secondary-vps/human-agent-base-build-sop-phase11.md` | Same corrections |
| Notion: ai-agent-base-build-sop (3e4a3e33) | Full corrected content |
| Notion: human-agent-base-build-sop (04ea3e33) | Full corrected content |
