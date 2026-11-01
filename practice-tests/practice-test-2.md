# Practice Test 2: CCAR-P: Claude Certified Architect Professional Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-2.md`. Track results in `score-tracker.md`.

---

**Q1.** A healthcare company wants Claude Desktop users to query patient records through an MCP server connected to their EHR system. Different clinicians have different access levels based on role (nurse vs. physician vs. billing staff), and every access to PHI must be individually auditable. Which authentication/authorization pattern should the architecture use?

A) Have the MCP server validate the caller's identity via an OAuth token passed through from the client, then enforce role-based access control server-side before querying the EHR
B) Embed a single shared API key in the MCP server configuration that all users share when connecting to the EHR
C) Include the user's role in Claude's system prompt and instruct Claude to filter out data the role shouldn't see
D) Deploy a separate MCP server instance per clinician role with permissions hardcoded into each server's connection string

---

**Q2.** A Claude-powered agent processes incoming webhook events from a payment processor to update order status in an ERP system via an MCP tool. The payment processor's webhook delivery is at-least-once, so duplicate events are expected during network retries. How should the integration be designed to prevent duplicate ERP updates?

A) Use the event's unique ID as an idempotency key, persist processed IDs, and skip invoking the ERP update tool for already-seen event IDs
B) Have the agent compare each event's timestamp to the last processed timestamp and skip older events
C) Rely on Claude's context window across the conversation to recognize and skip content it has already seen
D) Configure the payment processor's webhook to guarantee exactly-once delivery so duplicates never occur

---

**Q3.** An enterprise's core order-management system only exposes a SOAP/XML interface over a mainframe, and security policy prohibits opening any new network path directly to the mainframe for AI tooling. The architect needs Claude to read and update order records via MCP. What is the best integration approach?

A) Have Claude generate raw SOAP XML envelopes and parse the mainframe's XML responses directly as text, without any intermediary layer
B) Build the MCP server with a SOAP client that connects directly to the mainframe over a new dedicated VPN tunnel approved on a case-by-case basis
C) Introduce a middleware/integration layer that exposes a modern REST/JSON API over the existing SOAP interface, and build the MCP server against that middleware instead of the mainframe directly
D) Abandon real-time integration and instead have a human export data to CSV periodically for Claude to reference

---

**Q4.** A retrieval pipeline must support both exact lookups of high-cardinality part numbers (millions of unique SKUs) and semantic search over free-text part descriptions, surfaced to Claude via a retrieval tool. Which retrieval architecture best satisfies both requirements?

A) Embed the entire product catalog as large semantic chunks and rely solely on vector similarity search for all queries, including part number lookups
B) Pass the full product catalog as context in every prompt so Claude can search it directly
C) Embed each part number individually as its own vector and rely on vector similarity to match exact SKU queries
D) Use a hybrid retrieval design: an exact-match index (e.g., key-value or SQL lookup) for identifiers like part numbers, combined with vector search for descriptions, exposed to Claude through a single retrieval tool that routes the query appropriately

---

**Q5.** A nightly batch job needs to process 500,000 documents through the Claude API for classification, writing results into downstream MCP-connected systems, without overwhelming rate limits or downstream services. Which design best handles this at enterprise scale?

A) Submit all 500,000 requests concurrently and rely on the client SDK's default retry logic to handle any failures
B) Use a queue-based worker pool with concurrency adjusted based on rate-limit response headers, combined with exponential backoff on 429 responses
C) Process documents sequentially one at a time with a fixed sleep interval between each request
D) Concatenate all 500,000 documents into a single large prompt sent as one request

---

**Q6.** An MCP server exposes both read (query) and destructive (delete) tools against a CRM system. The architect wants Claude to answer sales questions conversationally but must prevent any possibility of accidental data deletion triggered through conversation, even if a user asks Claude to delete something in a confusing or adversarial way. What is the best architectural control?

A) Do not expose any delete-capable tool in the MCP server used for the sales conversational interface at all; provide a separate, access-controlled MCP server with delete tools only for authorized admin workflows
B) Grant the MCP server's service account full CRUD permissions, and instruct Claude via the system prompt to never call the delete tool unless explicitly asked
C) Expose all tools including delete, but require the user to type a confirmation phrase like "CONFIRM DELETE" in chat before Claude will invoke the delete tool
D) Expose all tools as normal, but log every tool call so that any accidental deletions can be identified and reversed after the fact

