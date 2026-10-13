# Models by capability - ranked highest to lowest

Source: OmniRoute `model_intelligence` table (arena ELO data), synced from the gateway.
43 models, 5 capability buckets. Rank 1 is the strongest.

- Score = normalized fitness, 0.00 to 1.00, for that capability.
- ELO = raw arena rating (higher is stronger).
- Names are the original model names as OmniRoute records them.

Note: OmniRoute only separates Coding from the rest in this dataset, so Review,
Debugging, Documentation and General share one ranking. The five buckets are:
coding, review, debugging, documentation, default (general).

---

## Coding (25 models)

| # | Model | Score | ELO |
|---|-------|-------|-----|
| 1 | claude-opus-5.5-max | 0.980 | 1814 |
| 2 | gpt-6-astra | 0.900 | 1788 |
| 3 | gpt-6-astra-max | 0.900 | 1788 |
| 4 | claude-sonnet-5.5-xhigh | 0.854 | 1773 |
| 5 | gpt-6.1-sol-max | 0.804 | 1757 |
| 6 | claude-fable-5.1-max | 0.767 | 1745 |
| 7 | claude-sonnet-5.5-high | 0.678 | 1716 |
| 8 | claude-opus-5 | 0.604 | 1692 |
| 9 | claude-opus-5-max | 0.604 | 1692 |
| 10 | gpt-6-sol | 0.588 | 1687 |
| 11 | gpt-6-sol-max | 0.588 | 1687 |
| 12 | gemini-4-argon-high | 0.560 | 1678 |
| 13 | qwen3.8-max | 0.542 | 1672 |
| 14 | qwen3.8-max-0902 | 0.542 | 1672 |
| 15 | claude-opus-5-high | 0.499 | 1658 |
| 16 | muse-spark-1.3 | 0.493 | 1656 |
| 17 | muse-spark-1.3-max | 0.493 | 1656 |
| 18 | kimi-k3 | 0.489 | 1655 |
| 19 | kimi-k3-max | 0.489 | 1655 |
| 20 | grok-4.7 | 0.437 | 1638 |
| 21 | grok-4.7-xhigh | 0.437 | 1638 |
| 22 | mimo-v2.6-flash | 0.431 | 1636 |
| 23 | hy4-preview | 0.418 | 1632 |
| 24 | qwen3.8-flash-next | 0.418 | 1632 |
| 25 | mimo-v2.6-pro | 0.412 | 1630 |

## Review / Debugging / Documentation / General (27 models, one shared ranking)

| # | Model | Score | ELO |
|---|-------|-------|-----|
| 1 | gemini-4-argon-high | 0.980 | 1525 |
| 2 | claude-opus-4-6 | 0.697 | 1505 |
| 3 | claude-opus-4-6-high | 0.697 | 1505 |
| 4 | claude-fable-5 | 0.683 | 1504 |
| 5 | claude-fable-5-high | 0.683 | 1504 |
| 6 | claude-opus-5.5-high | 0.683 | 1504 |
| 7 | claude-fable-5.1-max | 0.640 | 1501 |
| 8 | claude-opus-4-7 | 0.640 | 1501 |
| 9 | claude-opus-4-7-high | 0.640 | 1501 |
| 10 | gemini-3.8-flash | 0.556 | 1495 |
| 11 | gemini-3.8-flash-high | 0.556 | 1495 |
| 12 | muse-spark-1.2 | 0.541 | 1494 |
| 13 | muse-spark-1.3 | 0.541 | 1494 |
| 14 | muse-spark-1.3-max | 0.541 | 1494 |
| 15 | muse-spark-1.1 | 0.499 | 1491 |
| 16 | claude-opus-5 | 0.485 | 1490 |
| 17 | claude-opus-5-high | 0.485 | 1490 |
| 18 | claude-opus-5-max | 0.471 | 1489 |
| 19 | muse-spark | 0.471 | 1489 |
| 20 | gemini-3.7-flash | 0.457 | 1488 |
| 21 | gemini-3.7-flash-high | 0.457 | 1488 |
| 22 | kimi-k3 | 0.457 | 1488 |
| 23 | kimi-k3-max | 0.457 | 1488 |
| 24 | gemini-3.1-pro-preview | 0.442 | 1487 |
| 25 | gemini-3-pro | 0.414 | 1485 |
| 26 | gpt-5.6-sol | 0.400 | 1484 |
| 27 | gpt-5.6-sol-xhigh | 0.400 | 1484 |

---

Empty buckets in the table: planning, analysis. No models ranked there yet.
