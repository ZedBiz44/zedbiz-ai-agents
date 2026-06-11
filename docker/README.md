# ZedBiz OpenClaw Base Image

## Overview

`Dockerfile.base` builds the single shared base image used by all ZedBiz OpenClaw agents on VPS1.

All agents use `ghcr.io/zedbiz44/openclaw-base:latest` instead of the stock `ghcr.io/openclaw/openclaw` image.
Victor is the exception -- his image inherits FROM this base and adds Docker CLI and SSH on top.

## Tools Included

| Tool | Version | Purpose |
|---|---|---|
| yt-dlp | 2026.06.09 | YouTube/media data scraping, playlist metadata, transcripts |
| ffprobe | 7.0.2 | Video metadata analysis |
| rclone | 1.74.3 | Cloud storage sync (Google Drive, S3, Dropbox) |
| gh | 2.94.0 | GitHub CLI |
| yq | 4.53.3 | YAML and JSON processor |
| pandoc | 3.10 | Document converter (Markdown to Word, HTML to PDF) |
| uv | 0.11.20 | Fast Python package manager (includes uvx) |
| debugpy | 1.8.21 | Python remote debugging (OpenClaw Python Debugging Skill) |
| gron | 0.7.1 | JSON flattener for grep |
| glow | 2.1.2 | Markdown renderer in terminal |
| eza | 0.23.4 | Modern ls replacement with tree view |
| duf | 0.9.1 | Disk usage overview |
| fzf | 0.73.1 | Fuzzy finder |
| freeze | 0.2.2 | Code screenshot generator |
| vhs | 0.11.0 | Terminal session recorder to GIF |
| git-cliff | 2.13.1 | Changelog generator from git commits |
| just | 1.52.0 | Task runner / command shortcuts |
| grex | 1.4.6 | Regex generator from examples |

## Build

```bash
docker build -f docker/Dockerfile.base \
  -t ghcr.io/zedbiz44/openclaw-base:2026.6.5 \
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

## Rollout History

2026-06-05 -- Initial build and rollout to all VPS1 agents by Manus.
2026-06-10 -- Version bump to OpenClaw 2026.6.5, 14 tool updates, added debugpy 1.8.21.
