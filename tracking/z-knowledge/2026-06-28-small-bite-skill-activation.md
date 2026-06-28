# 2026-06-28 | Cody | Small-Bite Skill Activation For Z-Knowledge Agents

Date: 2026-06-28 | Author: Cody | Status: Complete with one GitHub repo creation blocker

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
- Created a local source repo for `small-bite-wiki-research-skill`.

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

Local-only repo prepared:

- Local path: `D:\Google Drive\Documents\Codex-Projects\ZedBiz-Skill-Repos\small-bite-wiki-research-skill`
- Local commit: `c60850bbef60f66b709490060b08bbc4391634f7`
- Intended remote: `https://github.com/ZedBiz44/small-bite-wiki-research-skill.git`

GitHub repo creation blocker:

- The GitHub connector available in this session can write files inside existing repositories but does not expose repository creation.
- Local `gh` CLI is not installed.
- Push failed with `Repository not found` because `ZedBiz44/small-bite-wiki-research-skill` does not exist yet.

## Follow-Up

Create an empty GitHub repo named `small-bite-wiki-research-skill` under `ZedBiz44`, then push the prepared local repo from `D:\Google Drive\Documents\Codex-Projects\ZedBiz-Skill-Repos\small-bite-wiki-research-skill`.
