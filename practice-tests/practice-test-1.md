# Practice Test 1: CCAR-P: Claude Certified Architect Professional Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** An internal MCP server lets Claude query a CRM system on behalf of signed-in employees. Different employees should only see records they are authorized to view in the CRM, and the security team has rejected any design using a single shared privileged credential. Which authentication approach BEST meets these requirements?

A) Embed one service-account token with admin privileges in the MCP server configuration and use it for every user's requests
B) Have Claude store each user's raw CRM password in the conversation context and pass it as an API key on each tool call
C) Disable authentication on the MCP server's CRM calls since the server is reachable only from inside the corporate VPN
D) Implement an OAuth 2.0 on-behalf-of flow so the MCP server forwards the authenticated user's own scoped access token to the CRM, which enforces user-level permissions

---

**Q2.** A legacy on-premises database only exposes a JDBC interface, with no REST, GraphQL, or SOAP endpoint available. Claude needs read-only access to a few specific views in this database for an internal reporting agent. Which integration approach is architecturally correct?

A) Give Claude a JDBC driver and connection string directly so it can issue raw SQL over the wire itself
B) Export the entire database nightly as CSV files to a public, internet-facing storage bucket so Claude can read them without authentication
C) Build a custom MCP server that wraps the JDBC connectivity internally and exposes narrowly scoped tools (e.g., specific parameterized queries against approved views) over the MCP protocol
D) Block the integration project until the database vendor ships a native REST API for the legacy system

---

**Q3.** A compliance Q&A system retrieves internal policy documents (each roughly 2 pages) for Claude to answer questions. Many questions require synthesizing information across 3-5 related policies. Given Claude's large context window, which retrieval configuration BEST supports accurate, well-grounded answers?

A) Use very small 100-token chunks with no overlap to maximize retrieval precision, and retrieve only the top 3 chunks per query
B) Store the entire policy corpus as a single combined chunk and always pass the full corpus regardless of the query
C) Chunk strictly by sentence boundaries and retrieve exactly 1 chunk per query to minimize token usage
D) Use document-level chunking (roughly whole-document chunks) with modest semantic overlap, and retrieve the top 8-10 chunks, letting Claude's context window synthesize across them

---

**Q4.** A legacy mainframe batch job drops output files to an SFTP server nightly at 2 AM. The mainframe cannot be modified to emit webhooks or push events. A Claude-powered agent needs to process exception records in these files and draft remediation emails, with no real-time latency requirement. Which integration pattern is MOST appropriate?

A) Implement a scheduled file-watcher/poller that detects new files on the SFTP server after batch completion and triggers a Claude batch processing job to handle the exception records asynchronously
B) Modify the mainframe to push a webhook event the moment the batch job starts
C) Have Claude synchronously poll the SFTP server via API calls every second around the clock for the fastest possible pickup
D) Have Claude connect directly to the mainframe using real-time terminal screen-scraping during business hours

---

**Q5.** When connecting Claude to backend systems with highly variable or long response latency (for example, a legacy mainframe batch job that can take several minutes), what is the key architectural distinction between synchronous and asynchronous/batch integration patterns?

A) In synchronous patterns, the caller blocks waiting for a response within the request lifecycle; asynchronous/batch patterns decouple submission from result retrieval via callbacks, polling, or queues, which suits unpredictable or long downstream latency
B) Synchronous integrations must always use MCP, while asynchronous integrations can never use MCP
C) Asynchronous integration cannot be used with the Claude API because all Claude model calls are strictly synchronous
D) Synchronous integration is reserved for internal testing only, and production systems must always use asynchronous batch processing

---

**Q6.** A 20-year-old internal inventory system only exposes a SOAP/XML API with no modern alternative planned. Claude needs to retrieve inventory data as a tool-use capability in an agentic workflow. Which integration approach correctly handles this legacy constraint?

A) Have Claude itself natively construct and parse raw SOAP XML envelopes as part of its tool-calling schema
B) Build an adapter layer (such as an MCP server) that translates incoming tool calls into SOAP/XML requests, invokes the legacy API, and returns transformed JSON responses to Claude
C) Require the legacy system to be fully migrated to a modern REST API before any integration work can proceed
D) Have Claude generate raw SOAP XML text in its response and send it directly over the network without any backend execution service

---

**Q7.** A bank integrates Claude with an internal HR records system through an MCP server. Each employee using the Claude-based assistant must only be able to retrieve HR records they are personally authorized to view in the underlying system — not records accessible to a broad service account. Which integration design BEST satisfies this requirement?