---

**Q7.** An enterprise MCP server brokers access from Claude to multiple internal REST APIs. End users authenticate to Claude via SSO using SAML, but each downstream API requires an OAuth 2.0 access token scoped to the calling user's department, and all calls must be individually auditable per user. Which approach BEST meets least-privilege and auditability requirements for the MCP server's calls to downstream APIs?

A) Use a single shared OAuth client-credentials token for all MCP tool calls, and include the end user's chat session ID in request headers for audit logging
B) Authenticate the MCP server once at startup using client credentials, cache the resulting token for the server's lifetime, and reuse it for all subsequent users
C) Implement OAuth 2.0 token exchange (RFC 8693) so the MCP server exchanges the user's SAML-derived identity assertion for a downstream-scoped access token on each request, preserving the original user's identity and department scope
D) Have end users paste their personal API keys into the Claude conversation so the MCP server can forward them directly to each downstream API

---

**Q8.** Claude's orchestration layer invokes a tool that publishes a message to an SQS standard (at-least-once delivery) queue, which a legacy batch job consumes to decrement warehouse inventory. The legacy system has no concept of conditional updates. Claude's orchestration layer automatically retries tool calls on timeout, so the same inventory-decrement message can be delivered more than once. Which approach BEST prevents duplicate inventory decrements in this architecture?

A) Increase Claude's tool-call timeout threshold so retries happen less often
B) Generate a unique idempotency key per tool call (e.g., derived from the conversation turn ID) and have the legacy consumer check and persist processed keys before applying each inventory update, rejecting duplicates
C) Switch the queue to FIFO mode and rely on its ordering guarantee to prevent duplicate processing
D) Disable automatic retries in Claude's orchestration layer so duplicate messages are never produced

---

**Q9.** A legacy mainframe exports nightly customer account data as fixed-width COBOL copybook files, where each record contains a hierarchical structure: an account header, multiple transaction sub-records, and nested fee schedules. An architecture team must feed this into Claude's retrieval pipeline for a customer-support RAG application that needs to cite the specific account field used in each answer. Which ingestion strategy BEST preserves retrieval accuracy and citation fidelity given this legacy format?

A) Concatenate the entire nightly extract into one large text blob per day and embed it as a single chunk so Claude always has full context during retrieval
B) Skip chunking and pass the raw fixed-width file directly into Claude's context window at query time, letting the model parse the copybook format inline
C) Parse the copybook layout to map fixed-width fields into named, structured records, then chunk by logical unit (one account plus its transactions) with field-level metadata, embedding each chunk separately with citation pointers to field names
D) Convert the fixed-width data to CSV without field-name mapping and chunk by fixed line-count windows (e.g., every 500 lines) regardless of record boundaries

---

**Q10.** A global enterprise processes 50,000 customer support tickets per day. Most tickets are simple account or billing questions, a minority require multi-step investigation across internal systems, and a small fraction are ambiguous and need a human. The company wants to minimize cost per ticket while keeping response quality high at this scale. Which architecture BEST meets these requirements?

A) Tiered workflow: Haiku classifies intent and routes simple queries to Haiku for response generation, complex queries to Sonnet with tool use, and escalates ambiguous cases to a human agent
B) Single large multi-agent system with Opus orchestrating multiple Opus sub-agents for every ticket, regardless of complexity
C) One large monolithic prompt with all ticket context sent to Opus for every ticket
D) A multi-agent swarm where each incoming ticket spawns five autonomous agents to independently research and vote on a response

---

**Q11.** A financial services client needs a real-time trading dashboard chatbot that answers simple factual lookups with sub-second response times. Which model and API configuration is BEST suited to this requirement?

A) Claude Opus with extended thinking enabled and streaming disabled
B) Claude Haiku with streaming enabled and extended thinking disabled
C) Claude Sonnet with extended thinking enabled and the entire knowledge base re-sent via prompt caching on every request
D) Claude Opus via the Batch API

---

**Q12.** A RAG pipeline sends a static 20,000-token system prompt containing company policies on every request, and the service handles thousands of requests per hour. Which configuration BEST reduces cost and latency per request without losing information?

