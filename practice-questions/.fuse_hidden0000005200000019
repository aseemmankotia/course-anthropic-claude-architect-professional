## Chapter 2: Integration I: APIs, Middleware, and MCP Server Design — Practice Questions

**Q1.** A financial services firm needs Claude to query transaction history from a 20-year-old mainframe exposed only via SOAP. The architecture team wants to minimize the attack surface and avoid letting Claude call mainframe operations directly. Which design BEST satisfies least-privilege tool exposure?

A) Build an MCP server that wraps the SOAP service and exposes only narrowly-scoped, read-only tools (e.g., get_transaction_by_id) with validated input schemas
B) Give Claude direct SOAP client credentials and let it call any mainframe operation it needs
C) Expose the entire SOAP WSDL as a single generic 'call_soap_operation' tool with a free-text parameter
D) Have Claude generate raw SOAP XML envelopes itself and send them through a generic HTTP tool

<details>
<summary>Answer</summary>

**Correct: A**

An MCP server acting as an adapter/anti-corruption layer in front of the legacy SOAP system lets you expose only the specific, validated operations Claude needs, enforcing least privilege and input validation at the boundary.

**Why the others are wrong:**
- Direct credentials and unrestricted operation access violates least-privilege and removes any guardrail layer.
- A single generic 'call any SOAP operation' tool defeats the purpose of least-privilege tool design — it's equivalent to full access with extra steps.
- Letting the model author raw SOAP XML introduces injection risk and removes schema validation, the opposite of enterprise-safe tool design.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An MCP server fronting an internal order-management API starts receiving bursts of tool calls that exceed the downstream system's capacity, causing 5xx errors to propagate back to Claude. Which middleware pattern should be added to the MCP server to handle this?

A) Increase the Claude model's context window so it makes fewer calls
B) Remove input validation from the tool schema to speed up calls
C) Switch the MCP server from stdio transport to a different transport
D) Add a backpressure mechanism (e.g., request queuing with a token-bucket limiter and exponential backoff on retries) between the MCP server and the downstream API

<details>
<summary>Answer</summary>

**Correct: D**

Backpressure handling (queuing, rate limiting, exponential backoff) is the correct middleware response to downstream capacity limits and protects the legacy system from overload.

**Why the others are wrong:**
- Context window size is unrelated to how fast tool calls are issued or how downstream load is managed.
- Removing validation increases risk and does nothing to reduce call volume or protect the downstream system.
- Transport choice (stdio vs HTTP) affects connectivity, not request throughput to the downstream API.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A legacy CRM uses outdated field names and a non-standard date format incompatible with modern conventions. The integration team wants Claude's tool calls to use clean, modern parameter names while the legacy system still receives its original format. Which pattern addresses this?

A) A rate limiter
B) A connection pool
C) An anti-corruption layer that translates between the clean MCP tool schema and the legacy system's domain model/format
D) A prompt template exposed via MCP prompts

<details>
<summary>Answer</summary>

**Correct: C**

An anti-corruption layer (ACL) is specifically designed to isolate a clean domain model from a legacy system's incompatible model, translating field names, formats, and semantics at the boundary.

**Why the others are wrong:**
- A rate limiter controls call frequency, not data/schema translation.
- Connection pooling manages resource reuse for connections, not semantic translation.
- MCP prompts are reusable prompt templates for the LLM, not a mechanism for translating API data formats.

</details>

---

**Q4.** A team is designing an MCP server that must expose both read-only reference data (product catalog) and callable actions (place_order) to Claude. Which MCP primitives should they use respectively for the catalog and the order action?

A) Prompts for the catalog, Resources for the order action
B) Tools for both the catalog and the order action
C) Resources for the catalog, Tools for the order action
D) Resources for both the catalog and the order action

<details>
<summary>Answer</summary>

**Correct: C**

MCP Resources are meant for exposing contextual/read-only data (like a catalog) that Claude can read, while Tools represent callable functions with side effects (like placing an order).

