# 2026-06-01 -- Suzy Memory Setup

**Date:** 2026-06-01 MST
**Agent:** Suzy
**VPS:** VPS2 (2.24.104.80)
**Performed by:** Manus AI

## Problem

Suzy was installed in Phase 1 without memory plugins configured. The bash tool error
`ls -la MEMORY.md memory` (exit code 2) was Suzy exploring her workspace on first boot
and finding no memory structure.

## What Was Set Up

### Plugins Added to openclaw.json

- `memory-core` -- dreaming system (nightly 2am Edmonton, light/deep/rem phases)
- `memory-wiki` -- knowledge vault at `/root/.openclaw-suzy/workspace/wiki`
- `active-memory` -- balanced prompt style, recent query mode, bridge enabled

### Wiki Vault Structure

```
/root/.openclaw-suzy/workspace/wiki/
  concepts/
  entities/
    index.md        <- auto-populates as Suzy has conversations
  reports/
  sources/
  syntheses/
  _views/
  _attachments/
  index.md
/root/.openclaw-suzy/workspace/memory/
  .dreams/
    session-corpus/
```

### Verification Results

```
memory status  -> Dreaming: 0 2 * * * (America/Edmonton)  ✅
wiki status    -> Vault: ready (...)                       ✅
entities dir   -> index.md present                        ✅
```

## Config Issue Fixed

The initial `memory-wiki` vault config was written as a plain string:
```json
"vault": "/root/.openclaw-suzy/workspace/wiki"
```

This caused the error: `vault: must be object`. Corrected to:
```json
"vault": {"path": "/root/.openclaw-suzy/workspace/wiki"}
```

## SOP Impact

Step 12b added to all four Phase 1.1 SOP pages (VPS1 and VPS2, agent and human versions)
to ensure memory is always set up before `openclaw doctor` is run.
