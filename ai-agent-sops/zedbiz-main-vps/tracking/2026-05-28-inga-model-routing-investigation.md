# 2026-05-28 Inga Model Routing Investigation

**Date:** 2026-05-28 (MST)
**Agent:** Manus
**Scope:** Inga (test agent) -- model routing for openai/* models via API key

---

## Context

Following the fleet-wide upgrade and OAuth rollout session, the goal was to make all model picker options work correctly. The known issue: `openai/*` models (gpt-4o, gpt-5.5-pro, etc.) were failing because OpenClaw routes them through Codex OAuth by default, and Codex does not recognize non-Codex model IDs.

---

## Test: pi Runtime Approach (FAILED)

**Hypothesis:** Adding `agentRuntime: {id: "pi"}` to `openai/*` model entries would force routing through the OpenAI API key instead of Codex OAuth.

**What was done:**
- Added `openai/gpt-4o` with `agentRuntime: {id: "pi"}` to Inga's model list
- Hot reload applied successfully
- User tested gpt-4o from the model picker

**Result:** FAILED
- Error: `Unknown model: openai-codex/gpt-4o`
- Root cause: OpenClaw ignored the `pi` agentRuntime override and still routed through Codex, then failed because Codex does not know `gpt-4o`
- `pi` is an internal pi-ai catalog name, NOT a valid user-facing agentRuntime id

---

## Research Findings: OpenClaw Docs

From `https://docs.openclaw.ai/concepts/agent-runtimes` and `https://docs.openclaw.ai/concepts/model-providers`:

**Key architecture facts:**
- `openai/*` model refs ALWAYS route through the Codex app-server runtime by default -- this is intentional design
- The Codex app-server runtime uses `openai-codex` auth profiles (ChatGPT OAuth)
- `openai-codex/*` model refs are LEGACY -- the docs say to use `openai/*` with Codex runtime
- Valid `agentRuntime.id` values for user config: `openclaw`, `codex`, `claude-cli`
- `pi` is NOT a valid user-facing agentRuntime id
- To use OpenClaw's embedded runner (which can use API key): `agentRuntime.id: "openclaw"`
- When `openclaw` runtime is paired with `openai-codex` auth profile, it routes through Codex-auth transport

**Key quote from docs:**
> "Selecting `openai/*` for an agent model now means 'run this through Codex' unless a non-agent OpenAI API surface is being used."

**Implication for gpt-5.5-pro:**
- `openai/gpt-5.5-pro` with `agentRuntime: {id: "openclaw"}` should route through OpenClaw's embedded runner
- This may work if the OpenClaw runner can handle gpt-5.5-pro via the Codex-auth transport
- However, gpt-5.5-pro availability depends on ChatGPT Pro/Business/Enterprise subscription tier

---

## Fix Applied to Inga

1. **Removed** `openai/gpt-4o` (test entry, no longer needed)
2. **Updated** `openai/gpt-5.5-pro`: changed `agentRuntime.id` from `"pi"` to `"openclaw"`
3. Hot reload confirmed: `agents.defaults.models.openai/gpt-4o, agents.defaults.models.openai/gpt-5.5-pro.agentRuntime.id`

**Inga's current openai/* model entries:**
```json
"openai/gpt-5.5-pro": {
    "agentRuntime": {
        "id": "openclaw"
    }
}
```

---

## Good News: openai-codex/* Models Working

User confirmed all openai-codex/* models are working correctly:
- `openai-codex/gpt-5.5` (default) -- working
- `openai-codex/gpt-5.4` -- working
- `openai-codex/gpt-5.4-mini` -- working
- `openai-codex/gpt-5.3-codex` -- working (restored from previous session)
- `openai-codex/gpt-5.3-codex-spark` -- working (restored from previous session)
- `openai-codex/gpt-5.2` -- not yet tested (empty entry, user to test)

---

## Status

| Model | Status | Notes |
|---|---|---|
| openai-codex/gpt-5.5 | Working | Default model |
| openai-codex/gpt-5.4 | Working | |
| openai-codex/gpt-5.4-mini | Working | |
| openai-codex/gpt-5.3-codex | Working | |
| openai-codex/gpt-5.3-codex-spark | Working | |
| openai-codex/gpt-5.2 | Untested | Empty entry |
| openai/gpt-5.5-pro | Pending test | agentRuntime: openclaw -- user to test |
| openrouter/* (63 models) | Untested | Need OpenRouter API key configured |

---

## Next Steps

- User to test `openai/gpt-5.5-pro` with the new `openclaw` runtime setting
- If gpt-5.5-pro still fails, it may be a subscription tier issue (Pro/Business/Enterprise only)
- OpenRouter models need an OpenRouter API key in the .env to work
- Once Inga's full model picker is validated, roll the final model list to all 9 other agents

---

## Files Changed

- `/opt/openclaw/agents/inga/config/openclaw.json` -- removed gpt-4o test entry, fixed gpt-5.5-pro agentRuntime
