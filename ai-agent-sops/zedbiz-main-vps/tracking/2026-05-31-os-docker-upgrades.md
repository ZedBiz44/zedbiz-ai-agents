# 2026-05-31 - OS and Docker Package Upgrades -- VPS1 and VPS2

## Summary

- **Date:** 2026-05-31 Mountain Time
- **Changed By:** Jack (applied manually), Manus (verified)
- **VPS:** main-vps (187.77.210.223) and secondary-vps (VPS2 Harry/Suzy)
- **Status:** done

## Why This Was Done

VPS2 (Harry) had 8 pending package updates identified, including 6 critical kernel updates. VPS1 was checked and found to have the same kernel updates plus additional Docker and system package updates pending. Both servers were updated in the same session.

## Packages Updated

### Kernel Updates (Both VPS1 and VPS2)

| Package | From | To | Urgency |
|---|---|---|---|
| linux-image-virtual | 6.8.0-117 | 6.8.0-124 | Critical |
| linux-headers-generic | 6.8.0-117 | 6.8.0-124 | Critical |
| linux-headers-virtual | 6.8.0-117 | 6.8.0-124 | Critical |
| linux-libc-dev | 6.8.0-117 | 6.8.0-124 | Critical |
| linux-tools-common | 6.8.0-117 | 6.8.0-124 | Critical |
| linux-virtual | 6.8.0-117 | 6.8.0-124 | Critical |
| snapd | 2.74.1 | 2.75.2 | Medium |
| cloud-init | 24.1.3 | 25.3 | Low |

### VPS1-Only Additional Updates

| Package | From | To |
|---|---|---|
| docker-ce | 29.4.1 | 29.5.2 |
| docker-ce-cli | 29.4.1 | 29.5.2 |
| containerd.io | 2.2.3 | 2.2.4 |
| docker-compose-plugin | 5.1.3 | 5.1.4 |
| docker-buildx-plugin | 0.33.0 | 0.34.1 |
| docker-ce-rootless-extras | 29.4.1 | 29.5.2 |
| open-vm-tools | 12.5.0 | 13.0.0 |
| iproute2 | 6.1.0-1ubuntu6.2 | 6.1.0-1ubuntu6.3 |
| ubuntu-pro-client | 37.1 | 37.2 |

## Verification -- VPS1

- Running kernel after reboot: `6.8.0-124-generic` (confirmed)
- Docker version: `29.5.2` (confirmed)
- All 10 OpenClaw agents: healthy and running (confirmed via docker ps)
- All systemd services came back up automatically after reboot via op run
- Zero pending upgrades remaining

## Rollback Note

- Kernel rollback requires selecting previous kernel at GRUB boot menu -- not practical remotely
- Docker rollback: `sudo apt install docker-ce=5:29.4.1~ubuntu.24.04~noble` if needed

## Links

- Notion Technical Documentation: https://www.notion.so/7a1a3e33d58183e3ae0201de59538c3e
