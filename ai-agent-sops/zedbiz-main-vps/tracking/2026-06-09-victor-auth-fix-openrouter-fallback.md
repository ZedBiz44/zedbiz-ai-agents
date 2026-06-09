# Victor Auth Fix + OpenRouter Fallback

**Date:** 2026-06-09 (MST)
**Recorded by:** Manus
**Status:** COMPLETED

## Changes Made

### 1. Added openai:api-key-backup to Victor's auth-profiles.json

Victor was missing the `openai:api-key-backup` profile that all other 9 agents have.
This was added by reading the key from Terry's auth-profiles.json and writing it to
the correct host path: `/opt/openclaw/agents/victor/agents/main/agent/auth-profiles.json`

Victor was restarted and confirmed healthy with both profiles loaded.

**Before:**
- profiles: `['openai-codex:jzedbiz@gmail.com']`

**After:**
- profiles: `['openai-codex:jzedbiz@gmail.com', 'openai:api-key-backup']`

### 2. Added OpenRouter model fallbacks to Victor's openclaw.json

Victor's default model was a plain string `"openai/gpt-5.5"`. Updated to the
primary+fallbacks object format matching Inga's config:

```json
{
  "primary": "openai/gpt-5.5",
  "fallbacks": [
    "openai/gpt-5.5",
    "openrouter/google/gemini-3.1-flash-lite",
    "openrouter/deepseek/deepseek-v4-flash:free"
  ]
}
```

**Note:** Model-level fallbacks kick in on model errors, not auth errors.
If OAuth expires, the auth.order chain (OAuth -> API key) handles it.
If the API key also fails, the model fallbacks to OpenRouter models.

### Files Modified

- `/opt/openclaw/agents/victor/agents/main/agent/auth-profiles.json`
- `/opt/openclaw/agents/victor/config/openclaw.json`

### Test Result

Victor restarted healthy. Both profiles confirmed loaded inside container.

### Rollback

To revert auth-profiles: remove the `openai:api-key-backup` key from the profiles object.
To revert model config: change model back to plain string `"openai/gpt-5.5"`.
