# Answer Key — Practice Test 2

**Q1. Correct: A**

Token passthrough with server-side RBAC enforcement preserves per-user identity, enables fine-grained audit logging tied to the actual clinician, and keeps authorization as a server-side control rather than a client-side or model-side assumption.

---

**Q2. Correct: A**

Idempotency keys based on a unique event ID, checked against a persisted store before invoking the downstream tool, is the standard pattern for safely handling at-least-once delivery semantics regardless of Claude's involvement.

---

**Q3. Correct: C**

Introducing a middleware/adapter layer respects the existing security policy (no new direct network path to the mainframe), provides a clean abstraction boundary for the MCP server, and allows modern auth, rate limiting, and monitoring to be layered in front of the legacy system — the standard enterprise pattern for legacy constraints.

---

**Q4. Correct: D**

Hybrid retrieval combining exact-match structured lookups for high-cardinality identifiers with vector search for unstructured text is the correct enterprise pattern — vector similarity is unreliable for exact-match requirements like SKUs, while exact-match indexes can't handle semantic description queries.

---

**Q5. Correct: B**

A queue-based worker pool with adaptive concurrency driven by rate-limit headers and exponential backoff on throttling responses is the standard backpressure-aware pattern for large-scale batch integration, maximizing throughput while respecting both the Claude API and downstream system limits.

---

**Q6. Correct: A**

The least-privilege principle requires that destructive capabilities simply not be reachable from a context where they shouldn't be used, rather than relying on prompts, conversational confirmations, or after-the-fact remediation — all of which are model-dependent or reactive controls, not enforceable security boundaries.

---

**Q7. Correct: C**

Token exchange (RFC 8693) lets the MCP server obtain a downstream-scoped token that preserves the calling user's identity and department scope per request, satisfying both least-privilege authorization and per-user auditability without exposing credentials.

---

**Q8. Correct: B**

An idempotency key checked and stored by the consumer before mutating state is the standard pattern that guarantees exactly-once effect regardless of delivery semantics, directly solving the duplicate-decrement problem at the point of state change.

---

**Q9. Correct: C**

Mapping the copybook definition to named structured fields and chunking at the logical record boundary (one account with its related sub-records) preserves semantic integrity per chunk and allows precise field-level citations, which fixed-width or boundary-agnostic approaches cannot provide.

---

**Q10. Correct: A**

At high volume, matching model capability to task complexity via a deterministic classify-then-route workflow minimizes cost while preserving quality where it matters, and reserves human escalation for genuinely ambiguous cases. This is the canonical cost/quality-tiering pattern for enterprise-scale support.

---

**Q11. Correct: B**

Haiku is the fastest model in the family for low-latency, low-complexity lookups, and streaming returns tokens as they're generated, minimizing perceived latency. Extended thinking is unnecessary for simple factual answers and would only add latency.

---

**Q12. Correct: D**

Prompt caching is specifically designed for this scenario: a large, unchanging prefix reused across many requests. Caching the policy block avoids reprocessing the full 20k tokens each time, cutting both cost and time-to-first-token on cache hits.

---

**Q13. Correct: B**

This matches Anthropic's published agentic pattern definitions: orchestrator-workers dynamically breaks down and delegates subtasks based on the input, whereas evaluator-optimizer pairs a generator and a critic/evaluator in an iterative feedback loop to refine output quality.

---

**Q14. Correct: C**

529 errors are transient overload signals. The standard resilience pattern is exponential backoff with jitter to avoid retry storms, combined with a fallback path (different model tier, region, or provider) if retries are exhausted, preserving availability for end users.

---

**Q15. Correct: A**

Open-ended tasks with an unpredictable number of steps and dynamic tool/source selection are the canonical use case for autonomous agents, as opposed to workflows, which are better suited to predictable, well-defined paths. The agent's own planning loop adapts to each case's variable complexity.

---

**Q16. Correct: B**

This combines the three resilience/cost patterns the exam expects together: exponential backoff with jitter (not fixed delay) correctly handles transient 429/529 errors without thundering-herd retries; multi-region/multi-endpoint deployment provides failover against regional outages; prompt caching eliminates redundant token cost for the repeated system prompt and few-shot content; and the Batch API is the correct choice for large non-urgent volume because it is priced lower and designed for exactly this throughput pattern.

