# Memory, Wiki, and People Wiki Audit -- All 10 Agents
**Date:** 2026-05-28 MST
**Session:** Manus
**Task:** Audit all 10 agents for memory dreaming system, memory wiki, and people wiki setup

---

## Audit Results (Pre-Fix)

| Agent | Dreaming | Wiki Vault | Wiki Vault Path | Entities Folder |
|---|---|---|---|---|
| inga | OK (0 2 * * * Edmonton) | ready | /opt/openclaw/shared/knowledge/inga/wiki | present |
| amanda | OK | ready | /opt/openclaw/shared/knowledge/amanda/wiki | present |
| victor | FAILED (showed "off") | FAILED (wrong path) | N/A | present |
| wilma | OK | ready | /opt/openclaw/shared/knowledge/wilma/wiki | present |
| marsha | OK | ready | /opt/openclaw/shared/knowledge/marsha/wiki | present |
| gohzed | OK | EMPTY vault | vault path NOT SET | missing |
| grogar | OK | EMPTY vault | vault path NOT SET | missing |
| maggie | OK | EMPTY vault | vault path NOT SET | missing |
| vivian | OK | EMPTY vault | vault path NOT SET | missing |
| terry | OK | EMPTY vault | vault path NOT SET | missing |

---

## Root Causes Found

### gohzed, grogar, maggie, vivian, terry -- Empty Wiki Vaults
- Wiki vault directories existed at `/opt/openclaw/shared/knowledge/{agent}/wiki/` but were completely empty
- `memory-wiki` plugin config had `vault.path` NOT SET -- plugin was using internal default
- Result: wiki plugin was enabled but had no vault to write to

### victor -- False "Dreaming: off" and Wrong Wiki Path
- Victor's `docker-compose.yml` has `user: root` (unique among all agents)
- All other `docker exec` commands run as root by default
- The gateway process runs as `node` user (via `su node`) and reads `/home/node/.openclaw/openclaw.json`
- But `docker exec victor openclaw ...` runs as root and reads `/root/.openclaw/openclaw.json` (doesn't exist)
- So all CLI status checks on Victor were reading a non-existent config and showing defaults
- **Victor's actual config is correct** -- confirmed by running `docker exec -u node victor openclaw ...`

---

## Fixes Applied

### gohzed, grogar, maggie, vivian, terry
1. Created full wiki vault directory structure using docker alpine workaround (root-safe writes):
   - `_views/`, `_attachments/`, `concepts/`, `entities/`, `reports/`, `sources/`, `syntheses/`
   - Copied `WIKI.md`, `AGENTS.md`, `inbox.md`, `index.md` from Inga as template
2. Fixed file ownership: `chown -R 1000:1000` (node user UID) via docker alpine workaround
3. Set `vault.path` in each agent's `openclaw.json` to `/opt/openclaw/shared/knowledge/{agent}/wiki`
4. Restarted all 5 agents

### victor
- No config fix needed -- config was already correct
- Documented the `user: root` quirk: always use `docker exec -u node victor` for CLI commands
- Wiki vault was already set up and ready

---

## Final Audit Results (Post-Fix)

| Agent | Dreaming | Wiki Vault | Entities Folder |
|---|---|---|---|
| inga | OK (0 2 * * * Edmonton) | ready | present |
| amanda | OK | ready | present |
| victor | OK (verified with -u node) | ready | present |
| wilma | OK | ready | present |
| marsha | OK | ready | present |
| gohzed | OK | ready (bridge mode) | present |
| grogar | OK | ready (bridge mode) | present |
| maggie | OK | ready (bridge mode) | present |
| vivian | OK | ready (bridge mode) | present |
| terry | OK | ready (bridge mode, 30 artifacts) | present |

**All 10 agents: dreaming scheduled, wiki vault ready, entities folder present.**

---

## Notes

### "People Wiki" Clarification
In OpenClaw v2026.5.26, there is no separate "people wiki" plugin. The people/contacts/relationships system is:
- The `entities/` subfolder inside the memory-wiki vault (auto-populated as agents interact)
- The `## Important People` section in `MEMORY.md` (template placeholder for agents using the standard template)
- Victor, Marsha, and Terry have custom `MEMORY.md` files without the template placeholder -- this is intentional, their entity tracking happens through the wiki vault entities folder

### Victor's docker-compose Quirk
Victor is the only agent with `user: root` in docker-compose.yml. This means:
- `docker exec victor openclaw ...` runs as root -- reads `/root/.openclaw/` (doesn't exist)
- Always use `docker exec -u node victor openclaw ...` for accurate CLI checks on Victor
- The gateway itself runs correctly as node user

### Wiki Vault Modes
- Agents with prior conversation history (inga, amanda, victor, wilma, marsha): `isolated` mode
- Newly initialized agents (gohzed, grogar, maggie, vivian, terry): `bridge` mode (auto-imports memory artifacts)
- Bridge mode is actually better for new agents -- it will auto-populate the wiki from memory as conversations happen

---

## Files Changed
- `/opt/openclaw/agents/gohzed/config/openclaw.json` -- added vault.path
- `/opt/openclaw/agents/grogar/config/openclaw.json` -- added vault.path
- `/opt/openclaw/agents/maggie/config/openclaw.json` -- added vault.path
- `/opt/openclaw/agents/vivian/config/openclaw.json` -- added vault.path
- `/opt/openclaw/agents/terry/config/openclaw.json` -- added vault.path
- `/opt/openclaw/shared/knowledge/gohzed/wiki/` -- initialized full structure
- `/opt/openclaw/shared/knowledge/grogar/wiki/` -- initialized full structure
- `/opt/openclaw/shared/knowledge/maggie/wiki/` -- initialized full structure
- `/opt/openclaw/shared/knowledge/vivian/wiki/` -- initialized full structure
- `/opt/openclaw/shared/knowledge/terry/wiki/` -- initialized full structure
