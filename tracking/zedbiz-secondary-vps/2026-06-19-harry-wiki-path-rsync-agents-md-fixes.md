# 2026-06-19 | Manus | Harry Wiki Path, Rsync Fixes, Frank Added, AGENTS.md Standardized
Date: 2026-06-19 Mountain Time
Agent: Manus
Status: Done
Server: VPS2 / Hostinger / 2.24.104.80

## Summary
Four related issues resolved in one session: Harry's wiki was stored in the wrong location, Harry's openclaw.json pointed to the old path, Frank was missing from the rsync sync script entirely, and Harry and Frank had incomplete AGENTS.md filing rules compared to Suzy.

---

## Issue 1: Harry Wiki Path Wrong

**Problem:** Harry's wiki content was split across two wrong locations:
- `/root/.openclaw-harry/wiki/main` (stray top-level wiki dir)
- `/root/.openclaw-harry/workspace/wiki/main` (nested main subdir inside workspace)

Target (matching Suzy and Frank): `/root/.openclaw-harry/workspace/wiki`

**Fix:**
- Used `cp -a` to merge `/root/.openclaw-harry/wiki/main/` into `/root/.openclaw-harry/workspace/wiki/` (used `wiki/main` as authoritative source -- it had more files including `lint.md`, `stale-pages.md`, `contradictions.md`, `lightningwp-research.md`)
- Removed `/root/.openclaw-harry/workspace/wiki/main` subdir
- Removed `/root/.openclaw-harry/wiki/` tree entirely

**Result:** Harry's wiki now lives at `/root/.openclaw-harry/workspace/wiki` -- identical structure to Suzy and Frank.

---

## Issue 2: Harry openclaw.json Pointed to Old Path

**Problem:** Harry's `memory-wiki` plugin config in `/root/.openclaw-harry/openclaw.json` still had:
```
"path": "/root/.openclaw-harry/wiki/main"
```

**Fix:** Updated via `sed` to:
```
"path": "/root/.openclaw-harry/workspace/wiki"
```

Restarted `openclaw-harry` service. Confirmed `active (running)`.

---

## Issue 3: Harry Missing From Rsync, Sync Script Aborting Early

**Problem:** The sync script `/root/bin/sync-vps2-wikis-to-vps1.sh` had two bugs:
1. Harry's path was still `wiki/main` (old path)
2. `set -euo pipefail` caused the script to abort on Suzy's rsync permission warnings (exit code 23) before ever reaching Harry's section

**Fix:**
- Updated Harry's path in the script to `/root/.openclaw-harry/workspace/wiki`
- Removed `set -e` (kept `set -uo pipefail`)
- Added `|| echo "WARN: rsync for $name exited with code $? (partial transfer is OK)"` to the rsync call so permission errors are logged but do not abort the run

**Result:** Harry's two missing research files synced to VPS1:
- `syntheses/lightningim-research.md`
- `syntheses/lightningwp-research.md`

---

## Issue 4: Frank Never Added to Rsync Script

**Problem:** Frank was set up on VPS2 after the rsync SOP was originally written and was never added to the sync script. His wiki content (`david-harder-and-syntac-business-research.md`, `zed-web-hosting-website-research.md`) never reached VPS1.

**Fix:** Added Frank to `/root/bin/sync-vps2-wikis-to-vps1.sh`:
```bash
sync_wiki suzy  /root/.openclaw-suzy/workspace/wiki
sync_wiki frank /root/.openclaw-frank/workspace/wiki
sync_wiki harry /root/.openclaw-harry/workspace/wiki
```

Ran manual sync. Frank's files confirmed on VPS1:
- `syntheses/david-harder-and-syntac-business-research.md`
- `syntheses/zed-web-hosting-website-research.md`

---

## Issue 5: AGENTS.md Filing Rules Missing From Harry and Frank

**Problem:** Harry and Frank had only the generic 5-line boilerplate in their `AGENTS.md`. Suzy had the full ZedBiz Research Filing Rules section. This caused Harry and Frank to skip creating `sources/` entries when doing website research -- they went straight to synthesis without logging the raw crawl record.

**Fix:** Wrote the canonical `AGENTS.md` to all three agents:
- `/root/.openclaw-harry/workspace/wiki/AGENTS.md`
- `/root/.openclaw-frank/workspace/wiki/AGENTS.md`
- `/root/.openclaw-suzy/workspace/wiki/AGENTS.md`

All three confirmed identical via md5sum: `26116c8e029a972f54921c14c825aa59`

**Canonical content includes:**
- Standard plugin-owned block rules
- ZedBiz Research Filing Rules: sources/ for raw material, entities/ for durable things, concepts/ for reusable ideas, syntheses/ for cleaned-up conclusions
- wiki_search / wiki_get before answering, wiki_lint after changes

---

## Notion SOP Updated

The Step-By-Step Rsync Setup SOP (https://app.notion.com/p/375a3e33d58181b2a8aecccc344c30eb) was updated with:
- Harry's corrected wiki path in all 3 occurrences
- Frank added to the verified VPS2 wiki folders list
- Frank added to the Step 7 verification command
- Frank added to the sync script code block in Step 11

---

## Current Sync Script State
```
sync_wiki suzy  /root/.openclaw-suzy/workspace/wiki
sync_wiki frank /root/.openclaw-frank/workspace/wiki
sync_wiki harry /root/.openclaw-harry/workspace/wiki
```
Cron: `*/30 * * * *` on VPS2 root crontab -- unchanged.
