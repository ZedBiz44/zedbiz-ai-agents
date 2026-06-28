# 2026-06-28 | Cody | Small-Bite Skill Activation For Z-Knowledge Agents

Date: 2026-06-28 | Author: Cody | Status: Complete

## Summary

Activated `small-bite-wiki-research` as a real workspace skill for the Z-Knowledge rollout and changed the Z-Knowledge skill instructions so broad first assignments default to a small first pass instead of an all-at-once wiki, Notion, and Z-Connections pipeline.

## Root Cause

`small-bite-wiki-research` existed under `plugin-skills`, including at `/opt/openclaw/agents/marsha/plugin-skills/small-bite-wiki-research`, but OpenClaw did not expose that location as an active ready skill for the affected agents. The active VPS1 runtime skill path is `/opt/openclaw/agents/{agent}/skills`.

The skill's older instructions also treated broad assignments and "get it done" language as permission to auto-run all stages. That could overwhelm first assignments and contribute to long-running or hung work.

## Changes Made

- Installed `small-bite-wiki-research` into the active VPS1 agent skill path.
- Updated `small-bite-wiki-research` so broad research topics default to one useful first bite.
- Updated `zedbiz-knowledge-routing` to route broad first assignments as small bites.
- Updated `zedbiz-wiki-research` to create the smallest useful wiki update first.
- Updated `zedbiz-notion-knowledge-publishing` to create or update the one best Notion record first and avoid speculative Z-Connections.
- Created and pushed the source repo for `small-bite-wiki-research-skill`.

## Verification

VPS1 Terry was tested first. `openclaw skills list` showed all four skills ready:

- `small-bite-wiki-research`
- `zedbiz-knowledge-routing`
- `zedbiz-notion-knowledge-publishing`
- `zedbiz-wiki-research`

After rollout, these running VPS1 agents each showed all four skills active:

- Amanda
- Edith
- Grogar
- Inga
- Maggie
- Marsha
- Terry
- Victor
- Vivian
- Wilma

VPS1 notes:

- Gohzed was installed on disk but was not present in the running-container verification output.
- Zara was skipped because `/opt/openclaw/agents/zara/skills` does not exist.

VPS2 file rollout completed for:

- Harry: `/root/.openclaw-harry/workspace/skills`
- Frank: `/root/.openclaw-frank/workspace/skills`
- Suzy: `/root/.openclaw-suzy/workspace/skills`

VPS2 runtime `skills list` verification was blocked because the shell lacks the live 1Password session context used by the running OpenClaw launch command.

## GitHub / Source Control

Pushed skill repo commits:

- `ZedBiz44/zedbiz-knowledge-routing-skill`: `0838f6dcaaa2ac10ab8466b944a3a3c7dba7e4ea`
- `ZedBiz44/zedbiz-wiki-research-skill`: `08e4c0d58dfc037392eb6c9bc9aa5a5a1022925f`
- `ZedBiz44/zedbiz-notion-knowledge-publishing-skill`: `fd40139e49a55a64e69ccae02c8fcb7247749f72`
- `ZedBiz44/small-bite-wiki-research-skill`: `c60850bbef60f66b709490060b08bbc4391634f7`

Local small-bite source path:

- `D:\Google Drive\Documents\Codex-Projects\ZedBiz-Skill-Repos\small-bite-wiki-research-skill`

## Current Operating Guidance

For first Z-Knowledge assignments, agents should start with one useful small bite: scope, duplicate check, light source review, and one best saved artifact when justified. They should only continue into a full staged wiki, Notion, and Z-Connections buildout when Jack explicitly asks for that continuation.
## Follow-Up Verification - VPS2 Runtime Skill Path

After a direct re-check, VPS2 required the four research skills in the runtime-discovered path:

- Harry: `/root/.openclaw-harry/.openclaw/skills`
- Frank: `/root/.openclaw-frank/.openclaw/skills`
- Suzy: `/root/.openclaw-suzy/.openclaw/skills`

The earlier `workspace/skills` copies were present, but OpenClaw's live `skills list` did not discover them from that location.

Final VPS2 verification now passes. With `HOME` set to each agent's runtime home and the matching `/opt/openclaw-{agent}` OpenClaw binary, `openclaw skills list --eligible` shows all four skills as ready for Harry, Frank, and Suzy:

- `small-bite-wiki-research`
- `zedbiz-knowledge-routing`
- `zedbiz-notion-knowledge-publishing`
- `zedbiz-wiki-research`