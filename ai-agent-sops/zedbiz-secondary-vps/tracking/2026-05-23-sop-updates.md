# SOP Updates — Native Install Method (VPS-2)

**Date:** 2026-05-23
**Updated by:** Manus

## Pages Updated in Notion

| Page | Notion Link | Change |
|------|-------------|--------|
| ai-agent-base-build-sop | https://www.notion.so/3e4a3e33d5818352bdb701eed29da450 | Fully rewritten for native systemd install (no Docker) |
| human-agent-base-build-sop | https://www.notion.so/04ea3e33d5818318b4ae812c80a98da3 | Fully rewritten for native systemd install (human version) |

## What Changed

The old SOPs were based on the Docker/Caddy/1Password stack used on VPS-1. The new SOPs reflect the native Node.js + systemd install method used for Harry and Edith on VPS-2.

### Key Differences

| Item | Old (VPS-1 Docker) | New (VPS-2 Native) |
|------|--------------------|--------------------|
| Install method | docker compose up | openclaw gateway install |
| Config location | /opt/openclaw/agents/[name]/config/ | /root/.openclaw-[name]/ |
| Service management | Docker restart | systemctl restart openclaw-[name] |
| Memory limit | Docker resource limits | MemoryMax=512M in systemd |
| Port range | 3000s | 18789+ |
| Caddy routing | Required | Not required (direct IP:port) |
| 1Password | Required for secrets | Not yet (Phase 2) |

## SOP Steps (Native)

0. Define variables (AGENT_ID, AGENT_NAME, AGENT_PORT)
1. openclaw gateway install --name openclaw-[name] --port [port] --home /root/.openclaw-[name]
2. Add MemoryMax=512M to systemd service, daemon-reload
3. Write openclaw.json (bind: lan, token, dangerouslyDisableDeviceAuth: true)
4. systemctl enable + restart
5. Final validation
6. Connect via browser at http://[VPS-IP]:[port]