A) Send the full 20k-token system prompt as plain text on every request and rely on model speed to minimize impact
B) Add a separate summarization API call before each request to compress policies down to 500 tokens
C) Switch to a smaller model with no caching to lower the per-token price
D) Apply prompt caching (cache_control) to the static system prompt block so repeated requests reuse the cached prefix instead of reprocessing it

---

**Q13.** In Anthropic's documented agentic workflow patterns, what is the KEY distinction between the orchestrator-workers pattern and the evaluator-optimizer pattern?

A) Orchestrator-workers is limited to single-turn tasks; evaluator-optimizer is limited to multi-turn tasks
B) Orchestrator-workers uses a central LLM to dynamically decompose a task and delegate subtasks to worker LLMs, while evaluator-optimizer uses a loop where one LLM generates a response and a second LLM evaluates and provides feedback for iterative refinement
C) Orchestrator-workers requires separate API keys per worker; evaluator-optimizer requires only one API key total
D) Orchestrator-workers always pairs Opus with Haiku workers; evaluator-optimizer always uses only Sonnet for both roles

---

**Q14.** During peak enterprise traffic, an application begins receiving intermittent 529 'Overloaded' errors from the Claude API. What is the BEST resilience pattern to implement in response?

A) Immediately return an error to the end user without retrying
B) Permanently route all traffic to the Batch API instead of the standard Messages API
C) Implement exponential backoff with jitter and automatic retries, falling back to an alternate model tier or region if retries are exhausted
D) Increase the max_tokens parameter on each request until the call succeeds

---

**Q15.** A legal team wants an assistant that researches case law across several internal databases, decides which sources to query, synthesizes findings over an unknown and variable number of steps, and produces a final memo. The number of steps and sources needed differs significantly case by case. Which architecture BEST fits this requirement?

A) An autonomous agent with access to database query tools that plans its own research steps, executes tool calls, and iterates until it determines the investigation is complete
B) A fixed sequential workflow chaining four predetermined prompts in a set order
C) A single prompt with the entire case law database inserted directly into the context window
D) A prompt-routing pattern that classifies the incoming query and forwards it to one of three fixed prompt templates

---

**Q16.** A financial services firm processes 500,000 pages/day through Claude for compliance document analysis. The system must survive regional outages, must not fail outright when hitting rate limits during peak load, and must control token costs since the same system prompt and few-shot examples are reused across nearly every request. Which architecture BEST satisfies all these requirements?

A) Deploy in a single region with a single API key, and retry every failed request after a fixed 1-second delay regardless of the error type
B) Deploy across multiple regions/endpoints, implement exponential backoff with jitter on 429/529 responses, use prompt caching for the repeated system prompt and few-shot examples, and route non-urgent high-volume analysis through the Message Batches API
C) Always route every request to the most capable model available, run in a single region, and skip caching to keep implementation simple
D) Implement client-side request throttling only, with no retry logic, and assume the rate limit ceiling will never be reached in production

---

**Q17.** An enterprise needs to generate an auditable research report synthesizing findings from 50 distinct internal data sources, each requiring independent investigation before synthesis. The team needs parallelism to control end-to-end latency, per-source traceability for audit purposes, and avoidance of context-window bloat from cramming all 50 sources into one conversation. Which solution design is the correct fit?

A) Use an orchestrator-worker multi-agent pattern: a lead agent decomposes the task into source-specific subtasks, spawns parallel subagents each with their own context window to investigate individual sources, and the lead agent synthesizes their findings into the final report
B) Concatenate all 50 sources into a single long prompt and have one agent produce the report in a single call
C) Run a single agent sequentially through the 50 sources one at a time in a single long-running conversation thread
D) Spin up multiple independent agents that each receive the identical full task and prompt, then select the final report by majority vote among their outputs

---

**Q18.** An architect built an LLM-as-judge pipeline that performs pairwise comparisons between Model A's and Model B's outputs for a summarization task. Analysis shows the judge picks whichever response appears first in the prompt roughly 80% of the time, regardless of actual quality. What is the BEST way to correct this positional bias?

