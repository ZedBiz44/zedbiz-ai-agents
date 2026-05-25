# SOP v8 — Bootstrap URL and Token Fix
**Date:** 2026-05-25
**Agent:** Manus
**Scope:** Both SOPs updated (ai-agent-base-build-sop-phase11.md, human-agent-base-build-sop-phase11.md)

## What Was Wrong

After fixing the identity path in v7, Harry was still failing the first-run bootstrap process. The gateway logs showed this error:

```
gateway failed: invalid gatewayUrl protocol: https: (expected ws:// or wss://)
raw_params={"gatewayUrl":"https://harry.zbiz.ca","gatewayToken":"YOUR_GATEWAY_TOKEN"...}
```

**Root causes:**

1. **Wrong Protocol:** The SOP told the agent to use `https://harry.zbiz.ca` for the gateway URL. The OpenClaw gateway tool requires a WebSocket protocol (`ws://` or `wss://`).
2. **Literal Placeholder:** The SOP contained the literal string `YOUR_GATEWAY_TOKEN` in the instructions, and the agent used it verbatim instead of substituting the actual token.

## The Fix

The SOP must provide the correct WebSocket URL and instruct the agent on how to find the real token.

### Fix 1: Update the Gateway URL in IDENTITY.md

Changed the URL in the `IDENTITY.md` seed block from:
`- URL: https://harry.zbiz.ca`
To:
`- URL: https://harry.zbiz.ca`
`- Gateway URL: wss://harry.zbiz.ca`

### Fix 2: Add a Bootstrap Rule to AGENTS.md

Added a specific rule to `AGENTS.md` explaining how to handle the bootstrap check:

```markdown
## Bootstrap Rule
If asked to check bootstrap status, use the `gateway` tool.
- `gatewayUrl` must be `wss://harry.zbiz.ca`
- `gatewayToken` must be the actual token. If you don't know it, ask the user for it. Do NOT use placeholder text like "YOUR_GATEWAY_TOKEN".
```

## Next Step

Wipe Harry on VPS-2 and rebuild from scratch using the corrected v8 SOP.
