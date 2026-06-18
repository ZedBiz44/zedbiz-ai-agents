# VPS1 OpenClaw 2026.6.8 GHCR Access Preflight + SOP Update -- 2026-06-16
Date: 2026-06-16 | Author: Cody | Status: Completed

## Summary
Resolved a stale GHCR Docker credential on VPS1 that was blocking the OpenClaw 2026.6.8 image pull.
Updated three SOPs with new guardrails and sequencing rules before proceeding with the image build.

## Problem
- VPS1 had a saved `ghcr.io` Docker login that returned `denied` on every pull attempt.
- Initial diagnosis (Manus) flagged this as a paid subscription gate -- this was incorrect.
- Root cause: the stale credential was interfering with anonymous public image access.

## Fix
- Ran `docker logout ghcr.io` on VPS1 to remove the stale credential.
- Confirmed anonymous manifest inspect and pull both passed for `ghcr.io/openclaw/openclaw:2026.6.8`.
- Pulled image successfully: local image ID `9f55f0cb32b2`, created `2026-06-16T16:09:19Z` (10:09 MDT).

## Evidence
- Before logout: `LOGGED_IN_TO_GHCR=True` -- normal access returned `denied`.
- Anonymous manifest test passed immediately after logout.
- Pull succeeded without any authentication.

## SOPs Updated
- **OpenClaw Update Procedure VPS1**: Added required GHCR access preflight step and decision rule.
- **Docker Base Image Creation**: Added public-image pull rule and no-overlay guardrail.
- **Agent Creation VPS1 SOP**: Added parent maintenance note.

## Guardrail Added
Do not use npm runtime overlays, hardcoded runtime paths, or side-loaded OpenClaw runtimes as a
workaround for image access issues. Fix image access first, rebuild the ZedBiz base image properly,
then test one agent before rollout.

## SOP Sequencing Rule Added
Standard planned sequence: check/apply VPS/Docker/OS updates first, stop and smoke-test existing
agents, then proceed with OpenClaw image rebuild and one-agent test.

## Notion Reference
https://app.notion.com/p/381a3e33d581818e9da6efd9890abb4d