A) Increase the temperature setting of the judge model to reduce bias
B) Run each comparison twice with the response order swapped and reconcile the two judgments
C) Add chain-of-thought instructions asking the judge to justify its choice before answering
D) Replace pairwise comparison with independent 1-10 scoring of each response

---

**Q19.** A company runs 10,000 daily support-ticket classification requests. Each request includes an identical 3,000-token system prompt (containing instructions and examples) plus a unique 150-token ticket description. Results feed a dashboard refreshed once per day, so real-time responses are not required. Which approach BEST minimizes API cost?

A) Enable prompt caching for the system prompt only
B) Reduce the system prompt to under 1024 tokens so caching is not needed
C) Submit all requests through the Message Batches API without caching
D) Enable prompt caching for the system prompt AND submit requests through the Message Batches API

---

**Q20.** A developer applies cache_control to a 900-token system prompt when calling Claude Sonnet. On the next identical request, the billing breakdown shows no cache-read tokens, only standard input tokens. What is the MOST likely cause?

A) The cache TTL expired after 5 minutes before the second request
B) cache_control can only be applied to user messages, not system prompts
C) The prompt is below the minimum cacheable length (1024 tokens for Sonnet)
D) The account has not enabled the prompt caching beta feature

---

**Q21.** An architect maintains a regression suite for an agent that calls a search tool, a calculator tool, and a database tool. After upgrading the underlying model, the suite's final-answer exact-match accuracy stays unchanged, but production users report the agent now requires many more tool calls to reach the same answers, increasing cost and latency. What is the regression suite MOST likely missing?

A) The golden set lacks diversity across difficulty levels
B) The test suite sample size is too small to detect the regression
C) The suite lacks LLM-as-judge scoring of final answer quality
D) The regression suite only asserts on final-answer exact match, not on tool-call trajectory efficiency

---

**Q22.** A data science team needs to run sentiment classification over 500,000 archived customer transcripts overnight, with results required by 9 AM the next morning. Minimizing API cost is the top priority, and no real-time response is needed. Which approach should the architecture use?

A) Standard Messages API with concurrent requests and prompt caching
B) Streaming API using server-sent events
C) Claude with extended thinking enabled
D) The Message Batches API

---

**Q23.** In an observability platform tracking Claude-based agent traces, which metric MOST directly signals that a model or prompt change has degraded the agent's planning efficiency, even when final-answer accuracy remains unchanged?

A) Number of tool-call round trips per completed task
B) Total tokens consumed per conversation turn
C) Average response latency per individual API call
D) Percentage of requests returning stop_reason: end_turn

---

**Q24.** A team runs a nightly regression suite of 500 golden-set examples against an agent that uses a 22,000-token system prompt and a fixed set of tool definitions on every call. Each example is also scored by an LLM-as-judge using a detailed rubric. The suite currently costs more per night than the team's entire production traffic because every regression call is synchronous and re-sends the full system prompt and tool schema every time. Results are only needed by the next morning. Which change BEST reduces cost and latency for this regression workflow without degrading evaluation fidelity?

A) Replace the LLM-as-judge rubric scoring with a smaller, faster model for every regression run to cut token costs, while keeping all 500 calls synchronous so results are available immediately
B) Enable prompt caching on the static system prompt and tool definitions (well above the 1024-token minimum) so repeated calls hit the cache, and submit the 500 regression calls plus judge-scoring calls through the Batch API since same-day turnaround is acceptable
C) Drop the golden-set LLM-as-judge evaluation entirely and rely only on deterministic string-match unit tests, since removing rubric scoring removes the largest cost driver in the suite
D) Raise the sampling temperature on the agent's synchronous calls to shorten its reasoning and reduce output tokens, keeping the existing synchronous, uncached regression pipeline unchanged

---

**Q25.** A financial services firm is building an agent that uses MCP servers to query customer accounts and execute trades. They must minimize the risk that a prompt injected into a tool's response (e.g., malicious text embedded in a market-data feed) causes an unauthorized trade. Which design BEST mitigates this risk while keeping the agent functional?

A) Rely on Claude's constitutional training alone to refuse malicious instructions embedded in tool outputs
B) Disable tool use entirely and have the agent only summarize account data
C) Grant the agent a single MCP server with full read/write access to simplify the architecture
D) Require explicit human-in-the-loop confirmation for any state-changing tool call (e.g., trade execution), while allowing read-only account queries to proceed autonomously

