# VPS1 Fleet: v2026.5.26 Upgrade + OpenAI OAuth Rollout
**Date:** 2026-05-28 (MST)
**Session:** Manus
**Status:** COMPLETED

---

## Actions Completed

### 1. Fleet Upgrade to v2026.5.26
- Pulled `ghcr.io/openclaw/openclaw:2026.5.26` on VPS1
- Updated `.env` and `.env.resolved` for all 10 agents
- Note: amanda, victor, wilma, inga required docker alpine workaround for file permission write
- All 10 agents restarted and confirmed healthy on new image

| Agent | Previous Version | New Version | Status |
|---|---|---|---|
| terry | 2026.5.22 | 2026.5.26 | healthy |
| amanda | 2026.5.20 | 2026.5.26 | healthy |
| victor | 2026.5.20 | 2026.5.26 | healthy |
| wilma | 2026.5.20 | 2026.5.26 | healthy |
| inga | 2026.5.20 | 2026.5.26 | healthy |
| marsha | 2026.5.20 | 2026.5.26 | healthy |
| gohzed | 2026.5.20 | 2026.5.26 | healthy |
| grogar | 2026.5.20 | 2026.5.26 | healthy |
| maggie | 2026.5.20 | 2026.5.26 | healthy |
| vivian | 2026.5.20 | 2026.5.26 | healthy |

### 2. Discord Plugin Auto-Install (v2026.5.26 Behavior)
- On first boot with v2026.5.26, victor, wilma, and inga triggered auto-install of `@openclaw/discord` plugin
- This is expected behavior -- the plugin was not bundled in 2026.5.20 for these agents
- All 3 agents successfully installed the plugin and connected to Discord gateway

### 3. Discord Issue Root Cause: Message Content Intent
- Identified that 9 of 10 bot applications were missing **Message Content Intent** in the Discord Developer Portal
- This is a privileged gateway intent that must be enabled per-bot at: https://discord.com/developers/applications
- Without it, bots connect to Discord but cannot read message content -- silent non-response
- **Action required by Jack:** Enable Message Content Intent + Server Members Intent for each bot in the portal

Bot Application IDs requiring portal fix:
- terry: `1500529156364964020`
- victor: `1492954584937664613`
- wilma: `1492893086827806881`
- inga: `1492938281736868000`
- marsha: `1503638505635713135`
- gohzed: `1492945213822341140`
- grogar: `1492941865484292218`
- maggie: `1492950782839095346`
- vivian: `1500533551861469365`

### 4. OpenAI OAuth Rollout -- All 10 Agents
- Jack completed OAuth device-code login for Marsha via: `docker exec -it marsha openclaw models auth login --provider openai-codex --device-code`
- OAuth profile: `openai-codex:jzedbiz@gmail.com` (expires 2026-06-07)
- `auth-profiles.json` copied from Marsha to all 9 remaining agents using docker alpine workaround
- File ownership corrected to UID 1000 (node user) on all agents
- `agents.defaults.model.primary` set to `openai/gpt-5.5` on all 10 agents
- `auth.order.openai` set to `["openai-codex:jzedbiz@gmail.com", "openai:api-key-backup"]` on all 10 agents
- Victor required direct JSON file write (config set debounce timing issue on rapid restart)
- All 10 agents confirmed running `openai/gpt-5.5` as active model

### 5. Auth Priority Chain (All 10 Agents)
1. ChatGPT subscription (OAuth) via `jzedbiz@gmail.com` -- free usage from subscription
2. OpenAI API key (`OPENAI_API_KEY` env var) -- paid fallback
3. OpenRouter -- available via model picker or `/model openrouter/...`

---

## Outstanding Items
- Jack still needs to enable **Message Content Intent** in Discord Developer Portal for 9 bot applications (listed above)
- OAuth token expires 2026-06-07 -- will need renewal via device-code login on one agent + re-copy to others

---

## Files Modified
- All 10 agents: `/opt/openclaw/agents/<agent>/.env` -- OPENCLAW_IMAGE updated
- All 10 agents: `/opt/openclaw/agents/<agent>/.env.resolved` -- OPENCLAW_IMAGE updated
- All 10 agents: `/opt/openclaw/agents/<agent>/config/openclaw.json` -- model.primary + auth.order.openai
- All 10 agents: `/opt/openclaw/agents/<agent>/config/agents/main/agent/auth-profiles.json` -- OAuth token deployed
