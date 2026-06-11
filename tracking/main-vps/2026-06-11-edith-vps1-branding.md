# 2026-06-11 - Edith VPS1 OpenClaw Branding

Date: 2026-06-11 Mountain Time
Agent: Cody
Status: Complete

## Summary

- Applied Edith's uploaded branding assets to the live VPS1 Docker OpenClaw agent.
- Updated Edith's mounted branding source folder at `/opt/openclaw/agents/edith/branding`.
- Ran Edith's existing post-start branding hook so the live container immediately picked up the new avatar, favicons, and browser title.
- Verified the public Edith control UI is still healthy and serving the updated browser title and favicon assets.

## Source Assets Used

- `edith-avatar-white.png` -> `branding/avatar.png`
- `favicon.ico` -> `branding/favicon.ico`
- `edith-favicon-32.png` -> `branding/favicon-32.png`
- `apple-touch-icon.png` -> `branding/apple-touch-icon.png`
- Generated a small `favicon.svg` wrapper from the uploaded 32px Edith favicon so the SVG favicon path matches the new branding.

## VPS1 Changes

- Server: `187.77.210.223`
- Agent: `edith`
- Agent directory: `/opt/openclaw/agents/edith`
- Branding directory: `/opt/openclaw/agents/edith/branding`
- Backup created before replacement:
  - `/opt/openclaw/agents/edith/backups/branding-before-cody-20260611-001740.tgz`
- Files replaced in the host branding directory:
  - `avatar.png`
  - `favicon.ico`
  - `favicon.svg`
  - `favicon-32.png`
  - `apple-touch-icon.png`
- Ownership set to UID/GID `1000:1000`.
- Permissions set to `644`.

## Verification

- Container `edith` was running and healthy before and after the change.
- Edith's post-start hook completed:
  - Avatar set
  - Favicons copied to the root UI directory
  - Browser title set to `Edith`
- Container verification:
  - `/home/node/.openclaw/avatars/openclaw.png` is present, `1936186` bytes, `node:node`, `644`.
  - `/app/dist/control-ui/favicon.ico` is present, `158017` bytes, `node:node`, `644`.
  - `/app/dist/control-ui/favicon.svg` is present, `3483` bytes, `node:node`, `644`.
  - `/app/dist/control-ui/favicon-32.png` is present, `2517` bytes, `node:node`, `644`.
  - `/app/dist/control-ui/apple-touch-icon.png` is present, `57054` bytes, `node:node`, `644`.
  - `/app/dist/control-ui/index.html` contains `<title>Edith</title>`.
- Public URL verification:
  - `https://edith.zbiz.ca/` returned `<title>Edith</title>`.
  - `https://edith.zbiz.ca/favicon.ico` returned `HTTP/2 200`.
  - `https://edith.zbiz.ca/favicon.svg` returned `HTTP/2 200`.
  - `https://edith.zbiz.ca/favicon-32.png` returned `HTTP/2 200`.
  - `https://edith.zbiz.ca/apple-touch-icon.png` returned `HTTP/2 200`.

## Notes

- The old VPS2 Edith folder-based setup still exists, but this task was applied to the live VPS1 Docker agent because Edith is now present and healthy there.
- Direct public avatar routes `/avatar.png` and `/avatars/openclaw.png` returned `404`; this is not treated as a failure because the OpenClaw app uses the internal avatar file path copied by the post-start hook.