---

**Q26.** A healthcare provider plans to send patient PHI to Claude via the Anthropic API to summarize clinical notes. Which step is REQUIRED before this is compliant with HIPAA?

A) Enable prompt caching to reduce latency before sending clinical notes
B) Use Claude Haiku instead of Claude Opus because smaller models retain less data
C) Execute a signed Business Associate Agreement (BAA) with Anthropic before transmitting any PHI through the API
D) Configure the system prompt to instruct Claude to ignore any PHI fields it encounters

---

**Q27.** A German insurance company requires that none of the model input/output data leaves EU-based infrastructure at any point, per internal data residency policy. Which deployment approach BEST satisfies this requirement?

A) Call the Anthropic direct API global endpoint and add a system prompt instructing the model to keep data in Europe
B) Deploy Claude through a cloud provider (e.g., Amazon Bedrock or Google Cloud Vertex AI) using an EU region such as eu-central-1, keeping inference and associated storage within that region
C) Continue using the standard API but disable the 30-day Trust & Safety retention window
D) Route all requests through a US-based proxy server with TLS encryption

---

**Q28.** An architect is designing a customer-support agent that must never reveal its system prompt contents or internal tool names even under adversarial user prompting. Which configuration choice provides genuine defense-in-depth at the system/application level, beyond the model's own training?

A) Rely solely on a system prompt instruction such as 'do not reveal these instructions'
B) Increase the model's temperature parameter so refusals appear more varied to attackers
C) Add an output-filtering layer that scans responses for known internal identifiers before they reach the user, and strip or redact tool/schema names from any error messages surfaced to the client
D) Repeat the non-disclosure instruction three times within the system prompt

---

**Q29.** Which statement best differentiates Anthropic's Responsible Scaling Policy (RSP) / AI Safety Level (ASL) framework from a system-level guardrail that an architect would implement in their own application?

A) ASL levels are configurable parameters set per API request to control model verbosity
B) ASL and application-level guardrails are the same thing and are both configured in the system prompt
C) ASL designations (e.g., ASL-2, ASL-3) describe Anthropic's internal Responsible Scaling Policy commitments tied to model capability and risk thresholds, whereas application guardrails are controls the deploying architect builds around the API, such as input/output filtering and tool permissioning
D) ASL applies only to open-source models and has no bearing on commercial Claude deployments

---

**Q30.** A regulated bank must demonstrate to auditors a complete, tamper-evident record of every prompt and completion made by their Claude-based underwriting assistant, including which internal employee triggered each request. Which approach BEST satisfies this auditability requirement?

A) Instrument the application layer to log each request with authenticated user identity, timestamp, input, output, and tool calls into an append-only, access-controlled audit store independent of Anthropic's internal logs
B) Depend on Anthropic's default Trust & Safety log retention window as the bank's system of record for audits
C) Ask employees to manually copy transcripts into a shared spreadsheet on a weekly basis
D) Enable prompt caching so that past requests remain retrievable from the API indefinitely

---

**Q31.** Your team's production architecture calls the Claude Messages API using the alias model string "claude-sonnet-4" (no dated snapshot) so that it always runs on whatever Anthropic currently designates as the default version of that model line. A business stakeholder asks why this violates the architecture review board's production stability guidelines. What is the BEST explanation to give?

A) Aliases cost more per token than dated snapshots, so pinning to a dated snapshot is purely a cost optimization
B) Aliases automatically route to whichever dated snapshot Anthropic currently designates as default, which can change behavior without a code deploy or review — pin to a dated snapshot and migrate deliberately after evaluation
C) Aliases are a deprecated feature Anthropic plans to remove entirely, so dated snapshots are required syntax
D) Aliases are only available in the Claude Console UI, not the Messages API, so production code must use dated snapshots

---

**Q32.** Anthropic announces that claude-3-opus-20240229 will be retired in 6 months, with claude-opus-4-20250514 recommended as the replacement. What is the BEST migration strategy for a production architecture team?

