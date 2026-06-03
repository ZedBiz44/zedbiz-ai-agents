# SOP Audit and Corrections -- June 3, 2026

## Metadata
- **Date:** 2026-06-03 MST
- **Changed By:** Manus
- **Scope:** Agent-Creation-VPS1-SOP and Agent-Creation-VPS2-SOP full audit
- **Status:** Complete

## Context
Victor's doctor command failure was investigated and resolved (Asana MCP npx cache corruption -- self-healed). During the investigation, a broader audit of both SOP databases was requested to find and fix documentation drift accumulated over the last week of changes.

## Audit Scope
- Last week of GitHub tracking logs reviewed (Manus + Cody)
- All 16 VPS1 SOP sub-pages read in full
- All 9 VPS2 SOP sub-pages read in full
- Cross-referenced against: 2026-05-31 fleet upgrade note, 2026-06-02 VPS2 corrections note, live VPS state

## Confirmed Errors Fixed (13 Total)

### VPS1 Fixes (6)
1. **All /core references changed to /workspace** -- VPS1 Phase 1.1 (Human), Phase 1.2 (AI), Phase 4.2 (WordPress MCP). All volume mount paths, chown commands, wiki vault paths, and file verification commands updated.
2. **OPENCLAW_IMAGE version updated** -- Changed from `2026.5.6`/`2026.5.26` to `ghcr.io/openclaw/openclaw:2026.5.28` in Phase 1.1 templates.
3. **Branding path in post-start.sh updated** -- Changed from `/home/node/.openclaw/branding/` to `/app/dist/control-ui/branding/` in Phase 1.1 templates.
4. **WordPress MCP mcp.servers entry corrected** -- Phase 4.2 updated to document that the `@jordymeow/wordpress-mcp` npx entry is wrong and must be replaced with a direct streamable-http connection.
5. **Phase 4.2 status corrected** -- Changed from "Complete -- May 13, 2026" to "Broken (Needs Repair) -- June 2, 2026" based on Cody's June 2 verification.
6. **Uncertain Items list appended** -- 7 flagged items added to the bottom of the VPS1 main page for future review.

### VPS2 Fixes (7)
7. **Deleted 2 duplicate Phase 1.1-agent pages** -- ai-agent-base-build-sop (1) and (2) removed. Current page (3e4a3e33) is the source of truth.
8. **Deleted 2 duplicate Victor test pages** -- Old non-emoji versions (c2aa, 856a) removed. Emoji versions (99da, c54a) retained.
9. **WordPress AI Engine MCP page assigned Phase 2.5** -- Previously had no phase assigned.
10. **Phase numbering conflicts resolved** -- memory-stack-sop moved from Phase 2.1 to Phase 2.2; Memory/Knowledge Setup moved from Phase 2.1a to Phase 2.2a.
11. **VPS2 Phase 1.1 Human SOP model updated** -- Changed `openai/gpt-4o` default to `openrouter/google/gemini-3.1-flash-preview` to match the current standard.
12. **Uncertain Items list appended** -- 6 flagged items added to the bottom of the VPS2 main page for future review.
13. **Stale duplicate SOP deleted from GitHub root** -- `ai-agent-base-build-sop.md` at repo root removed. `ai-agent-sops/zedbiz-secondary-vps/ai-agent-base-build-sop-phase11.md` is the canonical source.

## Uncertain Items Flagged (13 Total)
See the bottom of each main SOP page for the full flagged lists. Key items:
- VPS1: Phase 2.1/2.1b memory paths, Phase 2.2 skill paths, Discord stale plugins, Caddy path, AGENTS.md size warning
- VPS2: Phase 1.2 1Password vault naming, Phase 1.3 Caddy path, OAuth copy-from-existing-agent option, Phase 3 page staleness, Victor tests relevance, update_models_vps2.py script existence

## Files Changed
- Notion: VPS1 Phase 1.1 Human SOP (c4fa3e33) -- /core -> /workspace, image version, branding path
- Notion: VPS1 Phase 1.2 AI SOP (c3fa3e33) -- /core -> /workspace, image version, branding path
- Notion: VPS1 Phase 4.2 WordPress MCP (4e0a3e33) -- /core -> /workspace, status corrected, mcp.servers note added
- Notion: VPS1 Main Page (f24a3e33) -- Uncertain Items list appended
- Notion: VPS2 Phase 1.1 Human SOP (04ea3e33) -- gpt-4o -> gemini-3.1-flash-preview
- Notion: VPS2 WordPress MCP (7cba3e33) -- Phase 2.5 assigned
- Notion: VPS2 memory-stack-sop (e22a3e33) -- Phase 2.1 -> Phase 2.2
- Notion: VPS2 Memory/Knowledge Setup (d07a3e33) -- Phase 2.1a -> Phase 2.2a
- Notion: VPS2 Main Page (ee1a3e33) -- Uncertain Items list appended
- Notion: Deleted ai-agent-base-build-sop (1) (373a-8026), ai-agent-base-build-sop (2) (373a-8061)
- Notion: Deleted Victor Daily Tests (c2aa), Victor Weekly Tests (856a)
- GitHub: Deleted ai-agent-base-build-sop.md from repo root
