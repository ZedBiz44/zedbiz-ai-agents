# 2026-06-12 - VPS1 Agent Directory Permission Cleanup

Date | Author | Status: 2026-06-12 | Cody | Completed

## Summary

- Normalized active VPS1 top-level agent directory permissions to `775`.
- Tested ownership normalization on Edith and Inga only, as the first-agent safety check.
- Rolled Edith and Inga top-level ownership back to UID/GID `1000:1000` after the stricter host-mount write test showed UID `1000` could no longer write directly at the agent root when ownership was changed to `1001:1001`.

## Live Server

- Server: `187.77.210.223`
- Base path: `/opt/openclaw/agents`
- Active agent directories normalized to `775`:
  - `amanda`
  - `edith`
  - `gohzed`
  - `grogar`
  - `inga`
  - `maggie`
  - `marsha`
  - `terry`
  - `victor`
  - `vivian`
  - `wilma`

## Edith And Inga Test

- Before the change, both Edith and Inga could write as UID `1000` inside `workspace` and at the agent `.openclaw` root.
- Edith and Inga were temporarily changed to top-level owner `1001:1001` with mode `775`.
- Container-side write checks still passed.
- A stricter host-mount test as UID `1000:1000` showed:
  - `workspace` write: passed
  - top-level agent root write: failed
- Because that could block future runtime writes, Edith and Inga top-level ownership was reverted to `1000:1000`.

## Final State

- `/opt/openclaw/agents` remains `jackadmin:jackadmin 775`.
- Edith and Inga are `ubuntu:ubuntu 775`.
- Edith and Inga `workspace` paths remain owned by `ubuntu:ubuntu` and writable by UID `1000`.
- Edith container remained healthy.
- Inga container remained healthy.

## Decision

- Permission normalization is safe and completed.
- Ownership normalization to `jackadmin:jackadmin` is not safe as a blanket rule for VPS1 agent roots.
- For VPS1, active agent roots that runtime UID `1000` may write to should remain UID/GID `1000:1000` unless the container model is changed.
