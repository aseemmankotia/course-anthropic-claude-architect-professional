## Chapter 9 Cheat Sheet: Claude Models, Prompting, Context Engineering

### Model Family Steering
| Model | Best For | Cost/Latency |
|---|---|---|
| Opus | Deepest reasoning, complex multi-step tasks | Highest |
| Sonnet | Balanced production default | Medium |
| Haiku | High-volume, simple, latency-sensitive tasks | Lowest |

**Exam rule:** scenario gives latency/cost/accuracy constraints → match to the family, not the 'smartest' option by default.

### Extended Thinking
- Enable via: `thinking: {type: "enabled", budget_tokens: N}`
- **Minimum budget_tokens: 1024**
- budget_tokens must be **less than** max_tokens
- Thinking tokens are billed **as output tokens** — cost scales with budget
- Use when: multi-step reasoning, planning, cross-referencing data, complex audits
- Skip when: simple classification/extraction, latency-critical paths

### Context Window Management
- Standard context window: **200K tokens** across most Claude 3.x/4.x models
- Some Sonnet variants support an extended (beta) **1M-token** context window
- **Compaction**: summarize older turns → condensed memory block; keep recent turns verbatim
- **RAG**: retrieve external knowledge on demand (different problem than compaction)
- Never resend full raw transcript indefinitely and rely on silent truncation

### Long-Running Agent Memory
- Pattern: external memory store (vector DB/file/state object) + periodic summarization
- Inject summarized state into context rather than replaying entire history
- Enterprise agents combine: RAG (facts) + compaction (history) + structured memory (state)

### Structured Output Enforcement
- **Best method**: force `tool_choice: {"type": "tool", "name": "<tool>"}` with a schema-defining tool
- Prompt-only ("respond in JSON") is unreliable — not a guarantee
- Regex/post-hoc filtering discards failures instead of preventing them

### tool_choice Reference
| Value | Behavior |
|---|---|
| `auto` | Model decides whether/which tool to call |
| `any` | Must call some tool, model picks which |
| `{type:"tool", name:...}` | Forces exactly that tool (structured output pattern) |
| `none` | Disables tool use |

### Parallel Tool-Use Orchestration
- Model may return multiple `tool_use` blocks in one response
- Orchestrator executes concurrently
- Return multiple `tool_result` blocks matched by `tool_use` id in next message

### Common Exam Traps
- Choosing Opus by default for every task (ignores cost/latency constraints)
- Forgetting budget_tokens minimum (1024) or setting it ≥ max_tokens
- Treating compaction and RAG as the same technique
- Relying on prompt wording alone for guaranteed structured output instead of forced tool_choice
