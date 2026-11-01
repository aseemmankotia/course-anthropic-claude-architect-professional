## Chapter 6 Cheat Sheet — Evaluation, Testing, and Optimization (16%)

### Prompt Caching
| Setting | Value |
|---|---|
| Default (ephemeral) cache TTL | 5 minutes, refreshed on each hit |
| Extended cache TTL option | 1 hour (beta) |
| Min cacheable prefix — Opus/Sonnet | 1024 tokens |
| Min cacheable prefix — Haiku | 2048 tokens |
| Cache write cost (5-min) | ~1.25x base input price |
| Cache write cost (1-hr) | ~2x base input price |
| Cache read cost | ~10% of base input price (~90% discount) |

**Rule of thumb:** cache the large, static prefix (system prompt, reference docs); put the unique/variable content after the cache breakpoint. Only worth it if the prefix is reused enough times to amortize the write cost.

### Message Batches API
| Setting | Value |
|---|---|
| Cost discount | 50% off input + output tokens |
| Processing window | up to 24 hours |
| Max requests per batch | 100,000 |
| Max batch size | 256 MB |

**Use when:** large volume, no real-time requirement. **Don't use when:** latency-sensitive or interactive.

**Caching vs Batching:** caching cuts cost of *repeated identical prefixes*; batching cuts cost of *large async volume* regardless of repetition. They can be combined.

### LLM-as-Judge Bias Table
| Bias | Symptom | Mitigation |
|---|---|---|
| Position bias | Favors first/last option | Randomize/swap order, average |
| Verbosity bias | Favors longer answers | Rubric ignoring length |
| Self-preference bias | Favors own model family | Use different judge model/vendor, ensemble |

### Golden Sets & Regression
- Keep a **held-out** golden set never used for prompt tuning — prevents overfitting.
- Build **scoring rubrics** with explicit, binary-checkable criteria per example.
- Regression suite reruns golden set on every prompt/agent change; compare against a pass threshold.
- For agents: log **full traces** — tool calls, arguments, results, per-step latency/tokens — not just final output.

### Statistical Significance
- Same examples, two versions → **paired test** (e.g., McNemar's test for binary pass/fail).
- Report confidence interval / p-value, not just raw percentage difference.
- Small samples (<100) are noise-prone — don't trust a few-point swing without a significance test.

### Exam Traps
- Confusing caching (repeat-prefix savings) with batching (bulk async savings).
- Assuming 1024-token cache minimum applies to all models (Haiku = 2048).
- Mistaking a raw percentage gain for statistical significance.
- Treating final-output-only logs as sufficient agent observability.