A) Configure the MCP server with a single long-lived service account that has read access to all HR records, and instruct Claude via the system prompt to only discuss records relevant to the requesting employee
B) Have the MCP server always fetch the full employee dataset with an admin credential, then rely on Claude to filter the response text before showing it to the user
C) Embed each employee's permitted record IDs directly in the system prompt and have Claude self-restrict which records it asks the tool for
D) Configure the MCP server to require an OAuth 2.1 access token issued for the authenticated end user, and pass that user-scoped token through to the downstream HR system so authorization is enforced at the data layer, not by Claude

---

**Q8.** An enterprise needs to run Claude-based extraction over 500,000 scanned invoices overnight, with no requirement for real-time responses, and wants to minimize per-request cost. Which integration approach is MOST appropriate?

A) Call the standard synchronous Messages API in a tight loop across all 500,000 invoices, implementing custom retry logic for rate limit errors
B) Use the streaming Messages API so each invoice response begins returning tokens immediately, reducing total wall-clock time
C) Submit the invoices as jobs to the Message Batches API, splitting them across batches (up to 100,000 requests per batch), and retrieve results once processing completes within 24 hours at the batch discount rate
D) Build a custom job queue with exponential backoff against the standard Messages endpoint to throttle throughput manually

---

**Q9.** A legacy mainframe system exposes order data only through a SOAP/XML web service with no REST or JSON interface. The architecture team wants Claude to be able to look up order status through an MCP tool. What is the BEST configuration approach?

A) Have Claude construct raw SOAP XML envelopes directly inside its tool call arguments, sent straight to the mainframe's SOAP endpoint
B) Build an adapter/gateway service that translates between a well-defined MCP tool (with a JSON input/output schema) and the mainframe's SOAP/XML interface, hiding the legacy protocol from Claude entirely
C) Modify Claude's model weights so it natively understands and generates SOAP/XML payloads for this integration
D) Expose the mainframe's terminal screen output to Claude and have it parse the raw screen text within the system prompt on every call

---

**Q10.** A SaaS company is building an internal engineering assistant that answers questions about system architecture by searching docs, querying a metrics API, and occasionally writing a small script to analyze logs. Each request requires 2-4 sequential tool calls, and the exact sequence of tools needed cannot be predetermined because it depends on what earlier tool calls return. Which architecture pattern BEST fits this requirement?

A) A fixed prompt-chaining workflow where each step's tool call is hardcoded in advance
B) A single large prompt that includes all tool outputs pre-fetched before the model is called
C) A routing workflow that classifies the question and sends it to one of several hardcoded tool pipelines
D) An autonomous agent loop where Claude decides which tools to call and in what order based on intermediate results

---

**Q11.** A company must classify 2 million short support tickets per day into one of 12 categories, with a strict budget and a latency requirement under 500ms per ticket. Accuracy only needs to be 'good enough' to beat the existing keyword-based system. Which model choice is MOST appropriate?

A) A smaller, fast Haiku-tier model, since the task is simple classification at massive scale with strict latency and cost constraints
B) The largest available Opus-tier model for every ticket to maximize accuracy
C) A mid-tier Sonnet model with extended thinking enabled for every request
D) Route every ticket through Opus first, then re-run on Haiku to save cost

---

**Q12.** An enterprise RAG application sends the same 6,000-token system prompt (containing tool definitions and formatting instructions) on every single API call, with only the user's question changing. Which configuration change will MOST reduce both latency and cost at scale?

A) Enable prompt caching on the static system prompt so repeated calls reuse the cached prefix instead of reprocessing it
B) Move the system prompt into the user message instead
C) Reduce the system prompt to under 1,024 tokens regardless of content loss
D) Switch to a smaller model to compensate for the large system prompt

---

**Q13.** A production application making heavy use of the Claude API starts intermittently receiving overloaded_error (529) responses during peak traffic. The architecture currently fires requests directly from the application thread with no retry logic. Which resilience pattern should the architect implement?

A) Immediately switch all traffic to a different model family to avoid the errors entirely
B) Retry failed requests instantly in a tight loop until they succeed
C) Lower the max_tokens parameter on every request to reduce error rates
D) Implement exponential backoff with jitter and a request queue to smooth bursty traffic, retrying only on retryable error codes

---

