# VPS1 Fleet Status Audit & Issue Review — 2026-05-27

**Session:** Manus — OpenClaw VPS Fleet Status Audit
**Date:** 2026-05-27 (MDT)
**Objective:** Audit all 10 agents on VPS1, check Notion Technical Documentation, review open issues, and verify fleet-wide configuration alignment before starting customization.

---

## 1. Fleet Status Summary

All 10 agent containers are currently **running and healthy** on Docker, but deep analysis of their host-side configuration files revealed critical gaps in systemd survival, configuration consistency, and security.

### Docker Container & Image Status
| Agent | Port | Image Version | Container Status | Systemd Service |
|---|---|---|---|---|
| **terry** | 3010 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled & Working |
| **amanda** | 3000 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |
| **victor** | 3002 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |
| **wilma** | 3003 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |
| **inga** | 3004 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |
| **marsha** | 3005 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |
| **gohzed** | 3006 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |
| **grogar** | 3007 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |
| **maggie** | 3008 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |
| **vivian** | 3009 | `2026.5.20` | ✅ Up 4 days (healthy) | Enabled (Broken on reboot) |

---

## 2. Key Inconsistencies & Issues Found

### A. The systemd Reboot Loophole (Critical)
- **Problem:** All 10 agent systemd services are enabled (`systemctl is-enabled`), but **only Terry** has the actual `/opt/openclaw/agents/terry/start-agent.sh` script in his directory. The other 9 agents are completely missing this script. Their systemd unit files point directly to `/opt/openclaw/agents/<agent>/start-agent.sh`.
- **Impact:** On the next VPS reboot, systemd will try to start the 9 agents, fail immediately because the script is missing, and the agents will remain dead.

### B. Discord Token Missing in Vivian Config (Critical)
- **Problem:** Vivian's `/opt/openclaw/agents/vivian/config/openclaw.json` is completely missing the `token` block inside her `channels.discord` configuration. While her `.env.resolved` has the correct token, OpenClaw does not know to pull it from the environment because the configuration block is missing.
- **Impact:** Vivian cannot connect to Discord.

### C. Duplicate Gateway Tokens (Security/Identity Risk)
- **Problem:** **Marsha** and **Maggie** share the exact same gateway token:
  `487de9765521a30cb0e1c55f9cd453c7eec690fc21402d9a`
- **Impact:** This causes session delivery queues and control UI connections to conflict between Marsha and Maggie.

### D. AllowedOrigins Inconsistencies
- **Problem:** Older configurations (amanda, victor, wilma, inga, marsha, maggie) have `http://localhost` and `http://127.0.0.1` in their `allowedOrigins` block. Newer configurations (gohzed, grogar, vivian, terry) strictly allow their specific subdomain (e.g., `https://gohzed.zbiz.ca`).
- **Impact:** Minor, but breaks exact parity.

### E. Local Skills Directory Gap
- **Problem:** **Terry** is the only agent with a `/config/skills/` directory containing the `sessions-log` skill. The other 9 agents are completely missing the local `skills/` folder.
- **Impact:** Parity gap.

### F. Leftover Stray Files
- **amanda:** `/opt/openclaw/agents/amanda/apply_cfg.py` directory still exists.
- **maggie:** `/opt/openclaw/agents/maggie/config/config.json` stray file exists.
- **vivian:** `/opt/openclaw/agents/vivian/config/token.txt` stray file exists.

---

## 3. Order of Operations to Align the Fleet

To ensure all 10 agents are exactly identical and ready for individual customization, we must execute the following:

1. **Deploy missing start-agent.sh scripts** to the other 9 agents so they survive reboots via systemd.
2. **Inject the missing Discord token block** into Vivian's `openclaw.json` so she can connect to Discord.
3. **Regenerate unique Gateway tokens** for Marsha and Maggie to resolve the token conflict.
4. **Clean up all stray files** across amanda, maggie, and vivian directories.
5. **Create the local skills directory** across the other 9 agents to match Terry's structure.

---

## 4. Next Steps & Action Plan

These findings have been logged in Notion and GitHub. Once Jack confirms, we will execute the alignment steps to achieve perfect fleet-wide parity.
