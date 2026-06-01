# 2026-05-31 - Fleet Upgrade to v2026.5.28 + Permission Fixes + Branding Fixes + systemd Services

## Summary

- **Date:** 2026-05-31 Mountain Time
- **Changed By:** Manus
- **VPS:** main-vps (187.77.210.223)
- **Affected Agents:** All 10 -- terry, amanda, victor, wilma, inga, marsha, gohzed, grogar, maggie, vivian
- **Status:** done

## Why This Was Done

Multiple compounding issues were found across the fleet:

- Agents were hanging mid-task due to context overflow (no context limit set, unlimited token accumulation)
- Workspace files were owned by UID 1001 (jackadmin) or UID 0 (root) instead of UID 1000 (node), blocking agents from writing to their own workspace
- Branding/avatar copy was silently failing because `post-start.sh` still referenced the old pre-2026.5.26 container path
- All agents needed to be upgraded from v2026.5.26 to v2026.5.28
- systemd services were missing on several agents, meaning secrets were not injected on boot or crash recovery

## What Changed

### Image Upgrade

- Updated `OPENCLAW_IMAGE` in `.env` for all 10 agents from `2026.5.26` to `ghcr.io/openclaw/openclaw:2026.5.28`
- Pulled new image and restarted all containers

### Workspace Permission Fix

- All 10 agent workspaces were owned by UID 1001 (jackadmin) or UID 0 (root)
- Fixed using the alpine Docker workaround (runs as root inside container, no sudo needed):
  ```bash
  docker run --rm -v /opt/openclaw/agents/{agent}/workspace:/ws alpine chown -R 1000:1000 /ws
  ```
- Confirmed all workspace files now owned by 1000:1000 (node user)
- Root cause: jackadmin (UID 1001) had been writing files directly to workspace bind mounts

### Branding Path Fix (post-start.sh)

- Old path (pre-2026.5.26): `/home/node/.openclaw/branding/`
- New path (2026.5.26+): `/app/dist/control-ui/branding/`
- Updated `BRANDING_DIR` in `post-start.sh` for all 10 agents
- Avatars and favicons now copy correctly on every container start

### systemd Services

- Created `/etc/systemd/system/openclaw-{agent}.service` for all 10 agents
- Service type: `oneshot` with `RemainAfterExit=yes`
- `ExecStart` calls `op run --env-file=.env -- docker compose up -d` to inject 1Password secrets on every boot and crash recovery
- `Restart=on-failure` with 30s delay
- All 10 services enabled and active: `systemctl enable --now openclaw-{agent}`

### Doctor Fix

- Ran `openclaw doctor --fix` twice on all 10 agents
- Doctor migrated `openai-codex/*` model refs to `openai/*` with `agentRuntime.id: codex`
- Second run confirmed clean (cosmetic "Unknown model" warning on first run is expected -- see Notion Maintenance page)

## Files Changed

- `/opt/openclaw/agents/{agent}/.env` -- OPENCLAW_IMAGE updated (all 10)
- `/opt/openclaw/agents/{agent}/workspace/post-start.sh` -- BRANDING_DIR path updated (all 10)
- `/opt/openclaw/agents/{agent}/config/openclaw.json` -- model refs migrated by doctor (all 10)
- `/etc/systemd/system/openclaw-{agent}.service` -- created (all 10)

## Commands Run (per agent, repeated for all 10)

```bash
# Permission fix
docker run --rm -v /opt/openclaw/agents/{agent}/workspace:/ws alpine chown -R 1000:1000 /ws

# Image upgrade
sed -i 's/2026.5.26/ghcr.io\/openclaw\/openclaw:2026.5.28/' /opt/openclaw/agents/{agent}/.env
cd /opt/openclaw/agents/{agent} && docker compose pull && docker compose up -d

# Branding fix
sed -i 's|BRANDING_DIR=.*|BRANDING_DIR=/app/dist/control-ui/branding|' /opt/openclaw/agents/{agent}/workspace/post-start.sh

# Doctor
docker exec {agent} openclaw doctor --fix
docker exec {agent} openclaw doctor --fix

# systemd service
systemctl enable --now openclaw-{agent}
```

## Test Result

- All 10 agents confirmed healthy via `docker ps` -- status `Up`, no `Exited` or `Restarting`
- All 10 systemd services confirmed active via `systemctl status openclaw-{agent}`
- Branding path verified via `grep BRANDING_DIR` on each post-start.sh
- Workspace ownership verified via `ls -la` on each workspace
- Doctor ran clean (second pass) on all 10 agents

## Known Open Items (not blocking)

- Wilma: `wordpress-allzed` MCP fails to connect -- pre-existing issue, not introduced this session
- Doctor "Unknown model" warning on first run -- cosmetic, by design per 2026.5.28 (see Notion Maintenance page)
- Minor file cleanup still needed: `token.txt` (terry, vivian), `x` directory (terry), `openclaw_fixed.json` (inga, vivian), `test.txt` (wilma, inga), `apply_cfg.py` directory (amanda)
- npm volume mount fix still needed: add `./npm:/home/node/.openclaw/npm` to each `docker-compose.yml` to persist Discord plugin and Asana MCP across restarts
- Security items flagged by Terry healthcheck skill: `dangerouslyDisableDeviceAuth=true`, Discord DMs open, gateway auth token stored as plaintext, credentials dir mode 755

## Rollback Note

- To roll back image: change `OPENCLAW_IMAGE` back to `ghcr.io/openclaw/openclaw:2026.5.26` in `.env` and run `docker compose up -d`
- To roll back branding path: change `BRANDING_DIR` back to `/home/node/.openclaw/branding/` in `post-start.sh`
- systemd services can be disabled with `systemctl disable --now openclaw-{agent}` and removed from `/etc/systemd/system/`

## Links

- GitHub Issue: #1 (fleet rollout -- now complete, issue updated)
- Notion Maintenance page: https://www.notion.so/Maintenance-91ea3e33d58183f3b2948111b513e531
- VPS path: `/opt/openclaw/agents/`