**Q14.** A company needs to process 500,000 legal documents overnight for summarization, with results needed by 9am but no requirement for real-time responses. Which API pattern is MOST cost-effective and appropriate for this reference architecture?

A) Standard synchronous Messages API calls issued one at a time in a sequential loop
B) The Message Batches API, submitting the full job asynchronously for processing within the batch SLA window
C) Streaming responses via the Messages API with server-sent events for each document
D) A multi-agent system where separate agents negotiate which documents to prioritize

---

**Q15.** Per Anthropic's guidance on building effective agentic systems, what is the key architectural distinction between a 'workflow' and an 'agent'?

A) Workflows always use smaller models while agents always use the largest available model
B) Agents cannot use tools, while workflows are defined specifically by their use of external tool calls
C) Workflows are systems where LLMs and tools are orchestrated through predefined code paths, while agents are systems where the LLM dynamically directs its own process and tool usage
D) There is no meaningful distinction; the terms are interchangeable in production architecture

---

**Q16.** A company processes 500,000 customer support tickets per day through Claude API calls for classification and response drafting. During peak hours, the system experiences repeated 429 rate-limit errors that cause ticket processing to fail outright. Which architecture change BEST improves resilience without requiring a higher rate-limit tier?

A) Add more application servers running Claude API calls concurrently to increase overall throughput
B) Reduce the max_tokens parameter on each request so responses complete faster and free up rate-limit capacity sooner
C) Route non-time-critical ticket classification through the Batch API behind a queueing layer with retry logic, and apply exponential backoff with jitter for any remaining synchronous calls
D) Replace exponential backoff with immediate retries at a fixed 1-second interval, since 429 errors indicate only a temporary capacity issue

---

**Q17.** An enterprise needs a system for legal contract review: extract key clauses, check them against a fixed compliance checklist, and generate a structured summary flagging risks. The steps are well-defined, sequential, and do not require dynamic re-planning. Per Anthropic's guidance on choosing between workflows and agents, which architecture is MOST appropriate?

A) A single autonomous agent with broad tool access (web search, file read/write, email) that decides its own steps on each run
B) A deterministic prompt-chaining workflow where extraction, checklist comparison, and summarization are separate fixed Claude calls, with validated outputs passed between steps
C) A multi-agent orchestrator-worker system where an orchestrator dynamically spawns subagents per contract, each with independent planning loops
D) A single large-context call that includes the full contract, checklist, and output instructions, asking Claude to perform all three tasks and self-verify in one pass

---

**Q18.** A team built a customer support agent and needs to evaluate response quality at scale before each deployment. They need fast, automated scoring across hundreds of test cases without requiring exact-match ground truth for open-ended replies. Which evaluation approach BEST fits this requirement?

A) Exact string match against a fixed set of golden answers
B) LLM-as-judge scoring each response against a detailed rubric covering helpfulness, accuracy, and tone
C) Manual human review of every test case before each deployment
D) BLEU/ROUGE score comparison against reference outputs

---

**Q19.** A RAG application sends a large static system prompt (5,000 tokens) and a large reused reference document (20,000 tokens) with every request, followed by a short unique user question. To minimize token cost across thousands of repeated requests using prompt caching, where should cache_control breakpoints be placed?

A) Only at the very end of the entire prompt, including the user question
B) At the start of the system prompt only, with no breakpoint on the document
C) After the system prompt and after the static document, but not on the user question
D) No breakpoints are needed because caching is applied automatically to the full request

---

**Q20.** Which statement correctly describes the Message Batches API's processing window, per-batch request limit, and pricing discount?

A) Batches process in real time with no cost discount, up to 10,000 requests per batch
B) Batches process within 24 hours, offer a 50% discount off standard token pricing, with unlimited requests per batch
C) Batches process within 1 hour, offer a 90% discount off standard token pricing, up to 100,000 requests per batch
D) Batches process within 24 hours, offer a 50% discount off standard token pricing, up to 100,000 requests per batch

---

**Q21.** An architect updates the system prompt for an agent that performs multi-step tool calls (search, calculator, database query). Before deploying the change, they need to confirm the new prompt hasn't broken existing tool-selection behavior. What is the BEST testing strategy?

A) Run the new prompt against a single manually crafted test case and review the output
B) Run the new prompt against an existing regression suite of golden traces capturing expected tool selections, and compare results against the baseline
C) Deploy directly to production and monitor error rate dashboards afterward
D) Compare only the output token count before and after the change