---

**Q17. Correct: A**

The orchestrator-worker multi-agent pattern is the architecture Anthropic documents for exactly this class of problem: it gives each subagent an isolated context window (avoiding context bloat), enables true parallelism across the 50 sources (controlling latency), and preserves per-subagent traceability for audit since each subagent's work can be inspected independently before the lead agent synthesizes the final output.

---

**Q18. Correct: B**

Swapping the order of the two candidate responses and averaging or reconciling the two resulting judgments is the standard documented technique for canceling out positional bias in pairwise LLM-as-judge setups, since it forces the bias to work against itself across the two trials.

---

**Q19. Correct: D**

Prompt caching and the Message Batches API discounts stack: caching the repeated 3,000-token system prompt yields up to a 90% discount on cached input tokens after the first write, while routing through Batches adds a further 50% discount on top of standard pricing since no real-time latency is required. Combining both captures the maximum available savings.

---

**Q20. Correct: C**

Claude models enforce a minimum cacheable prompt length (1024 tokens for Opus and Sonnet, 2048 tokens for Haiku). A 900-token block falls under that threshold, so the API silently processes it as a normal, uncached prompt with no error raised.

---

**Q21. Correct: D**

The suite is blind to this regression because it only checks whether the final answer matches, not how many steps or tool calls were used to get there. A complete agent regression suite needs assertions on trajectory metrics such as tool-call count, tool-call ordering, and step efficiency to catch planning degradations that don't affect final correctness.

---

**Q22. Correct: D**

The Message Batches API is designed exactly for this profile: large-volume, non-latency-sensitive, asynchronous workloads, offering a 50% cost discount versus standard pricing with results typically available well within the 24-hour processing window, which comfortably meets the 9 AM deadline.

---

**Q23. Correct: A**

Tool-call round trips per completed task directly captures how many planning-and-execution cycles the agent needs to resolve a task. An increase in this count, with accuracy held constant, is the clearest sign of a planning efficiency regression that would otherwise be invisible to accuracy-based metrics alone.

---

**Q24. Correct: B**

The static 22K-token system prompt and tool schema are ideal prompt-caching candidates (well over the 1024-token minimum) since cached reads are billed at a steep discount versus full-price reprocessing on every call. Because the regression results aren't needed until the next morning, routing both the agent calls and the judge-scoring calls through the Batch API adds a substantial additional cost discount with no fidelity loss, since batch processing doesn't change model behavior or outputs — it only changes delivery timing. Combining caching with batching directly attacks the two real cost drivers (repeated static context and unnecessary synchronous pricing) while leaving the rubric and golden set intact.

---

**Q25. Correct: D**

Human-in-the-loop confirmation for irreversible, state-changing actions creates a system-level control boundary that a prompt injection embedded in tool output cannot bypass, while still allowing autonomous read-only operations — this is the core least-privilege/defense-in-depth pattern tested for MCP tool design in regulated workflows.

---

**Q26. Correct: C**

HIPAA requires a covered entity to have an executed Business Associate Agreement with any vendor that processes PHI on its behalf; this is a contractual/legal prerequisite, not a model configuration, and must be in place before any PHI is sent to the API.

---

**Q27. Correct: B**

Data residency is an infrastructure/region guarantee — deploying via a hyperscaler's EU region ensures inference and storage stay within that geographic boundary, which is how EU data residency requirements are actually satisfied for Claude deployments.

---

**Q28. Correct: C**

Defense-in-depth at the system level requires controls that operate outside the model's own reasoning — an output filter that catches leaked secrets/identifiers and sanitized error handling form an independent layer that still protects even if an adversarial prompt bypasses the model's instruction-following.

---

**Q29. Correct: C**

Anthropic's Responsible Scaling Policy defines AI Safety Levels (ASL) as organization-level commitments about safety and security practices required before training/deploying models of a given capability, which is distinct from — and complementary to — the deployment-time controls an architect implements in their own application.

---

**Q30. Correct: A**

