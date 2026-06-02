# 2026-06-01 -- SOP Updates (VPS1 and VPS2)

**Date:** 2026-06-01 MST
**Performed by:** Manus AI
**Notion Database:** Agent-Creation-VPS2-SOP (ee1a3e33)
**GitHub:** This file

---

## Changes Made

### Phase 1.1-agent -- VPS2 (Notion ID: 3e4a3e33)

- **Default model changed:** `openai/gpt-4o` (broken) → `openrouter/google/gemini-3.1-flash-preview`
- **Fallback model added:** `openrouter/deepseek/deepseek-v4-flash`
- **New Step 5b:** Install `@openclaw/codex` npm plugin and create `codex-home` dir
  - `npm install --prefix /root/.openclaw-${AGENT_ID}/npm @openclaw/codex@latest`
  - `mkdir -p /root/.openclaw-${AGENT_ID}/agents/main/agent/codex-home`
  - Without this step, agents throw "Requested agent harness codex is not registered"
- **openai + openrouter plugins** added explicitly to config template
- **Phase goal updated:** Agent is usable immediately after Phase 1.1 (no waiting for Phase 2)
- **Step 12b added:** Memory setup (see below)

### Phase 1.1-human -- VPS2 (Notion ID: 04ea3e33)

Same changes as agent SOP, in human-executable step format.

### Phase restructure

| Old | New | Page |
|-----|-----|------|
| Phase 1.4 | Phase 2.1 | openclaw-llm-model-picker (13aa3e33) |
| Phase 1.4a | Phase 2.1a | OpenAI-Oath-Provider VPS2 |

Rationale: Phases 1.4 and 1.4a are post-install enhancements, not required for a
functional agent. Moving them to Phase 2 keeps Phase 1 focused on getting the agent
running and responding.

### Phase 2.1 -- openclaw-llm-model-picker (Notion ID: 13aa3e33)

Rewritten for VPS2 folder-install:
- Removed all Docker/VPS1 commands
- Documents the Phase 1.1 minimal model vs Phase 2.1 full 63-model list
- VPS2 update method: SSH + Python script
- Full model table: 5 Codex + 58 OpenRouter
- Troubleshooting: codex harness error, stale browser cache

### Phase 2.1a -- OpenAI-Oath-Provider VPS2

Rewritten with two clear paths:
- **Path A (fast, ~2 min, Manus task):** Copy auth-profiles.json from existing agent on same VPS
  - `cp /root/.openclaw-[existing]/auth-profiles.json /root/.openclaw-[new]/`
  - `cp /root/.openclaw-[existing]/auth-state.json /root/.openclaw-[new]/`
  - `systemctl restart openclaw-[new]`
- **Path B (5 min, human task):** Fresh device code login via browser
- `@openclaw/codex` npm plugin requirement documented
- Token expiry tracking (June 6, 2026 for current VPS2 token)

### Step 12b -- Memory Setup (All Four Phase 1.1 SOPs)

Added to:
- VPS2 Phase 1.1-agent (3e4a3e33)
- VPS2 Phase 1.1-human (04ea3e33)
- VPS1 Phase 1.1-agent (c3fa3e33)
- VPS1 Phase 1.1-human (287a3e33)

Inserted between Step 12 (HTTPS verify + test message) and Step 13 (openclaw doctor).

Covers:
- Add memory-core, memory-wiki, active-memory plugins to openclaw.json
- Create wiki vault directory structure
- VPS1 version includes Docker alpine chown workaround for UID 1000
- Verify all three: dreaming schedule, vault ready, entities/index.md

---

## Root Cause -- Why These SOP Updates Were Needed

Suzy's install on 2026-06-01 exposed several gaps in the existing VPS2 SOP:

1. `--accept-risk` flag does not exist in OpenClaw 2026.5.28 -- SOP had stale command
2. Default model `openai/gpt-4o` is not available on VPS2 -- agents couldn't respond after install
3. `@openclaw/codex` npm plugin not in SOP -- caused "harness not registered" error
4. Memory setup not in Phase 1 -- agents installed without dreaming or knowledge vault
5. Phase 1.4/1.4a were blocking Phase 1 completion unnecessarily
