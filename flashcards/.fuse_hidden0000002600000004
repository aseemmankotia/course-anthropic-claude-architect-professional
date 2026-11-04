## Chapter 6: Evaluation, Testing, and Optimization — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Default ephemeral prompt cache TTL? | 5 minutes, refreshed on each cache hit. Exam tip: a separate 1-hour cache option exists at a higher write cost. |
| 2 | Minimum prefix length to enable caching on Opus/Sonnet-class models? | 1024 tokens. Haiku-class models require 2048 tokens minimum. Below threshold, cache_control has no effect. |
| 3 | Cache READ cost vs base input token price? | ~10% of base input price (≈90% discount). Caching only pays off with repeated reads of the same prefix. |
| 4 | Cache WRITE cost (5-min cache) vs base input price? | ~1.25x base input price for the initial write. 1-hour TTL cache write costs ~2x base input price instead. |
| 5 | Message Batches API cost discount vs standard Messages API? | Flat 50% discount on input and output tokens. Trade-off: results arrive within a processing window of up to 24 hours. |
| 6 | Max requests / size per Message Batch? | 100,000 requests OR 256MB total request size, whichever limit is hit first. Split larger jobs across multiple batches. |
| 7 | Position bias in LLM-as-judge evaluation? | Judge favors whichever answer is placed first/last regardless of quality. Mitigation: randomize/swap order across trials and average scores. |
| 8 | Self-preference bias in LLM-as-judge? | Judge rates outputs from its own model family higher. Mitigation: use a different model/vendor as judge, or ensemble multiple judges. |
| 9 | Verbosity bias in LLM-as-judge? | Judge tends to rate longer responses as better regardless of correctness. Mitigation: explicit rubric instructing judge to ignore length. |
| 10 | Why use a held-out golden dataset instead of tuning directly on the eval set? | Prevents overfitting prompt/agent changes to known test cases so measured gains generalize to real production traffic. |
| 11 | Correct statistical test for comparing pass/fail of two prompt versions on the SAME golden-set examples? | A paired test, e.g., McNemar's test for paired binary outcomes. Raw percentage-point gaps on small samples (<100) aren't reliable without it. |
| 12 | What must agent regression observability capture beyond final output? | Full trace: each tool call, arguments, results, reasoning steps, and per-step latency/token usage — needed to localize where a regression occurred. |
