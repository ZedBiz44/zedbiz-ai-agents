# VPS2 OpenClaw 2026.6.8 Skills And Plugins Update

**2026-06-20 | Cody | Status: Done**

## Summary
- Updated VPS2 agents Frank, Harry, and Suzy to OpenClaw `2026.6.8`.
- Updated each per-agent npm tool folder to `@openclaw/codex@2026.6.8` and `@openclaw/discord@2026.6.8`.
- Updated the live VPS2 curated installer at `/root/curated-vps2-tool-installer.sh` so it accepts a target version and defaults to `2026.6.8`.
- Installed the shared binary layer required by the SOP-listed skills.
- Updated the related Notion SOP pages and Tech Updates journal.

## Final Verification
- Frank: service active, OpenClaw `2026.6.8`, local UI `HTTP 200` on port `4300`, 33 visible skills, plugin errors `0`.
- Harry: service active, OpenClaw `2026.6.8`, local UI `HTTP 200` on port `4100`, 34 visible skills, plugin errors `0`.
- Suzy: service active, OpenClaw `2026.6.8`, local UI `HTTP 200` on port `4200`, 33 visible skills, plugin errors `0`.

## Shared Tools Installed
- Ubuntu packages: `gh`, `ripgrep`.
- npm global tools: `clawhub`, `goplaces`, `@openai/codex`.
- Go-installed tools: `blogwatcher`, `camsnap`, `xurl`.
- Existing/verified tools: `jq`, `ffmpeg`.

## Plugin Result
Enabled discovered SOP plugins across the fleet:
- `perplexity`
- `tavily`
- `policy`
- `webhooks`
- `workboard`
- `llm-task`
- `codex-supervisor`
- `gradium`
- `oc-path`
- `open-prose`
- `parallel`
- `telegram`
- `thread-ownership`

Known inventory correction:
- `diagnostics-otel` was not present in the live VPS2 OpenClaw `2026.6.8` plugin inventory.
- Brave Search was not available as a discovered plugin/provider, so Suzy should not be assigned to Brave until the provider exists and passes a live test.

## Credential Follow-Ups
- Frank and Suzy are missing `channels.discord.token`; their Discord skill remains disabled.
- All three agents are missing the Google Places API key; `goplaces` remains disabled.
- Doctor warns about non-blocking hardening items: plaintext gateway token, LAN binding, no command owner, and OpenAI OAuth expiry timing.

## Notion Updates
- `Skills-Plugins-Setup-SOP` updated with live fleet results, credential gaps, and plugin inventory correction.
- `Curated-VPS2-Tool-Installer` updated with pinned-version installer behavior and shared binary layer.
- `OpenClaw-Update-Procedure-VPS2` updated with final rollout state and proceed-after-Frank guidance for assigned full-fleet rollouts.
