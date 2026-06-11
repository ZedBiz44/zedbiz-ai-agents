# 2026-06-11 | Cody | Frank VPS2 OpenClaw Update And Curated Tools

Date: 2026-06-11 Mountain Time
Agent: Cody
Status: Done
Server: VPS2 / Hostinger / 2.24.104.80
Validation agent: Frank

## Summary

Updated the VPS2 SOP in Notion and validated the update procedure on Frank as the required one-agent test.

The main operational fix is that VPS2 folder-based agents need two coordinated updates:

- OpenClaw core in `/opt/openclaw-{agent}`
- Curated OpenClaw tools in `/root/.openclaw-{agent}/npm`

This closes the previous VPS2 gap where core updates could happen while `@openclaw/codex` and `@openclaw/discord` stayed behind.

## Notion Pages Updated

- Agent-Creation-VPS2-SOP phase page: `OpenClaw Update Procedure-VPS2`
- Agent-Creation-VPS2-SOP phase page: `Curated VPS2 Tool Installer`
- Tech Updates journal: `2026-06-11 | Cody | VPS2 Update Procedure + Frank Tool Test`

## Frank Test Results

Before:

- OpenClaw core: `2026.5.28`
- `@openclaw/codex`: `2026.5.28`
- `@openclaw/discord`: `2026.5.28`
- Service: `openclaw-frank` active

After:

- OpenClaw core: `2026.6.5`
- `@openclaw/codex`: `2026.6.5`
- `@openclaw/discord`: `2026.6.5`
- Service: `openclaw-frank` active after restart
- Local HTTP check on port `4300`: `HTTP/1.1 200 OK`
- Gateway logs: `gateway ready`

## Doctor Result

The first SOP draft used the wrong doctor syntax for this OpenClaw build. `openclaw doctor --fix --config ...` is not supported in `2026.6.5`.

Working VPS2 pattern:

```bash
export HOME="/root/.openclaw-frank"
export OPENCLAW_STATE_DIR="/root/.openclaw-frank"
export OPENCLAW_CONFIG_PATH="/root/.openclaw-frank/openclaw.json"
cd /opt/openclaw-frank
/opt/openclaw-frank/node_modules/.bin/openclaw doctor --fix --yes --non-interactive
systemctl restart openclaw-frank
```

Doctor completed successfully and performed expected post-upgrade cleanup:

- SQLite state migrations
- retired model ref cleanup from `openai/gpt-5.2` to `openai/gpt-5.5`
- plugin peer-link repair for managed npm plugins
- auth profile SQLite import and OpenAI provider unification
- state directory permission tightening
- unusable Linux skill cleanup

## Non-Blocking Follow-Ups

These were left visible for future hardening, not treated as blockers for the update:

- broad network binding warning
- plaintext `gateway.auth.token` warning
- no command owner configured
- empty plugin allowlist warning

## Rollout Guidance

Frank passed the one-agent validation. Apply to additional VPS2 agents one at a time, using the SOP in Notion Phase 5.1 and the curated installer method in Phase 5.2.
