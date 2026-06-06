# 2026-06-05 - Victor Job Description Operating Manual

## Summary

- **Date:** 2026-06-05 Mountain Time
- **Changed By:** Codex
- **VPS:** VPS1 / main-vps
- **Affected Agent:** Victor
- **Status:** documented in Notion and tracked in GitHub

## Business Reason

Victor is the VPS1 server and OpenClaw fleet caretaker. After the recent Docker-admin upgrade, orphan core gateway cleanup, and healthcheck diagnosis, Victor needed a clear operating role: duties, schedules, SOPs, tools, guardrails, and server structure in one plain-English place.

## Work Completed

- Created the Notion page structure for `Victor Job Description`.
- Added the inline database `Victor VPS1 Operating Manual`.
- Added operating pages for:
  - Victor core duties
  - Daily VPS1 healthcheck SOP
  - Daily and weekly schedule
  - Incident response SOP
  - Healthcheck guardrails
  - Victor tool inventory
  - VPS1 server map
  - Docker fleet checks
  - Caddy and public URL checks
  - Secrets and access boundaries
  - GitHub and Notion reporting rules
  - Escalation and DSCA rules
  - LLM cost control and automation
- Added the main overview content to the Victor Job Description page.
- Added a daily journal record in Notion Technical Documentation / Tech Updates.

## Operating Decisions Captured

- Victor is the designated server caretaker for VPS1.
- Victor has Docker-level admin capability by design; this should not be copied to every agent.
- Routine health checks should be scripted where possible to avoid unnecessary LLM model-credit usage.
- Agents inside Docker containers should not treat missing host-level tools such as `systemctl`, `ss`, `netstat`, `ufw`, or `nft` as automatic server failures.
- Host-level checks require either approved host SSH access or a designed host-check bridge.
- GitHub remains the technical source of truth; Notion remains the operational layer.
- Diagnose Mode uses DSCA: diagnose, solution, confirmation, act.
- Fleet changes should be tested on one agent before being applied to the rest.

## Notion Links

- Victor Job Description: https://app.notion.com/p/376a3e33d5818044ba0cff89141f372f
- Victor VPS1 Operating Manual: https://app.notion.com/p/ccd1eb29aaf44f2ab27447c04c2c837a
- Daily journal entry: https://app.notion.com/p/377a3e33d58181fabaddfc46a58a0965

## Source Notes

- This documentation is based on the verified Victor Docker-admin upgrade from `2026-06-01-victor-node-docker-admin-fix.md`.
- This documentation is also based on the verified orphan core gateway removal and healthcheck diagnosis from `2026-06-02-orphan-core-gateway-removal-and-healthcheck-diagnosis.md`.

## Rollback Note

This change is documentation-only. If the Notion structure needs to be revised, update the Notion page and add a follow-up tracking note rather than deleting this historical record.
