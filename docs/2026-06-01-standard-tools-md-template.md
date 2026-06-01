# 2026-06-01 - Standard TOOLS.md Template

## Summary

- **Date:** 2026-06-01 Mountain Time
- **Agent:** Manus
- **System:** Notion, OpenClaw, AI Agent Registry
- **Change Type:** documentation-updated, agent-template-created
- **Status:** done

## Why This Was Done

- Jack needed a standard `TOOLS.md` template for OpenClaw agents.
- The source material had useful safety guidance but needed to become a true tool map.
- A useful `TOOLS.md` must tell the agent what tools exist, what unlocks them, how to invoke them, what risk level they carry, and what fallback to use.

## What Changed

- Created shared Notion document `Standard-TOOLS-MD-Template`.
- Stored it in the `Agent-Documents` database as `Tools-MD`.
- Set the shared template version to `v2026.6.1-standard`.
- Built the template around:
  - Purpose
  - Environment summary
  - Capability gates
  - Tool selection priority
  - Read-only versus action tools
  - Available tool inventory block
  - Standard tool categories
  - Error handling and fallbacks
  - Credential and secret rules
  - Maintenance checklist

## Design Notes

- Removed citation artifacts from the source text.
- Made the template generic enough for all OpenClaw agents.
- Kept Skills as the home for reusable task methods.
- Kept `TOOLS.md` focused on actual capabilities, invocation details, access requirements, gotchas, and boundaries.
- Added placeholder blocks so each agent can document real tools instead of only high-level categories.

## Test Result

- **What was tested:** Notion page creation, database placement, page rendering, and Technical Documentation journal update.
- **Result:** Notion template was created and verified.
- **Evidence:** Created Notion page `Standard-TOOLS-MD-Template` and updated the Technical Documentation journal record.

## Links

- Notion template page: https://www.notion.so/372a3e33d581817e8ca4f7cd1e2541bf
- Agent-Documents database: https://www.notion.so/141ff5e3dfea492f8004afece936204e
- AI-Agent-Databases page: https://www.notion.so/371a3e33d58180b7b7b0fa140cbfea3c
