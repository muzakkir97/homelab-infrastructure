# Cost tracking

Monthly API and infrastructure costs for the homelab AI ecosystem.

## Budget target
**$10 USD/month** for Claude API credits.

## Monthly log

| Month | Claude API | Notes |
|-------|-----------|-------|
| Apr 2026 | ~$21 | Pre-optimisation — Sonnet for all tasks |
| May 2026 | ~$5 est. | Post-optimisation — Haiku for Da Vinci, hybrid routing |

## Cost optimisation history

### May 2026 — Major reduction
- **Problem:** Da Vinci using claude-sonnet-4-20250514 with 32k output tokens = ~$0.58/run
- **Fix:** Switched to claude-haiku-4-5-20251001 = ~$0.049/run (12x cheaper)
- **Fix:** Gilgamesh confirmed using local Ollama qwen3.5 for main chat (free)
- **Result:** Estimated monthly cost dropped from ~$35–58 to ~$5

## Per-run cost breakdown (current)

| Workflow | Model | Est. cost/run |
|----------|-------|---------------|
| Da Vinci — documentation pipeline | Haiku | ~$0.049 |
| Gilgamesh chat (Ollama online) | qwen3.5 local | $0.00 |
| Gilgamesh chat (Ollama offline) | Haiku fallback | ~$0.001 |
| Life logging classifier | Haiku | ~$0.0001 |

## Future projections

With planned GPU upgrade (RX 7900 XTX + qwen3.5:27b):
- Da Vinci moves to local inference → $0/run
- All Gilgamesh queries local → $0
- Estimated monthly API cost → ~$0
