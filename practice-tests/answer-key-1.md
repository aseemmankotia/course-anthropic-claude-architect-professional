# Answer Key — Practice Test 1

**Q1. Correct: D**

On-behalf-of delegation preserves the end user's existing permission scope in the downstream CRM, giving per-user least privilege without the MCP server needing elevated standing access.

---

**Q2. Correct: C**

Claude does not speak raw database protocols; the correct pattern is an adapter service (commonly an MCP server) that performs the protocol translation and enforces scope, exposing only the approved, parameterized operations as tools.

---

**Q3. Correct: D**

Cross-document synthesis requires enough contiguous context per retrieved unit and enough retrieved units to cover related policies; document-level chunks with overlap plus a higher top-k retrieval count leverage Claude's large context window effectively.

---

**Q4. Correct: A**

Since the source system cannot emit webhooks and there is no real-time requirement, a scheduled poller combined with asynchronous batch processing is the correct, resource-efficient pattern for this legacy constraint.

---

**Q5. Correct: A**

This correctly describes the core architectural trade-off: blocking request/response versus decoupled submission-and-retrieval, which is the deciding factor when downstream latency is unpredictable or long-running.

---

**Q6. Correct: B**

An adapter/MCP server is the correct pattern for legacy protocol translation: it isolates Claude from protocol-specific details, handles XML-to-JSON transformation, and provides a secure, auditable execution boundary.

---

**Q7. Correct: D**

The MCP authorization model supports OAuth 2.1-based flows for remote MCP servers specifically so that end-user identity propagates to the backend system, letting the HR system itself enforce row-level authorization rather than trusting the model or a prompt to restrict access.

---

**Q8. Correct: C**

The Message Batches API is purpose-built for high-volume, non-latency-sensitive workloads: it accepts large batches (up to 100,000 requests per batch), processes them asynchronously with results available within 24 hours, and bills at roughly 50% of standard API pricing — directly matching the cost and latency requirements here.

---

**Q9. Correct: B**

MCP tools should present Claude with a clean, well-defined JSON schema for inputs and outputs; an adapter layer that handles SOAP/XML translation, legacy session/auth management, and error mapping keeps the legacy protocol complexity out of the model-facing interface, which is the standard enterprise pattern for wrapping legacy systems behind MCP.

---

**Q10. Correct: D**

When the sequence and number of tool calls cannot be known ahead of time and depends on intermediate results, Anthropic's guidance recommends an agent loop where the model autonomously plans, calls tools, observes results, and decides the next action until the task is complete.

---

**Q11. Correct: A**

High-volume, latency-sensitive, low-complexity classification is the canonical use case for the fastest, cheapest model tier; Haiku-class models meet the 'good enough' quality bar at the throughput and cost this workload demands.

---

**Q12. Correct: A**

Prompt caching is designed exactly for this scenario: a large, static prefix (system prompt, tool definitions) reused across many calls. Caching it avoids reprocessing those tokens on every request, cutting both latency and cost for the cached portion.

---

**Q13. Correct: D**

The standard resilience pattern for transient capacity errors like 529 overloaded_error is exponential backoff with jitter combined with client-side request queuing/throttling, retrying only on codes documented as retryable, which smooths bursts and improves success rate without hammering the API.

---

**Q14. Correct: B**

The Message Batches API is purpose-built for large-volume, non-latency-sensitive workloads, offering substantial cost savings over standard API calls and asynchronous processing within a defined SLA window — exactly matching an overnight bulk-summarization use case.

---

**Q15. Correct: C**

Anthropic's architectural framework defines workflows as orchestrated systems following predefined code paths (e.g., prompt chaining, routing), whereas agents dynamically plan and control their own process, deciding which tools to use and when, based on environment feedback.

---

**Q16. Correct: C**

Moving bulk, non-urgent work to the Batch API decouples it from real-time rate limits entirely, while exponential backoff with jitter is the documented pattern for handling 429s on any remaining synchronous traffic — this reduces pressure on the rate limit ceiling instead of just working around symptoms.

---

**Q17. Correct: B**

When subtasks are predictable, sequential, and well-defined, Anthropic's agent design guidance recommends a prompt-chaining workflow: each step is a discrete, validated Claude call, which is more reliable, cheaper, and easier to debug than a fully agentic approach — a strong fit for a compliance-sensitive, deterministic process.

---

**Q18. Correct: B**

LLM-as-judge with a well-defined rubric enables scalable, automated, nuanced quality assessment that correlates well with human judgment on open-ended generative tasks, which exact-match or n-gram metrics cannot handle.

---

**Q19. Correct: C**

cache_control markers should be placed immediately after stable, reused content blocks (system prompt, document) so that identical prefixes hit the cache across requests, while the variable user question is excluded so it doesn't invalidate caching.

---

**Q20. Correct: D**

The Message Batches API is asynchronous, processes within 24 hours, applies a 50% discount versus standard pricing, and allows up to 100,000 requests (or 256MB) per batch.

---

**Q21. Correct: B**

A regression suite of golden traces capturing expected tool-call sequences and outcomes lets the architect systematically detect behavioral drift across many scenarios before the change reaches production.

---

**Q22. Correct: C**

Extended thinking tokens are billed as output tokens, so a larger budget_tokens value permits more reasoning steps while also increasing both the token cost and the overall response latency.

