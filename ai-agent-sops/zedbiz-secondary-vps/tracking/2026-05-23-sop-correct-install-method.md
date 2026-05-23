# SOP Update: Corrected Install Method — Per-Agent npm + systemd

**Date:** 2026-05-23
**Agent:** Manus

## Problem Identified

The previous `openclaw gateway install` command created a managed profile-based setup using the global npm install at `/usr/lib/node_modules/openclaw`. This caused a split-state layout:

- `/root/.openclaw-harry` — Harry config/state
- `/root/.openclaw/workspace-harry` — Harry workspace (wrong location)

This is the same cross-contamination problem that existed on VPS-1.

## Correct Method

Each agent gets its own isolated npm install and systemd service:

```
/opt/openclaw-[agent-name]/          <- Agent's own OpenClaw program
/root/.openclaw-[agent-name]/        <- ALL agent data (workspace, memory, skills, etc.)
```

The systemd service MUST include:
```
Environment=OPENCLAW_HOME=/root/.openclaw-[agent-name]
```

This is the single most critical line. Without it, OpenClaw falls back to the global ~/.openclaw directory.

## SOPs Updated

- ai-agent-base-build-sop: https://www.notion.so/3e4a3e33d5818352bdb701eed29da450
- human-agent-base-build-sop: https://www.notion.so/04ea3e33d5818318b4ae812c80a98da3

## Key Changes in Both SOPs

- Removed: `openclaw gateway install` command
- Added: `npm install openclaw` inside `/opt/openclaw-[agent-name]/`
- Added: `ExecStart=/opt/openclaw-[agent-name]/node_modules/.bin/openclaw gateway run`
- Added: `Environment=OPENCLAW_HOME=/root/.openclaw-[agent-name]` in systemd
- Added: Step 7 verification that workspace is in correct location
- Added: Assertion that workspace is NOT in /root/.openclaw/workspace-[agent-name]

## Action Items

- Harry and Edith on VPS-2 still use the old layout — rebuild required
- Both agents have no real data yet — clean rebuild is low risk
