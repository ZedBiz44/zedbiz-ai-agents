# OpenClaw 2026.6.8 Base Image Build + Terry/Marsha Update -- 2026-06-16
Date: 2026-06-16 | Author: Cody | Status: Completed

## Summary
Built the new ZedBiz OpenClaw base image on VPS1 using OpenClaw 2026.6.8.
Rolled out to Terry and Marsha as the one-agent test pair before fleet-wide rollout.

## Image Changes (2026.6.5 -> 2026.6.8)
- OpenClaw updated from 2026.6.5 to 2026.6.8
- uv updated from 0.11.20 to 0.11.21
- just updated from 1.52.0 to 1.53.0
- Added chromium and chromium-driver (browser automation)
- Added rsync (VPS/workspace sync workflows)
- Added sqlite3 (local structured data and agent-side state workflows)
- Added global npm MCP packages:
  - @roychri/mcp-server-asana 1.8.0
  - @notionhq/notion-mcp-server 2.2.1

## Victor Image
- Built `zedbiz-openclaw-victor:2026.6.8-ssh`
- Verified: OpenClaw 2026.6.8, Docker CLI 29.5.3, Chromium, sqlite3, rsync

## Terry Update
- Recreated from `ghcr.io/zedbiz44/openclaw-base:latest` (now points to 2026.6.8 image)
- Container health: healthy
- https://terry.zbiz.ca returned HTTP 200
- Runtime tools verified: OpenClaw 2026.6.8, Chromium/ChromeDriver, sqlite3, uv, just, Asana MCP, Notion MCP

## Marsha Update + Cleanup
- Removed bad hardcoded `runtime-2026.6.6` npm overlay launch path from Marsha's docker-compose command
- Marsha now launches through the image-baked openclaw binary
- Discord/Codex plugin install calls kept on image-baked CLI (retains Discord/voice plugin loading)
- Recreated from `ghcr.io/zedbiz44/openclaw-base:latest`
- Container health: healthy
- https://marsha.zbiz.ca returned HTTP 200
- Gateway loaded: Discord, talk-voice, voice-call plugins
- Branding verified: favicon HTTP 200, page title = Marsha
- Compose command no longer references runtime-2026.6.6

## Notes
- Old `runtime-2026.6.6` overlay folder left in place for rollback/forensics pending Jack cleanup approval
- Rollout intentionally limited to Terry and Marsha per instruction
- Remaining fleet not yet recreated on new image (fleet rollout done 2026-06-17)

## Skill Plugin SOP Updates (2026-06-17)
- Updated `skill-plugin-setup-sop` with Edith's current doctor results after skill cleanup
- Added grouped setup list for remaining missing skill requirements
- Added Edith plugin inventory: 70 loaded/enabled, 28 disabled, 0 plugin errors
- Added operating rule: skill should be ON in both Skills page and Agent > Skills only when agent is expected to use it
- Reworked SOP into evergreen fleet SOP (removed agent-specific troubleshooting sections)
- Added web search setup for Brave, Perplexity, and Tavily with 1Password key names and SecretRef guidance
- Added concise Google Workspace/gog OAuth setup steps
- Added explicit plugin enablement steps for: Perplexity, Tavily, policy, webhooks, workboard, llm-task,
  codex-supervisor, diagnostics-otel, gradium, oc-path, open-prose, parallel, telegram, thread-ownership
- Ran fleet plugin comparison for watched plugins -- all 11 agents match

## Notion Reference
https://app.notion.com/p/381a3e33d581815dbffbf58094d8c83b
