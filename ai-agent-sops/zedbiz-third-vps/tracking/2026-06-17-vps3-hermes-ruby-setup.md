# VPS3 Hermes/Ruby Setup -- 2026-06-17
Date: 2026-06-17 | Author: Cody | Status: Active

## Summary
Provisioned VPS3 (Hostinger, IP 2.25.210.154) as the home for Hermes/Ruby.
Completed full bootstrap, Docker/Caddy install, Hermes deployment, endpoint hardening, and dashboard fixes.

## VPS3 Baseline
- OS: Ubuntu 24.04.4 LTS
- Kernel: 6.8.0-124-generic
- Hostname: srv1764917
- User: root
- SSH key: dedicated Hermes VPS3 key (generated 2026-06-17)

## SSH Key Setup
- Generated dedicated SSH key pair for VPS3 Hermes
- Private key stored locally at C:\Users\zener\.ssh\hermes_vps3_key
- Public key added to Hostinger SSH Keys panel
- SSH access confirmed: `root@2.25.210.154`

## Installation Completed
- Installed Docker on VPS3
- Installed Caddy on VPS3
- Deployed Hermes (NousResearch provider) with persistent data at /opt/hermes-ruby
- Configured ruby.zbiz.ca through Caddy as a token-protected Hermes API endpoint

## Endpoint Hardening
- Narrowed Caddy for ruby.zbiz.ca: only /v1/* proxied to Hermes
- No-token /health returns 404
- No-token /v1/models returns 401
- Token-authenticated /v1/models returns Ruby model listing
- Token-authenticated chat completion returns "Ruby is online."

## Access Decision
- Keep ruby.zbiz.ca private at initial launch
- Access via Hermes API_SERVER_KEY for backend/API access
- Open WebUI admin login or Caddy basic auth for public ruby.zbiz.ca doorway

## Dashboard Fixes
- Re-activated ruby-calm light dashboard theme
- Added ruby-readable chat skin
- Updated dashboard terminal patch for readability
- Dashboard loads with status 200; chat surface renders light with dark readable terminal text

## Paste Support Fix
- Text paste works in embedded chat terminal
- Image paste uploads clipboard images to /opt/data/images/ and inserts saved path into chat
- Verified https://ruby.zbiz.ca returns 200 after patch

## Hermes Installation SOP
- Created Notion page: Hermes Installation SOP
- SOP covers: server bootstrap, Docker, Nous Portal setup, API server, Open WebUI, ruby.zbiz.ca,
  xAI Grok OAuth migration, backups, and required decisions

## GitHub Status
- Local VPS3 project folder exists but is not yet a GitHub repo
- GitHub technical-source record still needs a destination repo (repo creation not exposed in current GitHub connector)

## Notion Reference
https://app.notion.com/p/382a3e33d5818145bd19e21859312228
