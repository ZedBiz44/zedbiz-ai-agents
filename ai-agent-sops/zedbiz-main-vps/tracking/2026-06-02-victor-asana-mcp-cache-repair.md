# 2026-06-02 - Victor Asana MCP npx Cache Corruption + Wilma WordPress MCP 404

## Summary
- **Date:** 2026-06-02 Mountain Time
- **Diagnosed By:** Manus + Cody
- **VPS:** VPS1 / main-vps `187.77.210.223`
- **Affected Agents:** Victor (resolved), Wilma (open)
- **Status:** Victor resolved / Wilma open

---

## Victor - Asana MCP Cache Corruption

### Symptom
OpenClaw Control dashboard showed:

```
`docker exec victor openclaw doctor --lint --non-interactive --no-workspace-suggestions (agent)` failed
```

Doctor output included:

```
[bundle-mcp] failed to start server "asana": McpError: MCP error -32000: Connection closed
"checkId":"core/doctor/runtime-tool-schemas","severity":"error"
```

### Root Cause (Disease)

**Not a Victor config problem. Not a token problem. A broken npx cache.**

The causal chain:

1. `@roychri/mcp-server-asana` **v1.8.0 was published 2026-03-29** and added `jsdom` as a brand new dependency (v1.7.0 did not have jsdom at all).
2. `jsdom v27.4.0` requires `html-encoding-sniffer v6.0.0`.
3. `html-encoding-sniffer v6.0.0` requires `@exodus/bytes/encoding-lite.js`.
4. The npx cache directory `25e9b1099bfd11f4` was created on **2026-06-02 at 01:13 AM** (container start) with a **partial/interrupted install** of `@exodus/bytes`. The directory existed but contained only 5 files instead of the full 40+. `encoding-lite.js` was missing.
5. On every subsequent run, npx saw the cache hash already existed and **skipped re-downloading**, so it kept using the broken partial install.

### Why It Self-Healed During Investigation

Running `npx -y @jordymeow/wordpress-mcp` (Wilma investigation) or the manual Asana test triggered a fresh npx resolution that detected the broken state and re-downloaded `@exodus/bytes` as v1.15.1 (latest), which includes `encoding-lite.js`.

### Verification
- `timeout 5 npx -y @roychri/mcp-server-asana` now outputs: `Starting Asana MCP Server... Connecting server to transport... Asana MCP Server running on stdio`
- `openclaw doctor` now returns **zero errors** (warnings only, same as baseline)

### Fix Applied
No manual action required -- the cache self-healed. If this recurs, the manual fix is:

```bash
docker exec victor sh -c 'rm -rf /home/node/.npm/_npx/<hash>'
docker restart victor
```

---

## Wilma - WordPress MCP Package Does Not Exist (Open)

### Symptom
Wilma's `openclaw.json` references:

```json
"wordpress-allzed": {
  "command": "/usr/local/bin/npx",
  "args": ["-y", "@jordymeow/wordpress-mcp"],
  "env": {
    "WP_BASE_URL": "https://allzed.com",
    "WP_AUTH_TYPE": "bearer",
    "WP_BEARER_TOKEN": "op://agent-wilma/aiengine-bearer-token/credential"
  }
}
```

### Root Cause (Disease)

`@jordymeow/wordpress-mcp` **does not exist on npm** -- confirmed 404:

```
npm error code E404
npm error 404 Not Found - GET https://registry.npmjs.org/@jordymeow%2fwordpress-mcp
```

No jordymeow-scoped packages exist on npm at all (`npm search jordymeow` returns zero results).

The Jordymeow / Meow Apps WordPress MCP is **not distributed as an npm package**. It is a `mcp.js` script bundled inside the **AI Engine WordPress plugin** itself, designed to be run locally from the plugin's `labs/` folder. It is not a standalone npx-installable server.

This means Wilma's MCP config was written with a package name that never existed. The config was likely written based on documentation or an assumption that the AI Engine MCP would be published to npm -- it was not.

### Status
**Open -- needs a replacement package or a different connection method.**

### Candidate Replacements

| Package | Notes |
|---|---|
| `wordpress-mcp` (v1.0.2) | Generic, minimal, ISC license, bearer token support unknown |
| `@automattic/mcp-wordpress-remote` | Official Automattic package, remote MCP support |
| `claudeus-wp-mcp` (v3.0.2) | 145 tools, comprehensive, active maintenance |
| AI Engine `mcp.js` via local file | Original intent -- requires the plugin installed on allzed.com and running the relay script |

### Recommended Next Step
Confirm whether AI Engine Pro is installed and active on `allzed.com`. If yes, the correct approach is to use the `mcp.js` relay script from the plugin's `labs/` folder rather than an npx package. If not, select a replacement npm package that supports bearer token auth against the WP REST API.

**Do not apply a fix until Jack confirms the preferred approach.**

---

## Open Warnings Across Victor and Wilma (Not Blocking, Future Hardening)

These were flagged by doctor on both agents and are not today's disease but are cleanup/hardening work:

- `AGENTS.md` exceeds bootstrap limits and will be truncated
- `gateway.auth.token` is stored as plaintext in `openclaw.json`
- Gateway is bound to `lan` / `0.0.0.0` (network-accessible)
- Discord DM policy is open (`dmPolicy="open"`)
- Discord guild `groupPolicy="open"` (mention-gated, not allowlisted)
- `commands.ownerAllowFrom` is not configured
- Stale plugin entries: `@openclaw/discord`, `active-memory`, `memory-wiki` (Victor)
