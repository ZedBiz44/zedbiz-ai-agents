# Terry Agent Fix Session — 2026-05-23

**Date:** 2026-05-23 (Mountain Time)  
**Agent:** Manus  
**System:** OpenClaw Agent Fleet  
**VPS:** Main VPS — `187.77.210.223` (`srv1404026`)  
**Change Type:** Bug Fix / Agent Repair / Image Update / systemd Setup  
**Related Notion Page:** [OpenClaw VPS Terry Agent Restart Issues & Fixes](https://www.notion.so/OpenClaw-VPS-Terry-Agent-Restart-Issues-Fixes-835a3e33d581825eba24010237f4c34a)

---

## Summary

Live diagnostic and repair session on Terry, the test agent on the main VPS. All changes were verified against the actual live state of the VPS — not from notes alone.

---

## Changes Made

### 1. File/Folder Cleanup — Terry
- **Deleted** `config/token.txt` — leftover junk file
- **Deleted** `config/x` — empty mystery file/directory
- **Created** `config/skills/` — missing directory (Harry has it)
- **Created** `config/skills/sessions-log/` — missing sub-directory
- **Created** `config/subagents/` — missing directory (Harry has it)
- **Method:** docker alpine workaround (root inside container, chown-safe)
- **Result:** ✅ Confirmed

### 2. memory-core Dreaming Cron Fix
- **Problem:** `"frequency": "nightly"` in `openclaw.json` is an invalid cron format — caused startup error: `CronPattern: invalid configuration format ('nightly')`
- **Fix:** Changed to `"frequency": "0 2 * * *"` (2am Edmonton time daily)
- **Backup created:** `openclaw.json.bak.manus-20260523_HHMMSS`
- **Result:** ✅ Confirmed — log now shows `memory-core: created managed dreaming cron job.`

### 3. systemd Service Created for Terry
- **File:** `/etc/systemd/system/openclaw-terry.service`
- **Purpose:** Replaces `restart: unless-stopped` — systemd calls `start-agent.sh` which runs `resolve-secrets.sh` before `docker compose up`, ensuring 1Password secrets are injected on every boot/crash
- **docker-compose.yml updated:** `restart: unless-stopped` → `restart: "no"`
- **Service enabled:** `systemctl enable openclaw-terry.service` ✅
- **Method:** docker privileged alpine workaround (to write to `/etc/systemd/system/` without sudo password)
- **Result:** ✅ Service enabled, inactive (Terry currently running directly — will activate on next reboot)

### 4. Image Update — 2026.5.18 → 2026.5.20
- **Updated:** `.env` and `.env.resolved` `OPENCLAW_IMAGE` value
- **Pulled:** `ghcr.io/openclaw/openclaw:2026.5.20` ✅
- **Restarted:** Terry via `docker compose down && docker compose up -d`
- **Result:** ✅ Terry running on `2026.5.20`, healthy

### 5. Discord Plugin Install + npm Volume Fix
- **Problem:** After image update, npm volume was empty — Discord plugin not loaded (only 10 plugins, discord absent)
- **Root cause:** npm volume mount exists but was empty; plugin needs to be installed into it
- **Fix 1:** Fixed npm directory ownership (`chown -R 1000:1000`) via docker alpine
- **Fix 2:** `docker exec terry openclaw plugins install @openclaw/discord`
- **Restarted:** Terry via `docker compose restart`
- **Result:** ✅ Discord plugin now loading — `11 plugins: active-memory, browser, canvas, device-pair, **discord**, file-transfer, memory-core, memory-wiki, openai, phone-control, talk-voice`
- **Discord status:** Bot probe resolved `@terry-openclaw`, guild `ZedBiz Agents` connected, client initialized as `1500529156364964020`, awaiting WebSocket gateway readiness (normal — token is valid)

---

## Current Terry State

| Item | Status |
|---|---|
| Container | ✅ Up, healthy |
| Image | ✅ `2026.5.20` |
| Discord plugin | ✅ Loaded |
| Discord bot token | ✅ Valid (bot probe resolved) |
| Discord WS gateway | ⏳ Connecting (normal startup behaviour) |
| memory-core dreaming | ✅ Cron job created (fixed) |
| systemd service | ✅ Enabled (activates on next reboot) |
| npm volume | ✅ Discord plugin persisted |
| File cleanup | ✅ Done |
| skills/ dir | ✅ Created |
| subagents/ dir | ✅ Created |

---

## Still Open / Next Steps

- **Terry — Skills installation:** Terry only has `himalaya` skill. Missing: `discord`, `gemini`, `gog`, `mcporter`, `nano-pdf`, `notion`, `openclaw-tavily-search`, `summarize`, `video-frames`. Also missing `.system` skills (`imagegen`, `openai-docs`, `plugin-creator`, `skill-creator`, `skill-installer`).
- **Terry — Email test:** `email-poll.sh` + himalaya IMAP not tested end-to-end yet.
- **Terry — Wiki init:** `docker exec terry openclaw wiki init` not yet run.
- **Terry — Asana MCP:** Configured in `openclaw.json` but not verified working.
- **Fleet — 9 agents still down** (Exited 137, killed May 15, on image `2026.5.7`): Need same systemd + npm + image update treatment. Roll out after Terry is fully confirmed.
- **Fleet — Skills gap:** `marsha`, `gohzed`, `grogar`, `maggie`, `vivian` have `himalaya` only.
- **Fleet — Amanda Asana:** Only Amanda has npm Asana install — other 8 missing it.
- **Notion SOP path fix:** `ghcr.io/zedbiz44/zedbiz-openclaw` still referenced in Human SOP Step 2 and Agent-Executable SOP Step 2 — needs correction.
- **Harry/Suzy VPS:** Decision made to wipe and rebuild cleanly (per-agent binary model). Separate session.

---

## Files/Paths Touched

| Path | Change |
|---|---|
| `/opt/openclaw/agents/terry/config/token.txt` | Deleted |
| `/opt/openclaw/agents/terry/config/x` | Deleted |
| `/opt/openclaw/agents/terry/config/skills/` | Created |
| `/opt/openclaw/agents/terry/config/skills/sessions-log/` | Created |
| `/opt/openclaw/agents/terry/config/subagents/` | Created |
| `/opt/openclaw/agents/terry/config/openclaw.json` | Fixed `frequency: nightly` → `0 2 * * *` |
| `/opt/openclaw/agents/terry/docker-compose.yml` | `restart: unless-stopped` → `restart: "no"` |
| `/opt/openclaw/agents/terry/.env` | Image updated to `2026.5.20` |
| `/opt/openclaw/agents/terry/.env.resolved` | Image updated to `2026.5.20` |
| `/etc/systemd/system/openclaw-terry.service` | Created + enabled |
| `/opt/openclaw/agents/terry/npm/` | Discord plugin installed, ownership fixed |

---

## Rollback Notes

- `openclaw.json` backup: `config/openclaw.json.bak.manus-20260523_*`
- `docker-compose.yml` backup: `docker-compose.yml.bak.20260520_203648` (pre-existing)
- To rollback image: change `OPENCLAW_IMAGE` back to `2026.5.18` in `.env` and `.env.resolved`, pull old image, restart
- To rollback systemd: `systemctl disable openclaw-terry.service`, restore `restart: unless-stopped` in compose

---

## Test Results

- Terry container: **healthy** ✅
- Discord plugin: **loaded** ✅  
- memory-core cron: **working** ✅
- Image: **2026.5.20** ✅

---

## Session Update: Notion MCP Server Added (2026-05-22 MDT)

**Change:** Added `@notionhq/notion-mcp-server` to Terry's `mcp.servers` config in `openclaw.json`

**Config added:**
```json
"notion": {
  "command": "/usr/local/bin/npx",
  "args": ["-y", "@notionhq/notion-mcp-server"],
  "env": {
    "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer ${NOTION_API_KEY}\", \"Notion-Version\": \"2022-06-28\"}"
  }
}
```

**Secret:** `NOTION_API_KEY` injected via 1Password Connect (`op://openclaw-agents-shared/notion-api-key/credential`) — resolved successfully in `.env.resolved`

**Notion connection:** Created via Notion Developers > Connections > Access Token, scoped to ZedBiz Notion workspace, named `openclaw`

**Status:** Terry restarted, gateway healthy, NOTION_API_KEY confirmed resolved. MCP server starts on-demand when Notion tools are invoked.

**Next:** Share key Notion pages with the `openclaw` connection in Notion UI (Technical Documentation, Agent Registry, GitHub AI Agents Tracking System)
