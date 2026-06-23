# 2026-06-23 VPS2 Memory Embedding Provider Rollout

Date: 2026-06-23 | Author: Cody | Status: Final

## Summary

- Checked the active VPS2 fleet after the VPS1 memory embedding repair.
- Active agents on VPS2: Frank, Harry, Suzy.
- Found that all three agents still had `OPENAI_API_KEY` startup env pointing at the older `openai-api-key` 1Password item.
- Confirmed the newer `openai-embedding-key` item was not readable by the current VPS2 per-agent 1Password service-account tokens.
- Confirmed the existing runtime `OPENAI_API_KEY` value could call OpenAI embeddings directly for `text-embedding-3-small`.
- Found the actual OpenClaw memory failure: memory search was using the built-in `openai` provider route, which returned `429 insufficient_quota`, instead of the working runtime env key path.

## Fix Applied

- Backed up each agent config before changing it.
- Set each agent's memory search config to:
  - Provider: `openai-compatible`
  - Model: `text-embedding-3-small`
  - Base URL: `https://api.openai.com/v1`
  - API key source: env secret reference to `OPENAI_API_KEY`
  - Batch embeddings: disabled
- Rebuilt memory indexes for Frank first, then Harry and Suzy after Frank was proven.
- Restarted all three systemd services after the config/index repair.

## Verification

- Services:
  - `openclaw-frank`: active and enabled
  - `openclaw-harry`: active and enabled
  - `openclaw-suzy`: active and enabled
- Local HTTP:
  - Frank `127.0.0.1:4300`: `200`
  - Harry `127.0.0.1:4100`: `200`
  - Suzy `127.0.0.1:4200`: `200`
- Public HTTP:
  - `https://frank.zbiz.ca`: `200`
  - `https://harry.zbiz.ca`: `200`
  - `https://suzy.zbiz.ca`: `200`
- Final memory status:
  - Frank: provider `openai-compatible`, model `text-embedding-3-small`, probe `true`, index `valid`, dirty `false`, vector `true`, 75 files, 517 chunks
  - Harry: provider `openai-compatible`, model `text-embedding-3-small`, probe `true`, index `valid`, dirty `false`, vector `true`, 105 files, 776 chunks
  - Suzy: provider `openai-compatible`, model `text-embedding-3-small`, probe `true`, index `valid`, dirty `false`, vector `true`, 90 files, 646 chunks
- Real `memory search ZedBiz` returned results on Frank, Harry, and Suzy.
- Recent logs after restart showed no embedding errors or insufficient-quota errors.

## Follow-Up Note

- VPS2 is fixed operationally, but it is not fully aligned with the VPS1 1Password item naming yet.
- The `openai-embedding-key` item needs to be granted to the VPS2 service-account tokens before the `.env` files can safely be changed from `openai-api-key` to `openai-embedding-key`.
- I intentionally did not switch the `.env` files to an unreadable 1Password item, because that would break startup env resolution.