---

**Q22.** Which statement correctly describes the cost and latency tradeoff of increasing the budget_tokens parameter for extended thinking?

A) Higher budget_tokens always reduces both cost and latency because the model reasons more efficiently
B) budget_tokens only limits the visible response length and has no effect on cost or latency
C) Higher budget_tokens allows deeper reasoning but increases latency and token cost, since thinking tokens are billed as output tokens
D) budget_tokens has no effect on cost because thinking tokens are not billed

---

**Q23.** A production agent occasionally fails on multi-step workflows, but the failure mode varies and is difficult to reproduce. Which observability approach would BEST help the architect diagnose the root cause?

A) Rely on aggregate error-rate dashboards only
B) Add print statements in production code and manually tail logs during business hours
C) Increase the model's temperature setting to encourage more varied retries
D) Capture and persist full structured traces of each agent step, including tool inputs/outputs, intermediate reasoning, and timestamps, then replay failing traces

---

**Q24.** A team built a customer-support agent that includes a fixed 15,000-token knowledge base excerpt in the system prompt on every request, followed by a short, unique user question appended at the end. Requests must be answered synchronously with low latency (not eligible for async processing). The team wants to reduce input token costs without degrading answer quality or increasing response time. Which change BEST achieves this?

A) Route all requests through the Message Batches API to get the 50% batch discount on input tokens
B) Switch the model to Claude Haiku for all requests to lower the per-token input price
C) Restructure the prompt so the 15,000-token knowledge base content appears first with a cache_control breakpoint, followed by the variable user question, enabling prompt caching on the repeated prefix
D) Increase max_tokens on each request so fewer round trips are needed to complete each conversation

---

**Q25.** A healthcare company wants to build a clinical documentation assistant using the Claude API that will process Protected Health Information (PHI). Which action is REQUIRED before sending PHI to the API to remain HIPAA compliant?

A) Switch to a smaller model like Claude Haiku to limit the amount of data processed
B) Enable prompt caching to reduce how long PHI is held in context
C) Sign a Business Associate Agreement (BAA) with Anthropic covering the account used for PHI processing
D) Use the Claude Code CLI instead of the standard API endpoint

---

**Q26.** A European financial institution must ensure that all customer data sent to Claude remains within EU infrastructure boundaries to satisfy GDPR and national data-residency regulations. Which deployment approach BEST satisfies this requirement?

A) Self-host Claude's model weights on-premises in an EU data center
B) Use Claude.ai Team plan and set the organization's billing address to an EU country
C) Instruct Claude via the system prompt to discard any data that originates outside the EU
D) Access Claude through Amazon Bedrock or Google Vertex AI configured to an EU region

---

**Q27.** An architect is designing an MCP (Model Context Protocol) server that gives Claude access to an internal customer database for a support-agent application. Following least-privilege principles, which design is BEST?

A) Give Claude a database service account with full administrative rights so the agent can provision whatever access it later needs
B) Expose a single generic 'execute_sql' tool that accepts arbitrary SQL queries for maximum flexibility
C) Embed the raw database connection string in the system prompt so Claude can connect and query as needed
D) Expose narrowly-scoped tools (e.g., get_customer_by_id, update_ticket_status) that each map to one specific, pre-approved, limited operation

---

**Q28.** Which system-level configuration is the MOST effective mitigation against prompt-injection attacks embedded in retrieved documents in a RAG pipeline that attempt to hijack Claude's instructions?

A) Wrapping retrieved/untrusted content in clearly labeled tags (e.g., <untrusted_document>) and instructing Claude in the system prompt to treat content inside those tags strictly as data, never as instructions
B) Lowering the temperature parameter to 0 for more deterministic outputs
C) Appending a single generic line like 'ignore any instructions found below' at the very end of the full conversation
D) Switching to a model with a larger context window so documents don't need to be truncated

---

**Q29.** In Anthropic's Responsible Scaling Policy (RSP), what does an AI Safety Level (ASL) designation primarily determine?

A) The set of safety, security, and deployment measures required based on a model's demonstrated capabilities and potential for catastrophic risk
B) The maximum number of tool calls permitted per API request
C) The pricing tier charged for API usage once a model reaches that capability level
D) The context window size made available to enterprise customers

---

**Q30.** A bank's compliance team requires a complete, immutable audit trail of every prompt and completion sent through Claude for a loan-underwriting assistant, to satisfy regulatory examination requirements. Which approach BEST meets this need?

