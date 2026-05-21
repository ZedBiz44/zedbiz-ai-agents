# Notion GitHub Dashboard Setup

## Goal

Create a live Notion view that shows GitHub activity from `ZedBiz44/zedbiz-ai-agents` without forcing Jack to work inside GitHub every day.

## Recommended Dashboard

Use Notion as the visual dashboard and GitHub as the source of truth.

## Synced Database Setup

This part must be done inside Notion because Notion's "Paste as database" action is a workspace UI feature.

Use this repo link:

```text
https://github.com/ZedBiz44/zedbiz-ai-agents
```

Steps:

- Open the Notion page where the dashboard should live.
- Paste the GitHub repository link.
- Choose `Paste as database`.
- Name it `GitHub - ZedBiz AI Agent Tracking`.
- Place it near the top of the Technical Documentation page or the Agent Creation SOP dashboard.

## Recommended Notion Views

- `Recent Changes` - newest updates first.
- `Open Bugs` - GitHub Issues that are not closed.
- `Skills Added` - entries tagged as skills.
- `VPS Work` - entries tagged by main VPS or secondary VPS.
- `Needs Jack Decision` - issues tagged `needs-jack`.

## What GitHub Should Feed Into Notion

- Issues
- Pull requests
- commits
- changed files
- labels
- dates
- authors

## Important Limitation

The synced database shows GitHub activity, but the detailed operating record should still live in GitHub Markdown files and Issues.

Notion is the dashboard. GitHub is the audit trail.

