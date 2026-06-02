# 2026-06-02 Terry Model And 1Password Connect Health Fix

## Guide Name

VPS1 live OpenClaw fleet operations.

## Issue

- Terry was not responding in chat.
- Multiple agents showed red health-check command banners.
- `1password-connect-api` showed Docker status `unhealthy`.

## Old Assumption

- The visible red banners could be read as all agents failing.
- `1password-connect-api` being Docker-unhealthy could be read as the 1Password Connect service being down.

## Tested Correction

- Terry's container was healthy, but Terry's model request was failing before content.
- Terry was the model outlier, using `openrouter/deepseek/deepseek-v4-pro` while the rest of the checked fleet defaulted to `openai/gpt-5.5`.
- Terry logs showed OpenRouter rejecting the request because `reasoning_effort` and `reasoning.effort` were both sent with conflicting values.
- Terry was changed to `agents.defaults.model = openai/gpt-5.5` and restarted.
- The 1Password Connect API service itself answered `GET /health` with HTTP 200.
- The Docker health check was invalid for the image because it used `CMD-SHELL` with `curl`, but the `1password/connect-api` image has no `/bin/sh` or `curl`.
- The invalid health-check block was removed from `/opt/openclaw/1password-connect/docker-compose.yml`, then `connect-api` was recreated.

## Verification

- Terry restarted and returned Docker health `healthy`.
- Terry gateway log after restart showed `agent model: openai/gpt-5.5` and `gateway ready`.
- `1password-connect-api` no longer showed Docker `unhealthy`.
- `http://127.0.0.1:8080/health` returned HTTP 200 from the VPS host.

## Agent Or Person Who Verified It

Cody verified live on VPS1.

## Date Verified

2026-06-02 Mountain Time.

## VPS Agent

- Terry
- 1Password Connect API on VPS1

## Files Changed On VPS1

- `/opt/openclaw/agents/terry/config/openclaw.json`
- `/opt/openclaw/1password-connect/docker-compose.yml`

## Backups Created On VPS1

- Terry config backup: `/home/node/.openclaw/openclaw.json.bak-terry-model-fix-20260602T184743Z`
- 1Password compose backup: `/opt/openclaw/1password-connect/docker-compose.yml.bak-healthcheck-fix-20260602T184931Z`

## Rollback Note

- Terry rollback: restore the Terry backup file and restart Terry.
- 1Password Connect rollback: restore the compose backup, then run Docker Compose from `/opt/openclaw/1password-connect`.

## Follow-Up

- Update the health-check workflow so ordinary agents do container-safe checks only.
- Let Victor run host/VPS checks, because Victor now has Docker CLI and server-operator access.
- Do not interpret missing `ss`, `netstat`, `systemctl`, Docker CLI, or host apt files inside normal agent containers as an agent outage.