A) Immediately replace the model string in production without additional testing, since Anthropic recommends the replacement model
B) Wait until the official retirement date, then switch in an emergency deployment to avoid wasting QA effort early
C) Take no action, since legacy model endpoints remain available indefinitely for backward compatibility
D) Build a regression evaluation suite comparing output quality, latency, and cost between the current and candidate model on representative production prompts, validate in staging, then roll out gradually with monitoring well before the deadline

---

**Q33.** Which of the following is the authoritative source your architecture team should monitor to track Anthropic's official model deprecation and retirement dates?

A) Third-party community-maintained spreadsheets tracking Claude releases
B) Anthropic's documented model deprecation page and the deprecation notifications sent to Console account admins
C) Asking Claude directly inside a chat session for its own end-of-life date
D) Anthropic's social media announcements as the sole tracking mechanism

---

**Q34.** Your production system must guarantee that the exact same Claude model version answers every request throughout a regulatory compliance audit period. Which configuration approach is correct?

A) Set a system prompt instructing Claude to "always behave as the original model version"
B) Set the `model` parameter to the dated snapshot string (e.g., "claude-3-5-sonnet-20241022") and avoid alias strings entirely for the audited environment
C) Pin the `anthropic-version` HTTP header to a fixed date, which fixes the underlying model weights
D) Set `temperature` to 0, which locks the API to a specific model snapshot

---

**Q35.** In Anthropic's model lifecycle terminology, what is the key difference between a model being "deprecated" versus "retired"?

A) Deprecated models remain callable and are flagged with an announced retirement date during a notice period; retired models are no longer callable via the API
B) Deprecated models are removed from the API immediately; retired models remain callable for a grace period afterward
C) Deprecated and retired are interchangeable terms with no functional difference in Anthropic's lifecycle policy
D) Deprecated models can only be accessed through the Console, while retired models can only be accessed via the direct API

---

**Q36.** A compliance stakeholder asks why the architecture's deployment of Claude Opus 4 includes additional security review, restricted deployment environments, and enhanced usage monitoring compared to earlier Claude 3 models. What is the BEST explanation?

A) Claude Opus 4 was released under ASL-3 (AI Safety Level 3) of Anthropic's Responsible Scaling Policy, meaning safety evaluations indicated capability thresholds requiring enhanced security and deployment safeguards
B) Claude Opus 4 is an open-weight model, so additional licensing controls are legally required for any deployment
C) ASL-3 refers to a SOC 2 Type II compliance certification level that all enterprise customers must independently renew annually
D) The additional safeguards are a temporary beta restriction that will be removed once Opus 4 exits preview status

---

**Q37.** A company processes 2 million customer support tickets per day. 99% require simple intent classification into one of 10 categories with sub-second latency and minimal per-ticket cost. The remaining 1% are escalations requiring multi-step reasoning across policy documents. Which architecture BEST fits this scale?

A) Use Haiku for initial intent classification with a lightweight routing layer that escalates only the flagged ~1% of tickets to Opus or Sonnet for deeper reasoning
B) Route every ticket through Opus to guarantee consistent reasoning quality
C) Use Sonnet for all tickets since it is a reasonable middle ground on cost and capability
D) Route every ticket through Opus but add prompt caching to offset the cost

---

**Q38.** You are implementing extended thinking via the Messages API for a complex multi-step agentic task that also issues tool calls. Which statement about the required configuration is accurate?

A) budget_tokens has no minimum, but the combined total with max_tokens is capped at 4096 tokens
B) Extended thinking is incompatible with tool use and must be disabled before any tool call is issued
C) Thinking tokens are generated for free and do not count toward max_tokens or billed output
D) budget_tokens must be set to at least 1024 tokens, and while extended thinking is enabled you cannot modify temperature, top_p, or top_k

---

**Q39.** An agentic coding assistant runs long tool-use loops (file reads, searches, edits) that accumulate large tool results, risking context window overflow after roughly 50 turns mid-task. Which approach best manages this at production scale?

A) Reduce max_tokens on each response to force shorter model outputs
B) Switch the agent to Haiku, assuming it provides a larger context window than Sonnet or Opus
C) Enable context editing so older tool results are automatically cleared/truncated as the context grows, combined with the memory tool to persist critical facts outside the context window
D) Have a developer manually restart the conversation every 10 turns, pasting in a handwritten summary

---

