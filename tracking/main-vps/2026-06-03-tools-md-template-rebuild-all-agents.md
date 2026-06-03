# Tracking Log: Tools.md Template Rebuild — All Agents

**Date:** 2026-06-03 (Mountain Time)
**Agent/Person:** Manus
**System Affected:** Agent-MD-Files Notion Database
**VPS Affected:** VPS1 (main), VPS2 (Harry, Suzy, Edith)
**Change Type:** SOP Update / Template Rebuild
**Status:** Completed

---

## Reason For Change

The existing Tools.md pages in the Agent-MD-Files Notion database were outdated and incomplete. They were built from an earlier, minimal template that lacked:

- Core Tool Doctrine section
- Capability Gate rule
- Local Runtime Environment details (VPS, port, plugins, skills count)
- Browser Automation section
- Startup Verification Rule
- Tool Action Filter (Goal/Need/Scope/Risk/Authority/Reversibility)
- Tool Selection Priority
- Strict Boundaries
- Read-Only vs Action Tools distinction
- External Action Rules (air-gap)
- Full Standard Tool Categories (Local Files, Skills, Notion, Asana, Web, Browser, Comms, Docs, CRM)
- Error Handling and Fallbacks

Jack confirmed the Standard-TOOLS-MD-Template (v2026.6.1-general-4) is the correct source template.

---

## Files / Pages Touched

All 13 agent Tools.md pages in the Agent-MD-Files Notion database were updated:

| Agent | Notion Page ID | Char Count | VPS |
|-------|---------------|------------|-----|
| Victor | 373a3e33d58181d7b6d4ddbe1132ce32 | 9005 | VPS1 |
| Amanda | 373a3e33d58181bfb390f711e6c992db | 8570 | VPS1 |
| Wilma | 373a3e33d581814f81f8c8c370a2e03a | 8623 | VPS1 |
| Inga | 373a3e33d581816dace3c9782b5b3076 | 8275 | VPS1 |
| Gohzed | 373a3e33d58181ee9253fc5fce6482bc | 8647 | VPS1 |
| Marsha | 373a3e33d58181b79b56f919365299f2 | 8573 | VPS1 |
| Grogar | 373a3e33d58181a8abedf83b89d3ecf8 | 8660 | VPS1 |
| Maggie | 373a3e33d58181c8a6b1c20270e5fb95 | 8578 | VPS1 |
| Vivian | 373a3e33d58181819067d8663976f3a5 | 8703 | VPS1 |
| Harry | 373a3e33d581814486acf524da4e96cd | 8055 | VPS2 |
| Terry | 373a3e33d58181eca8e1d4d8f684fcfa | 8651 | VPS1 |
| Suzy | 373a3e33d5818189a66ffbe039cf3baa | 8004 | VPS2 |
| Edith | 373a3e33d581811abaddfcc2de9fd337 | 8013 | VPS2 |

---

## What Changed Per Agent

Each agent's Tools.md was rebuilt from the Standard-TOOLS-MD-Template with their individual information filled in:

- **Purpose section:** Agent name, role, and specialty filled in.
- **Core Tool Doctrine:** Full doctrine copied from template (identical across all agents).
- **Local Runtime Environment:** VPS, workspace path, port, plugins list, skills count, and extra tools specific to each agent.
- **Browser Automation:** Standard section from template.
- **MCP Servers section:** Only the MCP servers each agent actually has configured:
  - VPS1 agents with Notion MCP: Victor, Amanda, Marsha, Maggie, Vivian, Terry
  - VPS1 agents with Asana MCP: Victor, Amanda, Wilma, Inga, Gohzed, Marsha, Grogar, Maggie, Vivian, Terry
  - Wilma: WordPress (AllZed) MCP added
  - Gohzed, Grogar: HighLevel/CRM section added
  - Victor: GitHub section added
  - Harry, Suzy, Edith (VPS2): No MCP servers configured — noted explicitly
- **Communication Channels:** Discord + Email for VPS1 agents; Discord only for VPS2 agents.
- **All doctrine sections:** Startup Verification, Tool Action Filter, Tool Selection Priority, Strict Boundaries, Read-Only vs Action, External Action Rules, Standard Tool Categories, Error Handling.
- **Database properties updated:** Character-Count, Current-Version (v2026.06.03), Status (Current), Source-Type (Template-Filled), Source-Location, Notes, Last-Reviewed (2026-06-03).

---

## Template Source

- **Template:** Standard-TOOLS-MD-Template
- **Template Version:** v2026.6.1-general-4
- **Template Notion ID:** 372a3e33d581817e8ca4f7cd1e2541bf
- **Template Char Count:** 9396

---

## Test Result

- Victor tested first: content replaced, properties updated, Character-Count verified at 9005 in Notion.
- All 13 agents completed with 0 failures.

---

## Rollback Note

Previous Tools.md content was minimal (100-300 chars each). The old content was replaced via `replace_content` command. To roll back, the old content would need to be restored from the previous Notion page history (Notion keeps version history automatically).

---

## Related Notion Page

- Agent-MD-Files Database: https://app.notion.com/p/21a22636ad0b44e390de01802a1eeafa
- Standard-TOOLS-MD-Template: https://app.notion.com/p/372a3e33d581817e8ca4f7cd1e2541bf

---

## What Is Still Open

- The `Document-Slug` field was left blank in the original records. Slugs have been added in this update (e.g., `victor-tools-md`).
- Harry, Suzy, and Edith have no MCP servers configured. If MCP servers are added to VPS2 agents in the future, their Tools.md files should be updated to include the relevant MCP sections.
- Skills counts for Suzy and Edith show 0 — these agents were recently installed and skills have not been configured yet.

---

## What Jack Should Review Next

- Open any agent's Tools.md in Notion and confirm the layout looks correct.
- If any agent has additional tools, MCP servers, or capabilities not captured here, update their Tools.md accordingly.
- Consider adding a `File-Path` property update to reflect the actual workspace path per agent.