**Why the others are wrong:**
- Prompts are reusable instruction templates, not a mechanism for exposing read-only data.
- Using Tools for a static catalog works but loses the semantic distinction and the client-side caching/subscription benefits Resources provide.
- Resources are not designed for actions with side effects like placing an order; that requires a Tool.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** An enterprise has a dozen internal REST APIs (HR, finance, inventory) that Claude needs to access through MCP. Leadership wants a single place to enforce authentication, logging, and throttling before requests reach any backend. Which architectural component should be introduced?

A) A separate MCP server per API with duplicated auth logic in each
B) A gateway in front of the MCP servers that centralizes authentication, logging, and rate limiting before routing to the appropriate backend
C) Embedding auth tokens directly into the Claude system prompt
D) A anti-corruption layer inside each backend API's database

<details>
<summary>Answer</summary>

**Correct: B**

A gateway pattern centralizes cross-cutting concerns (auth, logging, throttling) in one place rather than duplicating them across every MCP server, which is the enterprise-scale best practice.

**Why the others are wrong:**
- Duplicating auth/logging logic across many servers increases maintenance burden and inconsistency risk.
- Embedding tokens in the prompt exposes secrets to the model and violates security best practice.
- An anti-corruption layer addresses domain model translation, not cross-cutting operational concerns like auth and throttling.

</details>

---

**Q6.** When designing the input_schema for an enterprise MCP tool called create_support_ticket, which approach best supports reliable, validated tool-use at scale?

A) Define explicit required fields (e.g., title, description, priority) with JSON Schema types and enums (e.g., priority restricted to "low"|"medium"|"high") so Claude's structured output can be validated before reaching the backend
B) Accept a single free-text 'details' string field and let Claude format everything into it
C) Omit the schema and rely on the tool's description alone to communicate expected parameters
D) Allow all fields to be optional so the tool never fails validation

<details>
<summary>Answer</summary>

**Correct: A**

Well-defined JSON Schema with required fields, types, and enums constrains Claude's tool-call output and allows the MCP server to validate input before touching the backend — critical at enterprise scale.

**Why the others are wrong:**
- A single free-text field forces downstream parsing and reintroduces the ambiguity structured tool-use is meant to eliminate.
- Without a schema, Claude has no reliable contract for what arguments to produce, increasing malformed calls.
- Making everything optional avoids validation errors but allows invalid/incomplete tickets to reach the backend, pushing the problem downstream.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** An MCP server needs to run as a locally-launched subprocess for a developer's desktop Claude client, while another deployment of the same server must be reachable remotely by multiple enterprise users over a network. Which transport choice fits each case respectively?

A) stdio for the remote enterprise deployment, HTTP-based transport for the local desktop case
B) Only stdio can be used in both cases
C) Only HTTP-based transport can be used in both cases
D) stdio for the local desktop subprocess, a network-based (Streamable HTTP) transport for the remote multi-user deployment

<details>
<summary>Answer</summary>

**Correct: D**

MCP's stdio transport is designed for locally-spawned processes communicating over standard input/output, while Streamable HTTP transport supports remote, multi-client, network-accessible MCP servers.

**Why the others are wrong:**
- This reverses the correct mapping — stdio cannot serve multiple remote clients over a network.
- stdio alone cannot support remote multi-user access, which is a key exam distinction.
- HTTP-based transport is unnecessary overhead for a simple local subprocess use case, though it's not technically wrong — the question asks for the best fit for both scenarios, which stdio+HTTP satisfies.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** In MCP architecture, what is the key functional difference between a Tool and a Prompt?

A) Tools are always faster than Prompts
B) Prompts can modify backend state while Tools cannot
C) Tools are callable functions that can produce side effects or fetch data, while Prompts are reusable, user-selectable templates for structuring interactions with the LLM
D) There is no functional difference; they are interchangeable terms in MCP

<details>
<summary>Answer</summary>

**Correct: C**

In the MCP spec, Tools are model-callable functions (may have side effects), while Prompts are predefined templates that help structure interactions — a distinct, non-overlapping primitive from Resources and Tools.

**Why the others are wrong:**
- Speed is not a defined distinguishing characteristic between the two primitive types.
- This reverses the roles — Prompts do not perform backend actions; Tools do.
- They are distinct primitives in the MCP spec with different purposes and are not interchangeable.

</details>

---

