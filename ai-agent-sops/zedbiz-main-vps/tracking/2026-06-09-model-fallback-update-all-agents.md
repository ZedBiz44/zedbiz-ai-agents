# Model Fallback Update -- All 10 Agents

**Date:** 2026-06-09 (MST)
**Recorded by:** Manus
**Status:** COMPLETED

## Change

Updated the default model config on all 10 agents from a plain string to the
primary+fallbacks object format. The fallback chain is now:

```json
{
  "primary": "openai/gpt-5.5",
  "fallbacks": [
    "openai/gpt-5.3-codex",
    "openrouter/google/gemini-3.1-flash-lite",
    "openrouter/deepseek/deepseek-v4-flash:free"
  ]
}
```

**Rationale:**
- `openai/gpt-5.5` via OAuth is the primary (best model, free via Codex OAuth)
- `openai/gpt-5.3-codex` as first fallback -- cheaper than gpt-5.5, agentic-optimized, good context window
- OpenRouter free models as last-resort fallbacks if all OpenAI auth fails

**Previous config:** `"openai/gpt-5.5"` (plain string, no fallbacks)

## Agents Updated

amanda, gohzed, grogar, inga, maggie, marsha, terry, victor, vivian, wilma

All 10 agents restarted and confirmed healthy after the change.

## Notes

- The "gpt" and "gpt-mini" entries visible in some control UI dropdowns are OpenAI's
  own live API aliases (not stored in agent config). They cannot be removed from our side.
  All agents have proper versioned gpt-5.4 / gpt-5.4-mini entries in their model lists.
- No deprecated 5.2/5.3 Codex entries were found in any agent config or catalog.
