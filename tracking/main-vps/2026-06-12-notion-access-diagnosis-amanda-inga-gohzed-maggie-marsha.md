# 2026-06-12 - Notion Access Diagnosis For Amanda, Inga, Gohzed, Maggie, And Marsha

Date | Author | Status: 2026-06-12 | Cody | Amanda Fixed And Tested

## Summary

- Investigated why Amanda, Inga, Gohzed, Maggie, and Marsha cannot access Notion.
- Found all five containers are running and healthy.
- Found all five have the `openai-codex:jzedbiz@gmail.com` OAuth profile present in OpenClaw config.
- Found all five have only the `asana` MCP server configured.
- Found no `notion` MCP server path in the five named agents' OpenClaw config.
- Compared against Terry, which does have both `asana` and `notion` configured at the MCP layer.
- Found Terry's active Notion MCP block was pointing at an invalid stale Notion token.
- Found a valid `NOTION_API_KEY` already present in each agent's runtime environment.
- Fixed Amanda by adding a Notion MCP server that references `NOTION_API_KEY` instead of embedding the stale token.
- Amanda passed the read-only Notion test through the actual agent runtime.

## Live Agents Checked

- `amanda`: healthy, MCP servers: `asana`
- `inga`: healthy, MCP servers: `asana`
- `gohzed`: healthy, MCP servers: `asana`
- `maggie`: healthy, MCP servers: `asana`
- `marsha`: healthy, MCP servers: `asana`

## Diagnosis

- This was not a simple expired OpenAI Codex OAuth problem.
- The OpenAI Codex OAuth profile exists in the five named agents.
- The Notion API key also exists and validates successfully in runtime.
- The missing piece was that Notion was not exposed to these agents as a configured MCP/tool server.
- In practical terms: the agents had the Notion credential, but most did not have the Notion tool doorway wired into their runtime.

## Comparison

- Terry is the known-good reference checked during this diagnosis.
- Terry's OpenClaw MCP layer includes both `asana` and `notion`.
- The five affected agents only include `asana`.
- Terry's existing Notion MCP block used a stale embedded token and returned `401 unauthorized`.
- The valid pattern is to use the runtime `NOTION_API_KEY`, which returned `200 OK`.

## Amanda Fix

- Backed up Amanda's OpenClaw config.
- Added a `notion` MCP server to Amanda using:
  - `/usr/local/bin/npx`
  - `@notionhq/notion-mcp-server`
  - `OPENAPI_MCP_HEADERS` built from `NOTION_API_KEY`
- Reloaded Amanda's MCP runtime.
- Verified Amanda's direct Notion API search returned `200 OK`.
- Verified `openclaw mcp probe notion` returned 22 tools.
- Ran a read-only Amanda agent-runtime test.
- Amanda replied: `NOTION_OK Technical Documentation`.
- Runtime trace showed one Notion tool call and zero failures.
- Amanda stayed healthy.

## Rest Of Fleet Check

- `inga`: valid `NOTION_API_KEY`, no Notion MCP server yet.
- `gohzed`: valid `NOTION_API_KEY`, no Notion MCP server yet.
- `maggie`: valid `NOTION_API_KEY`, no Notion MCP server yet.
- `marsha`: valid `NOTION_API_KEY`, no Notion MCP server yet.
- `terry`: has Notion MCP server, but the active embedded Notion token is stale; runtime `NOTION_API_KEY` is valid.
- `victor`: valid `NOTION_API_KEY`, no Notion MCP server yet.
- `vivian`: valid `NOTION_API_KEY`, no Notion MCP server yet.
- `wilma`: valid `NOTION_API_KEY`, no Notion MCP server yet.
- `edith`: valid `NOTION_API_KEY`, no MCP servers currently configured.

## Recommended Next Step

- Roll Amanda's environment-variable-based Notion MCP config to the remaining agents that should have Notion access.
- Retest one read-only Notion search per agent after rollout.
- Avoid copying Terry's stale embedded token pattern.