---

**Q23. Correct: D**

Structured, step-level trace capture with full tool I/O and reasoning context allows precise root-cause analysis and replay of specific failing scenarios, which is standard practice for debugging intermittent agent failures.

---

**Q24. Correct: C**

Prompt caching requires the cached segment to be an exact, unchanged prefix of the prompt. Placing the large static knowledge base first and the variable user question last lets Claude reuse the cached prefix across requests, charging cache-read tokens at a fraction of the base input price (and avoiding re-processing 15,000 tokens every call), which directly cuts cost and latency while preserving the full context for quality.

---

**Q25. Correct: C**

HIPAA requires a signed BAA with any vendor (business associate) that creates, receives, maintains, or transmits PHI on behalf of a covered entity. Anthropic offers BAAs to qualifying enterprise accounts, and this legal agreement is a prerequisite, not a technical setting.

---

**Q26. Correct: D**

Amazon Bedrock and Google Vertex AI both offer regional deployments of Claude that keep inference processing and associated data within the selected region, which is the architecturally supported way to meet EU data-residency requirements.

---

**Q27. Correct: D**

Least-privilege MCP/tool design means exposing narrow, purpose-built tools whose scope and permissions are limited to exactly what the agent task requires, minimizing blast radius if the model is manipulated or misbehaves.

---

**Q28. Correct: A**

Establishing a clear instruction hierarchy through structured delimiters plus an explicit system-level rule that untrusted content is data (not commands) is the documented, architecturally recommended mitigation for injection from retrieved content.

---

**Q29. Correct: A**

The ASL framework in Anthropic's RSP ties specific required safeguards — including security hardening, deployment restrictions, and evaluation protocols — to a model's assessed capability and risk profile, escalating protections as risk increases.

---

**Q30. Correct: B**

Because the Claude API is stateless and does not act as the customer's system of record for conversation content, architects must build their own logging/observability layer that captures full request/response payloads and writes them to a tamper-evident, access-controlled store to meet regulatory audit requirements.

---

**Q31. Correct: B**

Dated model snapshots are immutable, so pinning to one guarantees the model behavior never changes underneath the application. Upgrades then become a deliberate, tested migration event rather than a silent runtime change — this is the core lifecycle-management practice for production systems using Claude.

---

**Q32. Correct: D**

A core architect responsibility is defending model-selection decisions in terms stakeholders care about: cost, latency, and accuracy tradeoffs. Tiered routing by task complexity is the standard cost/performance optimization pattern and gives a concrete, defensible rationale.

---

**Q33. Correct: C**

Anthropic maintains official model deprecation documentation listing deprecation and retirement timelines for each dated snapshot; this is the authoritative source an architect must use for roadmap and migration planning.

---

**Q34. Correct: B**

Model migrations can shift output behavior, formatting, and reasoning quality even between closely related versions. Running the existing eval suite against the candidate version and validating it meets acceptance criteria before cutover is the standard, low-risk change-management practice.

---

**Q35. Correct: C**

Anthropic's lifecycle policy distinguishes a deprecation date, after which a model is discouraged for new development but still functional, from a later retirement date, after which the model is fully removed and calls fail — this distinction is essential for accurate roadmap planning.

---

**Q36. Correct: A**

The architectural rationale for pinning is reproducibility and controlled change: it isolates the production system from unannounced behavioral shifts and makes model upgrades an explicit, testable event — a key point an architect must be able to articulate to technical stakeholders.

---

**Q37. Correct: A**

Haiku is purpose-built for high-volume, latency-sensitive, lower-complexity tasks such as ticket classification, delivering the best cost/latency profile for this use case.

---

**Q38. Correct: B**

Complex, long-horizon agentic tasks involving architectural reasoning, planning, and recovery from failures are the canonical use case for Opus, the highest-capability model in the lineup.

---

**Q39. Correct: A**

Extended thinking is configured via a thinking parameter object whose budget_tokens field sets the maximum number of tokens the model can allocate to internal reasoning before generating the response.

---

**Q40. Correct: B**

Context compaction — clearing or summarizing stale tool outputs while retaining essential instructions and recent context — is the architecturally correct way to manage long-running agent sessions without losing critical state.

---

**Q41. Correct: A**

Compaction manages the existing context window by summarizing or discarding less-relevant state, while retrieval pulls new, relevant information from an external store (e.g., a vector database or memory tool) into context when needed; they solve complementary but distinct problems.

---

**Q42. Correct: A**

Explicitly setting tool_choice to the tool type with the specific tool name forces Claude to invoke exactly that tool, which is the reliable mechanism for guaranteeing structured output via a named tool call.

---

**Q43. Correct: A**

A repository-root CLAUDE.md, committed to version control, is automatically loaded by Claude Code for anyone working in that repo, giving every engineer consistent, up-to-date project context with zero repeated effort and a single source of truth that evolves with the codebase.

---

**Q44. Correct: C**

An internal MCP server catalog provides a standardized protocol for exposing tools and data sources to Claude Code, with centralized vetting, discoverability, and version control, which is the intended pattern for scaling safe tool access across an organization.

---

**Q45. Correct: D**

CLAUDE.md files are hierarchical: Claude Code combines context from root and nested files, with the more specific (closer to the working directory) file's instructions taking precedence when there's a conflict, while non-conflicting root-level context still applies.

---

