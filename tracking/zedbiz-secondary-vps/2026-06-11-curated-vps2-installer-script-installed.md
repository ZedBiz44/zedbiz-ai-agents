# 2026-06-11 | Cody | Curated VPS2 Installer Script Installed

Date: 2026-06-11 Mountain Time
Agent: Cody
Status: Done
Server: VPS2 / Hostinger / 2.24.104.80

## Summary

Placed the reusable curated VPS2 tool installer script on VPS2 after validating the update flow on Frank.

## Server Placement

- Script path: `/root/curated-vps2-tool-installer.sh`
- Owner/group: `root:root`
- Permissions: `700`
- Syntax check: passed with `bash -n`

## Purpose

The script implements the Phase 5.2 curated tool installer method for VPS2 folder-based OpenClaw agents.

Current curated tools:

- `@openclaw/codex@latest`
- `@openclaw/discord@latest`

## Usage

```bash
/root/curated-vps2-tool-installer.sh frank
```

Use Frank first for validation, then apply one VPS2 agent at a time after confirmation.