Regulatory audit requirements for identity attribution and tamper-evidence can only be met by a customer-controlled, append-only audit logging system built at the application layer, since the vendor's own abuse-monitoring logs are not designed for, or guaranteed to retain, per-employee attribution or long-term compliance evidence.

---

**Q31. Correct: B**

Model aliases are convenience pointers that can silently shift to a new underlying dated snapshot, introducing behavior or output changes with no corresponding code change or review gate — a serious production stability risk that dated-snapshot pinning avoids.

---

**Q32. Correct: D**

A deliberate, evaluated migration — regression testing against representative prompts, staged rollout, and monitoring, completed before the retirement deadline — is the standard lifecycle-management approach and avoids both premature regressions and last-minute outages.

---

**Q33. Correct: B**

Anthropic's official documentation and account-level deprecation notifications are the authoritative, verifiable source of truth for lifecycle dates, and are what the exam expects architects to rely on for compliance-grade planning.

---

**Q34. Correct: B**

The dated snapshot string passed in the `model` parameter is the only mechanism that guarantees a fixed set of underlying weights for every call, which is required for audit-grade reproducibility.

---

**Q35. Correct: A**

Deprecation marks the start of a notice period during which the model still functions but customers are expected to migrate; retirement is the point at which the model is actually removed and stops responding to API calls.

---

**Q36. Correct: A**

Anthropic's Responsible Scaling Policy defines AI Safety Levels (ASL); Claude Opus 4 was deployed under ASL-3 standards because capability evaluations crossed a threshold requiring stricter security and deployment controls, independent of any customer's own compliance program.

---

**Q37. Correct: A**

At 2M requests/day, cost and latency dominate for the 99% simple case — Haiku is purpose-built for high-throughput, low-latency classification. Tiered routing reserves the more expensive, higher-latency model only for the small subset of genuinely complex tasks, which is the standard production pattern for model selection at scale.

---

**Q38. Correct: D**

Extended thinking requires a budget_tokens value of at least 1024, and while it is enabled the API does not allow overriding temperature/top_p/top_k (sampling is fixed), since those parameters would undermine the deterministic reasoning behavior the thinking budget relies on.

---

**Q39. Correct: C**

Context editing automatically clears stale tool_use/tool_result content once a threshold is reached, keeping the active context within budget, while the memory tool lets the agent persist important state (progress, decisions, file state) outside the context window so it survives clearing or resets — together this is the standard pattern for long-running agentic loops.

---

**Q40. Correct: C**

Prompt caching is purpose-built for exactly this pattern: large, identical, reused context (system prompt + tool definitions) that is static across many calls. A cache_control breakpoint after that static block lets subsequent calls reuse the cached prefix at significantly reduced cost and latency rather than reprocessing 50K tokens every time.

---

**Q41. Correct: C**

Forcing tool_choice to a specific tool whose input_schema mirrors the desired output guarantees the model emits a structured tool_use block matching that schema, with no freeform preamble — this is the reliable, API-enforced mechanism for structured output, rather than relying on the model's adherence to natural-language instructions.

---

**Q42. Correct: D**

Interleaved thinking lets Claude emit thinking blocks in between sequential tool calls within a single turn, so it can reason about the result of one tool call before deciding whether/how to invoke the next — this capability must be explicitly enabled via a beta header, distinguishing it from standard extended thinking, which reasons once before the full response.

---

**Q43. Correct: D**

Claude Code's memory system loads CLAUDE.md recursively upward from the working directory to the filesystem root at launch, and additionally discovers CLAUDE.md files nested in subtrees, loading them when Claude actually reads files in that subtree — giving exactly the layered, non-duplicated behavior the scenario requires.

---

**Q44. Correct: D**

Project scope stores MCP server definitions in a .mcp.json file at the repo root, which is intended to be checked into version control so the entire team automatically gets the same server configuration when they pull the repo.

---

**Q45. Correct: A**

Agent Skills are explicitly designed around progressive disclosure — only compact metadata is loaded upfront for discovery, and the full skill content and supporting files are loaded on demand only when relevant, which is what allows large catalogs to scale without bloating context.

---

