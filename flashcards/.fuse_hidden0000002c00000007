## Chapter 9: Claude Models, Prompting, and Context Engineering at Scale — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Minimum budget_tokens value when enabling extended thinking? | 1024 tokens. Exam tip: thinking tokens are billed as output tokens, and budget_tokens must be set below max_tokens. |
| 2 | Standard context window size advertised across most Claude 3.x/4.x models? | 200K tokens. Exam tip: some Sonnet model variants offer an extended (beta) 1M-token context window for very long documents/agent histories. |
| 3 | Opus vs Sonnet vs Haiku — production steering rule of thumb? | Opus = max reasoning capability, highest cost/latency, for hardest tasks. Sonnet = balanced default for most production agents. Haiku = fastest/cheapest, for high-volume simple tasks. |
| 4 | What are the valid tool_choice values in the Messages API? | "auto" (model decides), "any" (must call some tool), {"type":"tool","name":...} (force specific tool), "none" (no tool use). Exam tip: forced specific tool = structured output pattern. |
| 5 | Context compaction pattern for long-running agents? | Periodically summarize older turns/tool results into a condensed memory block, keep recent turns verbatim, re-inject the summary to stay under the context window limit. |
| 6 | Long-running agent memory pattern beyond raw transcript replay? | Use an external memory store (vector DB, file, or state object) plus periodic summarization injected into the system/context, instead of resending full history every call. |
| 7 | Extended thinking budget tradeoff — when to raise vs skip it? | Raise budget_tokens for complex multi-step reasoning/planning tasks needing accuracy. Skip/disable thinking for simple, latency-sensitive, high-volume tasks. Exam tests matching budget to task complexity. |
| 8 | API syntax to enable extended thinking? | Pass thinking: {"type": "enabled", "budget_tokens": N} in the request. N must be >= 1024 and less than max_tokens. |
| 9 | Best practice to guarantee valid JSON for downstream system consumption? | Force tool_choice to a single tool whose input_schema defines the exact required JSON structure — far more reliable than prompting the model to 'output JSON'. |
| 10 | How does parallel tool-use orchestration work in a single turn? | The model can return multiple tool_use blocks in one response; the orchestrator executes them concurrently and returns matching tool_result blocks (matched by tool_use id) in the next message. |
| 11 | How are extended thinking tokens billed? | Thinking tokens are billed as output tokens. Exam tip: a large budget_tokens value directly increases cost and latency — size it to task complexity, not by default maxing it out. |
| 12 | Context engineering strategy for multi-turn enterprise agents at scale? | Combine RAG (retrieve external facts on demand) + compaction (condense accumulated conversation/tool history) + structured memory (persisted state object) — never just stuff the full transcript into every call. |