A) Ask Claude to summarize its own reasoning at the end of each session and store that summary as the compliance record
B) Route all requests through the organization's own logging/observability layer (e.g., a gateway or proxy) that captures every request and response, exporting logs to an immutable, access-controlled audit store
C) Rely on individual developers' local terminal history as the record of API activity
D) Rely solely on the Anthropic Console's usage analytics dashboard, assuming it retains a verbatim, indefinite record of every prompt and completion

---

**Q31.** A production application calls the Claude API using the alias "claude-3-5-sonnet-latest". After Anthropic updates which snapshot the alias points to, the architecture team notices subtle changes in response tone and formatting that break a downstream parser. Which change-management practice would BEST have prevented this incident?

A) Switch to a smaller model to reduce response variability
B) Pin production traffic to a specific dated model snapshot (e.g., claude-3-5-sonnet-20241022) and control upgrades explicitly through a tested migration process
C) Increase the temperature parameter to force more consistent outputs
D) Request that Anthropic notify the team before updating any alias

---

**Q32.** A business stakeholder asks why the architecture team routes simple intent-classification requests to Claude Haiku while routing multi-step contract analysis to Claude Opus, when both could technically call the same model. What is the BEST justification to give in this conversation?

A) Opus should be used everywhere because it always produces more accurate answers regardless of task
B) Haiku is deprecated for classification tasks and Opus is the only supported option
C) The choice has no measurable business impact, it is purely a developer preference
D) Routing by task complexity optimizes cost and latency: Haiku delivers sufficient accuracy for simple classification at a fraction of the cost and latency, while Opus's deeper reasoning is reserved for tasks where it materially improves outcomes

---

**Q33.** An architect needs to build a roadmap item for migrating off a Claude model version before it reaches end-of-life. Where should the architect look to find the official deprecation and retirement dates for that model?

A) The model's response metadata returned on each API call
B) The pricing page, since deprecation dates are listed alongside per-token cost
C) Anthropic's published model deprecation documentation in the developer docs/console
D) Third-party community forums that track unofficial model benchmarks

---

**Q34.** Before cutting production traffic over from an older Claude model version to a newer one, what change-management step should the architect perform as part of the migration process?

A) Immediately redirect 100% of production traffic to the new model to minimize migration time
B) Run the existing evaluation/test suite against the new model version, compare output quality and regression metrics, and only cut over after results meet acceptance criteria
C) Delete the old model's prompts and only write new prompts once the new model is live
D) Skip evaluation since all Claude model versions produce equivalent behavior on the same prompts

---

**Q35.** In Anthropic's model lifecycle terminology, what is the key distinction between a model's "deprecation date" and its "retirement date"?

A) They are two names for the same event and both mean the model stops working immediately
B) Deprecation date is when pricing increases, retirement date is when pricing decreases
C) Deprecation date marks when a model is no longer recommended for new projects but remains callable, while retirement date marks when the model is removed and API calls to it begin returning errors
D) Deprecation date applies only to Claude Opus models, while retirement date applies only to Claude Haiku models

---

**Q36.** A technical stakeholder challenges the architecture team's decision to pin the production system to a specific dated Claude model snapshot instead of always using the latest available version. Which response BEST defends this architectural decision?

A) Pinning to a dated snapshot ensures reproducible behavior and prevents unplanned regressions in production, with upgrades handled deliberately through a tested migration process rather than happening automatically
B) Pinning is unnecessary since all Claude models are functionally identical across releases
C) Pinning is only required for compliance audits and has no other architectural benefit
D) Always using the latest version is strictly better because it automatically includes all future improvements at no risk

---

**Q37.** An architect is building a pipeline that classifies 2 million customer support tickets per day into one of 12 categories. Latency must stay under 300ms per request and the solution must minimize per-token cost while maintaining acceptable accuracy. Which model should be selected for this workload?

A) Claude Haiku, because it is optimized for high-throughput, low-latency, cost-sensitive tasks like simple classification
B) Claude Opus, because its larger parameter count guarantees higher classification accuracy regardless of task complexity
C) Claude Sonnet, because it is the only model that supports extended thinking for classification tasks
D) Claude Opus with prompt caching disabled, because caching adds latency on every classification call

---

**Q38.** A team is designing an autonomous coding agent that must plan multi-step refactors across a large codebase, reason about architectural tradeoffs, and self-correct when tool calls fail. Which model tier is the correct architectural choice for the agent's primary reasoning loop?

