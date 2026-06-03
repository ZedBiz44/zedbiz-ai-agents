# Tracking Log: AGENTS.md Template Rebuild — All Agents

**Date:** 2026-06-03 (Mountain Time)
**Agent/Person:** Manus
**System Affected:** Agent-MD-Files Notion Database
**VPS Affected:** VPS1 (main), VPS2 (Harry, Suzy, Edith)
**Change Type:** SOP Update / Template Rebuild
**Status:** Completed

---

## Reason For Change

The existing AGENTS.md pages in the Agent-MD-Files Notion database were outdated and incomplete. They were built from an earlier minimal template that was missing:

- Agent Setup section with full VPS, port, URL, MCP servers, skills count, and channels
- Agent Title field
- Role Summary with decision authority notes
- Role-Specific Operating Rules (customized per agent)
- Decision Framework with agent-specific authority question
- All standard doctrine sections (Memory, Skills and Tools, Execution, Proactive Improvement, Security, Routing, Communication, Maintenance)
- Character-Count, Current-Version, Status, and Source-Location database properties

Jack confirmed the Standard-AGENTS-MD-Template (v2026.06.03) is the correct source template.

---

## Source Material Used Per Agent

Each agent's AGENTS.md was built using:
- Standard-AGENTS-MD-Template (Notion ID: 372a3e33d581814fb813cd0ff647579d)
- Agent Identity.md (fetched from Agent-MD-Files database)
- Agent Soul.md (fetched from Agent-MD-Files database where available)
- Agent Registry data (role, VPS, port, URL, channels, MCP servers, skills count)

---

## Files / Pages Touched

All 13 agent AGENTS.md pages in the Agent-MD-Files Notion database were updated:

| Agent | Notion Page ID | Char Count | VPS | Title |
|-------|---------------|------------|-----|-------|
| Victor | 373a3e33d581816cb0e4e7aec30ad0ca | 7828 | VPS1 | Tech Nerd |
| Amanda | 373a3e33d58181d6a82af06f85bf6de5 | 7624 | VPS1 | Marketing Director |
| Wilma | 373a3e33d5818169a341f41f4648a77f | 7709 | VPS1 | Website Manager |
| Inga | 373a3e33d58181f58fddc194312d6d87 | 7693 | VPS1 | Internet Marketing Manager |
| Gohzed | 373a3e33d5818167aa65e9b97294ef5e | 7677 | VPS1 | GHL Operations Specialist |
| Marsha | 373a3e33d58181729b5cd3a3ef685e4b | 7993 | VPS1 | Chief of Staff |
| Grogar | 373a3e33d581816b9a07cea4818ff4c6 | 7693 | VPS1 | Growth Garage Manager |
| Maggie | 373a3e33d58181999fb0edf0ffd4dbdc | 7710 | VPS1 | Brand Voice Specialist |
| Vivian | 373a3e33d581816db51ece0b0ba4ffe8 | 7767 | VPS1 | Video Specialist |
| Harry | 373a3e33d581813d9145e1285e7ddfd2 | 7744 | VPS2 | The Handyman |
| Terry | 373a3e33d581810785c7f05860ec6015 | 7642 | VPS1 | Test Agent |
| Suzy | 373a3e33d581811b8857d6d941e3080a | 7699 | VPS2 | Side-Hustle Queen |
| Edith | 373a3e33d5818111bd7fe0e400ac6b03 | 7968 | VPS2 | Memory Maid |

---

## What Changed Per Agent

Each agent's AGENTS.md was rebuilt with:

**Shared across all agents (from template):**
- Purpose section
- Priorities (Correctness, Evidence, Safety, Minimal change, Consistency, Performance)
- Startup Rules
- Decision Framework (with agent name filled in)
- Memory Rules
- Skills And Tools
- Execution And Completion Rules
- Proactive Improvement Loop
- Security Rules
- Communication Rules (including emoji reaction rule)
- Maintenance Rules

**Customized per agent:**
- Agent Setup: name, role, title, reports-to, workspace path, VPS, port, URL, channels, MCP servers, skills count, source of truth, approval required
- Role Summary: role description + decision authority notes
- Role-Specific Operating Rules: 7-8 rules unique to each agent's function
- Routing Rules: agent-specific source-of-truth routing

**Key customizations by agent:**
- Victor: Docker/VPS safety rules, alpine workaround rule, GitHub as source of truth
- Amanda: Brand voice consistency, Asana task routing, external send approval
- Wilma: WordPress AllZed MCP rules, site change confirmation, plugin approval
- Inga: Digital product validation, funnel testing, binary tools (ffmpeg, ntn)
- Gohzed: CRM pipeline confirmation, no-send-without-approval, HighLevel API preference
- Marsha: Highest operational authority, Asana currency, agent handoff rules
- Grogar: GHL Growth Garage content, HighLevel read-only reference
- Maggie: NLP/persuasion frameworks, brand guide baseline, copy review before external
- Vivian: openai-whisper-api transcription, media/ folder organization, video tracker
- Harry: VPS2 bare-metal rules, himalaya email, op run secrets, no Docker
- Terry: Test-one-at-a-time rule, no destructive tests on production, test log currency
- Suzy: Commercial usefulness first, fast test validation, VPS2 bare-metal, no Docker
- Edith: Organize before advising, fact/inference/gap separation, discretion on personnel context

**Database properties updated for all 13:**
- Character-Count (accurate per agent)
- Current-Version: v2026.06.03
- Status: Current
- Source-Type: Template-Filled
- Source-Location: Standard-AGENTS-MD-Template v2026.06.03 + Agent Registry + Soul + Identity
- Notes: rebuild summary with char count
- Last-Reviewed: 2026-06-03

---

## Template Source

- **Template:** Standard-AGENTS-MD-Template
- **Template Notion ID:** 372a3e33d581814fb813cd0ff647579d
- **Template Char Count:** 6572

---

## Test Result

- Victor tested first: content replaced, properties updated, Character-Count verified at 7828.
- All 13 agents completed with 0 failures.

---

## What Is Still Open

- Amanda and Gohzed do not have Soul.md files in the database. Their AGENTS.md was built from Identity.md and Registry data only. Soul.md files should be created for them when time allows.
- Harry, Suzy, and Edith (VPS2) have 0 skills installed. When skills are added, update the Skills Installed field in their AGENTS.md.
- If any agent's role, MCP servers, or authority changes, update their AGENTS.md Role-Specific Operating Rules accordingly.

---

## Related Notion Pages

- Agent-MD-Files Database: https://app.notion.com/p/21a22636ad0b44e390de01802a1eeafa
- Standard-AGENTS-MD-Template: https://app.notion.com/p/372a3e33d581814fb813cd0ff647579d
