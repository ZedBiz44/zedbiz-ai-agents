# VPS2 to VPS1 SSH Key Setup - Wiki Knowledge Sync

**Date:** 2026-06-04
**Operator:** Manus
**Task:** Set up dedicated SSH key pair so VPS2 (Harry/Suzy) can rsync wiki knowledge files to VPS1 (jackadmin)

---

## Objective

Enable VPS2 agents to push AI-readable Markdown wiki files to the shared knowledge area on VPS1 via rsync over SSH, without requiring a human to manually transfer keys or run commands.

---

## Steps Completed

### Step 1 - Generate Dedicated VPS2-to-VPS1 SSH Key on VPS2

Generated a new ed25519 key on VPS2 (Harry) specifically for the wiki sync operation.

```bash
ssh vps-harry "mkdir -p /root/.ssh && chmod 700 /root/.ssh && \
  ssh-keygen -t ed25519 -f /root/.ssh/vps2_to_vps1_suzy_sync -N '' \
  -C 'vps2-suzy-to-vps1-knowledge-sync'"
```

Key location on VPS2:
- Private: `/root/.ssh/vps2_to_vps1_suzy_sync`
- Public: `/root/.ssh/vps2_to_vps1_suzy_sync.pub`

### Step 2 - Retrieved VPS2 Public Key

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJnQh5edlKXbfA/4cVrYFVJjJRYUz+vFqB3sfbjPGDZ8 vps2-suzy-to-vps1-knowledge-sync
```

### Step 3 - Added Public Key to VPS1 authorized_keys

Added the VPS2 public key to `/home/jackadmin/.ssh/authorized_keys` on VPS1.

```bash
echo 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJnQh5edlKXbfA/4cVrYFVJjJRYUz+vFqB3sfbjPGDZ8 vps2-suzy-to-vps1-knowledge-sync' \
  >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Result: `KEY_ADDED`

### Step 4 - Created Incoming Knowledge Folder on VPS1

Created the folder structure on VPS1 to receive synced wiki files from Suzy on VPS2.

```bash
ssh vps-main "mkdir -p /opt/openclaw/shared/knowledge/incoming-vps2/suzy/wiki/{sources,entities,concepts,syntheses,_attachments}"
```

Folders created:
- `/opt/openclaw/shared/knowledge/incoming-vps2/suzy/wiki/sources/`
- `/opt/openclaw/shared/knowledge/incoming-vps2/suzy/wiki/entities/`
- `/opt/openclaw/shared/knowledge/incoming-vps2/suzy/wiki/concepts/`
- `/opt/openclaw/shared/knowledge/incoming-vps2/suzy/wiki/syntheses/`
- `/opt/openclaw/shared/knowledge/incoming-vps2/suzy/wiki/_attachments/`

### Step 5 - Tested VPS2-to-VPS1 Connectivity

Ran the connection test from VPS2 using the new dedicated key:

```bash
ssh vps-harry "ssh -i /root/.ssh/vps2_to_vps1_suzy_sync \
  -o BatchMode=yes -o StrictHostKeyChecking=accept-new \
  jackadmin@187.77.210.223 'echo VPS2_TO_VPS1_OK'"
```

Result: `VPS2_TO_VPS1_OK` ✅

---

## Status

All 5 steps from the Wiki Knowledge System Notion page completed successfully.
VPS2 can now SSH into VPS1 using the dedicated sync key.
Ready for rsync dry-run and scheduled sync setup.

---

## Notes

- The folder structure follows the current Notion SOP (agent-organized incoming lanes).
- Cody Codex is updating the Notion page with a revised flat shared knowledge model per Jack's direction.
- Once the Notion page is updated, the folder structure on VPS1 may be reorganized to a flat `/opt/openclaw/shared/knowledge/` path.
- WikiLint gate and Wiki Keeper agent role to be added in the next phase.
