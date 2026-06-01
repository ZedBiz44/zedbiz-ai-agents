# 2026-06-01 - Harry Codex Harness Error Fix

## Summary
- **Date:** 2026-06-01 MST
- **Changed By:** Manus
- **VPS:** VPS2 (2.24.104.80 / harry.zbiz.ca)
- **Affected Agent:** Harry
- **Status:** done

## Problem
Harry was throwing this error on every agent turn:

```
Agent failed before reply: Requested agent harness "codex" does not support openai/gpt-5.2
(provider is not one of: codex).
```

The model fallback chain was cycling through all configured models and failing because the
`codex` harness (auto-enabled by the presence of `openai/gpt-5.3-codex` in the model list)
only accepts models routed through the `openai-codex/*` or `openai/gpt-5.3-codex` provider.
When fallback reached `openai-codex/gpt-5.3-codex-spark` and `openai/gpt-5.2`, the harness
rejected them, exhausting all candidates and killing every request.

## Root Cause
Harry's `openclaw.json` was last touched by OpenClaw v2026.5.26 and contained:
- `openai-codex/gpt-5.3-codex-spark` in both `fallbacks` and `models` -- legacy prefix,
  not valid in 2026.5.28 catalog
- `openai/gpt-5.2` in both `fallbacks` and `models` -- retired model ref

`openclaw doctor --fix` partially ran but exited with a config validation error because it
tried to rewrite `openai-codex/gpt-5.3-codex-spark` to `openai/gpt-5.3-codex-spark` which
is itself not in the 2026.5.28 catalog. Doctor could not complete the migration automatically.

## What Was Fixed
Manually edited `/root/.openclaw-harry/openclaw.json`:
- Removed `openai-codex/gpt-5.3-codex-spark` from `fallbacks` and `models`
- Removed `openai/gpt-5.2` from `fallbacks` and `models`
- Final clean fallback chain: `gpt-5.5-pro, gpt-5.5, gpt-5.4, gpt-5.4-mini, gpt-5.3-codex`
- Updated `meta.lastTouchedVersion` to `2026.5.28`

## Commands Run
```bash
# Backup
cp /root/.openclaw-harry/openclaw.json /root/.openclaw-harry/openclaw.json.bak-2026-06-01

# Generate fixed config (Python inline, removed bad model refs)
cat /root/.openclaw-harry/openclaw.json | python3 -c "..." > /tmp/openclaw_fixed.json

# Apply
cp /tmp/openclaw_fixed.json /root/.openclaw-harry/openclaw.json

# Restart
systemctl restart openclaw-harry
```

## Verification
- Service: `active (running)` -- confirmed via systemctl
- Port 4100: listening (confirmed via ss -tlnp)
- HTTP 200: confirmed via curl localhost:4100
- Doctor: no Legacy model ref warnings, Errors: 0
- Logs: `[gateway] ready` -- no harness errors in post-restart logs

## Connection to VPS1 Fleet Issue
The same class of problem (legacy `openai-codex/*` refs + retired `gpt-5.2`) was fixed on
VPS1's 10 agents on 2026-05-31 via `openclaw doctor --fix` (see tracking doc
`2026-05-31-v2026528-upgrade-permissions-branding-systemd.md`). Harry was on VPS2 and
missed that batch run. This fix brings Harry into parity with the VPS1 fleet.

## Backup Location
`/root/.openclaw-harry/openclaw.json.bak-2026-06-01`

## Links
- Related VPS1 fix: `ai-agent-sops/zedbiz-main-vps/tracking/2026-05-31-v2026528-upgrade-permissions-branding-systemd.md`
