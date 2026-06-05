# ZedBiz OpenClaw Base Image

## Overview

`Dockerfile.base` builds the single shared base image used by all ZedBiz OpenClaw agents on VPS1.

All agents use `ghcr.io/zedbiz44/openclaw-base:latest` instead of the stock `ghcr.io/openclaw/openclaw` image.
Victor is the exception -- his image inherits FROM this base and adds Docker CLI and SSH on top.

## Tools Included

| Tool | Version | Purpose |
|---|---|---|
| yt-dlp | 2026.03.17 | YouTube/media data scraping, playlist metadata, transcripts |
| ffprobe | 7.0.2 | Video metadata analysis |
| rclone | 1.69.3 | Cloud storage sync (Google Drive, S3, Dropbox) |
| gh | 2.74.1 | GitHub CLI |
| yq | 4.45.4 | YAML and JSON processor |
| pandoc | 3.7.0.2 | Document converter (Markdown to Word, HTML to PDF) |
| uv | 0.7.8 | Fast Python package manager (includes uvx) |
| gron | 0.7.1 | JSON flattener for grep |
| glow | 2.1.0 | Markdown renderer in terminal |
| eza | 0.21.3 | Modern ls replacement with tree view |
| duf | 0.8.1 | Disk usage overview |
| fzf | 0.62.0 | Fuzzy finder |
| freeze | 0.1.6 | Code screenshot generator |
| vhs | 0.9.0 | Terminal session recorder to GIF |
| git-cliff | 2.9.0 | Changelog generator from git commits |
| just | 1.40.0 | Task runner / command shortcuts |
| grex | 1.4.5 | Regex generator from examples |

## Build

```bash
docker build -f docker/Dockerfile.base \
  -t ghcr.io/zedbiz44/openclaw-base:2026.5.28 \
  -t ghcr.io/zedbiz44/openclaw-base:latest \
  .
```

## Updating a Tool

Edit the `ARG` version at the top of `Dockerfile.base`, rebuild, and restart agents.

## Adding a New Tool

Add a new `RUN` block to `Dockerfile.base` following the existing pattern.
Rebuild the image and restart all agents -- they all pick it up automatically.

## Agent Configuration

Each agent's `.env` file should contain:
```
OPENCLAW_IMAGE=ghcr.io/zedbiz44/openclaw-base:latest
```

The `docker-compose.yml` uses `${OPENCLAW_IMAGE}` to pull the image at startup.

## Rollout Date

2026-06-05 -- Initial build and rollout to all VPS1 agents by Manus.