**Q40.** A production pipeline sends thousands of short requests per hour that all share an identical 50K-token system prompt and tool definition block, with only a small user message varying per call. Which technique BEST reduces cost and latency for this specific pattern?

A) Re-issue a retrieval (RAG) lookup for the system prompt content on every call
B) Store the system prompt using the memory tool so Claude retrieves it from persistent storage
C) Use prompt caching with a cache_control breakpoint placed after the static system prompt and tool definitions
D) Summarize the system prompt with Haiku before each call to shrink the token count

---

**Q41.** An automated pipeline requires Claude's output to strictly conform to a specific JSON schema with zero freeform preamble text, feeding directly into a downstream parser. Which configuration correctly enforces this in production?

A) Rely entirely on prompt instructions such as 'Always respond only in valid JSON'
B) Set a stop_sequence of a closing brace character to truncate the response at the end of JSON
C) Define a tool whose input_schema matches the desired output structure, set tool_choice to force that specific tool, and parse the resulting tool_use input block
D) Enable extended thinking so Claude can self-validate the JSON structure before responding

---

**Q42.** During a multi-step agentic tool-use turn, what does 'interleaved thinking' refer to in Claude's extended thinking feature?

A) Alternating requests between Opus and Sonnet on successive turns to balance cost and capability
B) Splitting the context window into alternating segments of cached and uncached content to optimize cost
C) Running two independent reasoning passes — a chain-of-thought pass and a self-critique pass — and merging them into a final answer
D) Claude producing thinking blocks between individual tool calls within the same turn, allowing it to reason about each tool result before deciding the next action, enabled via a specific API beta header

---

**Q43.** A monorepo serves three sub-teams (payments, auth, frontend). Leadership wants one root CLAUDE.md with company-wide conventions, plus each sub-team's own additional conventions that apply only when an engineer is working inside that team's directory — without duplicating the root content in every sub-team file. Which approach correctly uses Claude Code's memory file system to achieve this?

A) Place a CLAUDE.local.md in each sub-team directory, since local memory files are designed to automatically merge across the directory tree the same way project-level CLAUDE.md files do
B) Concatenate everything into a single massive root CLAUDE.md with one section per team, since Claude Code only ever loads one CLAUDE.md file per session regardless of directory structure
C) Require each engineer to manually type @payments/CLAUDE.md at the start of every session, since Claude Code has no automatic discovery of memory files outside the root directory
D) Create a CLAUDE.md inside each sub-team directory (e.g., payments/CLAUDE.md); Claude Code discovers memory files recursively from the current working directory up to the root, and also pulls in nested subdirectory CLAUDE.md files as it reads files in those subtrees — so subdirectory files add to, rather than replace, the root file

---

**Q44.** A platform team wants to define an MCP server connecting to the internal ticketing system such that it is automatically available to every engineer the moment they clone the repository, is version-controlled, and can be reviewed via pull request alongside code changes. Which MCP configuration scope should they use?

A) User scope, by adding the server to the engineer's personal Claude configuration so it syncs across all of that engineer's projects
B) Local scope, using `claude mcp add` with no scope flag, since this stores the server in project-specific user settings for convenience
C) Enterprise managed settings, since team-wide MCP servers can only be distributed through an org-wide managed-settings file controlled by IT and cannot be scoped to a single repository
D) Project scope, by defining the server in a .mcp.json file at the repository root and committing it to version control

---

**Q45.** An internal Skills catalog contains 40 Agent Skills covering tools like deployments, database migrations, and incident response. Engineers notice the context window filling up even for tasks unrelated to most of the catalog. Which design mechanism should an architect rely on to prevent this, based on how Agent Skills are actually loaded?

A) Rely on progressive disclosure: Claude Code loads only each skill's lightweight metadata (name and description) at startup, and pulls in the full SKILL.md body and bundled resources only when the current task indicates that specific skill is relevant
B) Reduce the catalog to fewer than 10 skills, since Claude Code loads the full body of every SKILL.md into context at the start of every session regardless of task relevance
C) Convert every skill into an MCP tool instead, since Agent Skills are not designed to scale past a handful of catalog entries
D) Instruct engineers to manually reference the exact skill folder path in every prompt, since Claude Code has no automatic mechanism for discovering or selecting relevant skills

---

