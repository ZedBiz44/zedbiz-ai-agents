# 2026-06-01 - Victor Node Runtime and Docker Admin Access Fix

## Summary

- **Date:** 2026-06-01 Mountain Time
- **Changed By:** Codex
- **VPS:** VPS1 / main-vps `187.77.210.223`
- **Affected Agent:** Victor
- **Status:** done

## Business Reason

Victor is the IT and server specialist for the VPS1 OpenClaw agent team. He needed clean diagnostic behavior and practical Docker-level server control without keeping the OpenClaw gateway defaulting to root.

## Problem

- Victor was the only VPS1 agent with `user: root` in `docker-compose.yml`.
- Plain `docker exec victor openclaw ...` ran as root and read `/root/.openclaw`, which created false diagnostic results.
- Victor had SSH only because startup ran `apt-get install openssh-client` as root each time.
- Simply removing `user: root` would have removed that install step and risked breaking Victor's server-tooling setup.
- Terry had `/var/run/docker.sock` mounted, but the useful part was Docker socket access, not root as the default OpenClaw user.

## Tested Correction

- Added `Dockerfile.victor` based on `ghcr.io/openclaw/openclaw:2026.5.28`.
- Installed `openssh-client` in the image build layer.
- Installed the static Docker CLI `29.5.2` in the image build layer.
- Removed `user: root` from Victor's compose service.
- Changed Victor startup command to run directly as `node`, without `su node`.
- Moved Himalaya config mount from `/root/.config/himalaya` to `/home/node/.config/himalaya`.
- Mounted `/var/run/docker.sock:/var/run/docker.sock`.
- Added `group_add: "988"` so the `node` user can actually access the host Docker socket.
- Rebuilt and recreated only the `victor` container.

## Verification

- Victor container status: `running healthy`.
- Container image: `zedbiz-openclaw-victor:2026.5.28-ssh`.
- Container default user: `node`.
- Gateway process user: `node`.
- SSH available inside Victor: `/usr/bin/ssh`.
- Docker CLI available inside Victor: `Docker version 29.5.2`.
- Docker socket mounted at `/var/run/docker.sock`.
- Victor `node` user groups include Docker socket group `988`.
- Docker socket test from inside Victor returned `OK`.
- `docker ps` from inside Victor listed the VPS1 containers.
- Docker container list through socket returned `19` containers.
- Plain `docker exec victor openclaw memory status` now reads the real `/home/node/.openclaw` config and shows dreaming enabled.

## Backups

- `/opt/openclaw/agents/victor/docker-compose.yml.bak-victor-node-docker-20260602T000036Z`
- `/opt/openclaw/agents/victor/docker-compose.yml.bak-victor-groupadd-20260602T000306Z`
- `/opt/openclaw/agents/victor/Dockerfile.victor.bak-docker-cli-20260602T001900Z`

## Rollback Note

Restore the latest backup compose file, then recreate Victor:

```bash
cd /opt/openclaw/agents/victor
cp docker-compose.yml.bak-victor-node-docker-20260602T000036Z docker-compose.yml
docker compose up -d --force-recreate victor
```

If rolling back fully, remove or ignore the local image `zedbiz-openclaw-victor:2026.5.28-ssh`.

## Follow-Up

- Victor now has effective Docker host control through the raw Docker socket. Treat this as high-trust admin access.
- Terry also has the raw Docker socket mounted, but should be separately checked for whether its `node` user can actually access it.
- Victor now has both Docker API access and the Docker CLI binary.
- Optional future Level 2: give Victor SSH access back to the VPS host as a real server user such as `jackadmin`. That would let Victor edit host files, run compose from host paths, and use the Alpine permission workaround directly. Do not add this unless Victor truly needs host-filesystem power beyond Docker fleet control.

## Why Victor Has Extra Power Compared To Other Agents

Victor is the IT and server specialist. The extra access is intentional and should not be copied to every agent.

| Upgrade | Why Victor Has It | Why Other Agents Usually Should Not |
|---|---|---|
| Custom local image | Keeps Victor's server tools stable after restart | Most agents do not need server tooling |
| `openssh-client` | Lets Victor perform server/admin workflows that require SSH | Most agents only need app/channel tools |
| Docker socket mount | Lets Victor manage the VPS1 container fleet | This is high-trust host control |
| Docker socket group `988` | Makes socket access actually work while Victor still runs as `node` | Unneeded unless the agent is trusted for Docker admin |
| Docker CLI `29.5.2` | Gives Victor normal commands like `docker ps`, `docker logs`, and `docker restart` | Gives too much operational control to non-admin agents |
| Removed `user: root` | Fixes false diagnostics and keeps OpenClaw running in the normal `node` config space | This matches the clean runtime standard |