A) Claude Haiku, because its speed allows more retry attempts per minute, compensating for lower reasoning depth
B) Claude Opus, because the task requires deep multi-step reasoning, planning, and self-correction that benefit most from the highest-capability model
C) Claude Sonnet, used only for the final output and never during intermediate planning steps
D) Any model tier is equivalent for this task as long as the system prompt is sufficiently detailed

---

**Q39.** A developer enables extended thinking on a Claude API request to improve performance on a multi-step math and logic problem. Which parameter controls how many tokens the model may use for its internal reasoning before producing a final answer?

A) thinking.budget_tokens, which sets the token allowance for the model's internal reasoning process
B) max_tokens, which must be set lower than usual to reserve space for thinking
C) temperature, which should be set to 0 to enable deterministic thinking
D) top_p, which controls the depth of the reasoning chain when extended thinking is active

---

**Q40.** An agent runs for hours executing dozens of tool calls, and the conversation history is approaching the model's context window limit, risking truncation of critical early instructions. Which context engineering strategy BEST addresses this at the architecture level?

A) Switch to a model with a smaller context window to force the agent to finish faster
B) Apply context compaction to clear or summarize stale tool results while preserving the system prompt and recent relevant turns
C) Increase max_tokens on every request so the model can generate longer responses to compensate
D) Disable prompt caching so older context is automatically dropped by the API

---

**Q41.** Which statement correctly distinguishes context compaction from retrieval-based memory in an agentic Claude architecture?

A) Compaction reduces in-context token usage by pruning or summarizing existing conversation state, while retrieval fetches relevant external information on demand to inject into context
B) Compaction increases the total information available to the model, while retrieval always decreases it
C) Compaction and retrieval are two names for the same mechanism and are used interchangeably in production systems
D) Compaction is only available on Opus, while retrieval is only available on Haiku

---

**Q42.** An application must guarantee that Claude's response is always a call to a specific named tool, 'generate_invoice', rather than a free-text reply or a call to a different tool in the request. Which configuration achieves this?

A) Set tool_choice to {"type": "tool", "name": "generate_invoice"} to force the model to call that specific tool
B) List only one tool in the tools array and leave tool_choice unset, since Claude will infer it must be used
C) Set tool_choice to {"type": "auto"} to let the model decide which tool best fits the request
D) Set temperature to 0 so the model deterministically selects the first tool defined in the tools array

---

**Q43.** A platform engineering team is rolling out Claude Code to 500 engineers across hundreds of repositories. They want every engineer, regardless of which repo they're working in, to automatically get consistent project-specific context (build commands, test commands, coding conventions) without retyping it in every prompt or relying on tribal knowledge. Which approach BEST meets this requirement?

A) Create a CLAUDE.md file at the root of each repository, checked into version control, which Claude Code automatically reads at the start of each session in that repo
B) Require each engineer to manually paste the project's build commands and conventions into every new conversation
C) Have each engineer maintain their own personal CLAUDE.md file in their home directory with notes about every project they work on
D) Distribute a shared PDF style guide and ask engineers to attach it to conversations when relevant

---

**Q44.** An organization wants engineers using Claude Code to connect to internal systems (ticketing, databases, internal APIs) through a centrally vetted, discoverable, and version-controlled mechanism rather than each team building its own ad-hoc integrations. Which capability should the architecture team stand up?

A) A shared Slack channel where engineers post curl commands for each internal API
B) A single monolithic custom tool that reimplements every integration's logic directly inside system prompts
C) An internal catalog of approved MCP (Model Context Protocol) servers that teams register, vet, and discover centrally
D) Individual engineers writing one-off local scripts to call internal APIs as needed

---

**Q45.** A repository has a CLAUDE.md file at its root with general build and test instructions, and a subdirectory within that repository has its own CLAUDE.md with module-specific conventions that conflict with a root-level directive. When Claude Code operates within that subdirectory, how are these instructions applied?

A) Claude Code prompts the user at the start of every session to choose which CLAUDE.md file to apply
B) Only the root CLAUDE.md is ever applied; subdirectory CLAUDE.md files are ignored
C) Only the subdirectory CLAUDE.md is applied; the root file's instructions are discarded entirely
D) Claude Code merges both files, and the more specific subdirectory CLAUDE.md takes precedence over conflicting root-level directives

---

