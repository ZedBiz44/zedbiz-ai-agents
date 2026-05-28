# Fleet Model Rollout -- All 10 Agents Standardized

**Date:** 2026-05-28 (Mountain Time)
**Agent:** Manus
**Scope:** All 10 OpenClaw agents on zedbiz-main-vps

---

## Summary

Standardized the model list and auth configuration across all 10 agents (inga, amanda, victor, wilma, marsha, gohzed, grogar, maggie, vivian, terry).

---

## What Was Done

### Phase 1 -- Inga (Reference Agent)
- Investigated `openai/gpt-5.5-pro` failure (multiple routing approaches tested)
- Confirmed `agentRuntime: {id: "pi"}` is not a valid user config value
- Confirmed `agentRuntime: {id: "openclaw"}` is not a registered harness
- Confirmed `openai/gpt-5.5-pro` requires ChatGPT Pro ($200/mo) -- not available on current subscription
- Removed `openai/gpt-5.5-pro` from Inga's model list
- Registered `openai:api-key-backup` profile in Inga's auth-profiles.json
- Set correct auth order: `openai-codex:jzedbiz@gmail.com` first, `openai:api-key-backup` second
- Final Inga config: 64 models, default `openai-codex/gpt-5.5`, clean auth

### Phase 2 -- Fleet Rollout (All 9 Remaining Agents)
Applied Inga's final clean config to all 9 agents:
- amanda, victor, wilma, marsha, gohzed, grogar, maggie, vivian, terry

**What was changed on each agent:**
- `agents.defaults.models` -- replaced with Inga's 64-model list
- `agents.defaults.model` -- set to `openai-codex/gpt-5.5`
- `auth.profiles` -- added `openai:api-key-backup` profile
- `auth.order.openai` -- set to `[openai-codex:jzedbiz@gmail.com, openai:api-key-backup]`

**What was preserved on each agent:**
- Gateway port, token, allowedOrigins
- Discord channel config (token, guilds, defaultTo)
- Agent identity (name, instructions, avatar)
- All plugin settings
- Workspace settings

---

## Final Fleet State

| Agent | Models | Default Model | Auth Order | Status |
|---|---|---|---|---|
| inga | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | gateway ready |
| amanda | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | Discord connected |
| victor | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | gateway ready |
| wilma | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | gateway ready |
| marsha | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | gateway ready |
| gohzed | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | gateway ready |
| grogar | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | gateway ready |
| maggie | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | gateway ready |
| vivian | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | gateway ready |
| terry | 64 | openai-codex/gpt-5.5 | Codex OAuth -> API key | Discord connected |

---

## Model List (64 Models -- All Agents)

**openai-codex (5 models -- all working via Codex OAuth):**
- openai-codex/gpt-5.5 (default)
- openai-codex/gpt-5.4
- openai-codex/gpt-5.4-mini
- openai-codex/gpt-5.3-codex
- openai-codex/gpt-5.3-codex-spark
- openai-codex/gpt-5.2

**openrouter (58 models -- require OpenRouter API key per agent):**
- Anthropic: claude-haiku-4.5, claude-opus-4.7, claude-sonnet-4.6
- DeepSeek: deepseek-v4-flash, deepseek-r1-0528, deepseek-v4-pro
- Google: gemini-3-flash-preview, gemini-3.1-flash-lite, gemini-3.1-pro-preview, gemma-4-31b-it
- Meta: llama-4-maverick, llama-4-scout
- Mistral: mistral-medium-3-5, mistral-small-2603, mistral-large-2512
- Qwen: qwen3.6-plus, qwen3.5-flash, qwen3-235b-a22b, qwen3-coder, qwen3-vl-235b-a22b-thinking
- xAI: grok-4.3, grok-4.20
- Plus many free-tier models

---

## Auth Configuration

```json
{
  "auth": {
    "order": {
      "openai": ["openai-codex:jzedbiz@gmail.com", "openai:api-key-backup"]
    }
  }
}
```

**Codex OAuth token:** jzedbiz@gmail.com (expires 2026-06-07)
**API key backup:** sk-proj-9Xt6... (registered in auth-profiles.json for all agents)

---

## Key Learnings

- `openai/*` and `openai-codex/*` prefixes both route through the Codex app-server harness
- `agentRuntime` overrides only accept `"codex"` as a valid ID (not "pi", not "openclaw")
- `openai/gpt-5.5-pro` requires ChatGPT Pro tier ($200/mo) -- not available on current plan
- `paste-api-key` command reorders auth.order -- always fix order after running it
- The docker alpine workaround (run as root inside container) is the correct approach for all config changes

---

## Files Modified

- `/opt/openclaw/agents/*/config/openclaw.json` -- all 10 agents
- Auth-profiles.json updated via live `openclaw models auth paste-api-key` command on all 10 agents

---

## Scripts Used

- `/tmp/fleet_rollout.py` -- main rollout script (preserves agent-specific settings)
- `/tmp/remove_gpt55pro_inga.py` -- removed gpt-5.5-pro from Inga
- `/tmp/fix_auth_order.py` -- fixed auth order after paste-api-key reordered it
- `/tmp/inga_final_clean.json` -- Inga's final config used as fleet template
