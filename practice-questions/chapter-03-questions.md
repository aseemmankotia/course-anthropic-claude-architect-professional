## Chapter 3: Integration II: Retrieval, Identity, and Event-Driven Patterns — Practice Questions

**Q1.** A retail company's RAG assistant answers customer questions using product catalog data that changes every few minutes (price, stock) and a compliance policy document that changes twice a year. Which integration strategy BEST balances freshness and cost?

A) Re-embed the entire corpus every night via a scheduled batch job
B) Use the Message Batches API to re-embed both data sources every hour
C) Cache all embeddings permanently and never refresh them
D) Use event-driven webhooks to re-embed product catalog changes in near real-time, and batch re-embed the policy document only when a new version is published

<details>
<summary>Answer</summary>

**Correct: D**

Matches sync strategy to each data source's own change frequency: event-driven for high-churn data, trigger-based batch for rarely-changing data, minimizing cost while preserving accuracy.

**Why the others are wrong:**
- Nightly batch leaves prices/stock stale for up to 24 hours, violating freshness needs for fast-changing data.
- Hourly re-embedding of a document that changes twice a year wastes compute; Message Batches API is a bulk-processing tool, not a freshness guarantee.
- Permanently cached embeddings never refresh, failing freshness requirements entirely.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An agent built on MCP needs to access a user's personal calendar events, respecting that the agent should never see calendar data the user themselves can't see. Which authentication pattern should the MCP server use?

A) A shared service account with admin-level calendar access
B) An API key stored in the agent's system prompt
C) Hardcoded credentials embedded in the MCP server code
D) Delegated OAuth 2.0 authorization where the agent acts on behalf of the authenticated user with user-scoped tokens

<details>
<summary>Answer</summary>

**Correct: D**

Delegated OAuth ensures the agent inherits exactly the calling user's permission scope, satisfying least-privilege and per-user data visibility.

**Why the others are wrong:**
- A service account with admin access violates least privilege and could expose calendars the user cannot see.
- An API key in a system prompt is a secrets-management anti-pattern, not a delegation mechanism.
- Hardcoded credentials are insecure, non-rotatable, and not scoped to any individual user.

</details>

---

**Q3.** When batching 80,000 independent classification requests through Anthropic's Message Batches API, which constraint must the architecture account for?

A) Each batch is limited to a maximum of 1,000 individual requests
B) Batches must complete within 60 seconds or they are discarded
C) Batches cannot exceed 10 requests per batch
D) Each batch is limited to a maximum of 100,000 individual requests

<details>
<summary>Answer</summary>

**Correct: D**

The Message Batches API supports up to 100,000 requests per batch, so 80,000 requests fit within a single batch.

**Why the others are wrong:**
- This understates the real limit by two orders of magnitude, forcing unnecessary batch splitting.
- Batches are designed for asynchronous processing within a 24-hour window, not a 60-second SLA.
- This value is far too small and not the actual documented limit.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A healthcare provider's claims system runs on a mainframe that exports a flat file once every 24 hours via FTP and cannot support webhooks or APIs. An agent needs to reconcile claims data daily. Which integration pattern is appropriate?

A) Require the mainframe team to build a REST API with webhook support before integrating
B) Build a scheduled polling job that retrieves and parses the flat file on the same cadence as the export, feeding it into the retrieval pipeline
C) Use OAuth 2.0 delegated authentication to connect directly to the mainframe
D) Use the Message Batches API to query the mainframe in real time

<details>
<summary>Answer</summary>

**Correct: B**

Legacy mainframe constraints (flat file, no API) require polling/batch ingestion aligned with the source's own update cadence — the standard documented pattern for legacy-system integration.

**Why the others are wrong:**
- Unrealistic given the constraint stated in the scenario; the question asks for a solution given the limitation, not how to eliminate it.
- OAuth is an identity/auth pattern and has no application to FTP flat-file exports with no such auth model.
- The Message Batches API is for LLM inference batching, not a mechanism for querying a mainframe.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A RAG system returns poor results for queries containing exact product SKUs and part numbers, even though semantic similarity search works well for conceptual questions. Which retrieval approach addresses both cases?

A) Implement hybrid search combining dense vector embeddings with sparse/keyword (BM25) retrieval, merged via reranking
B) Increase the embedding dimensionality
C) Switch entirely to keyword-based BM25 search
D) Increase the chunk size to include more context per chunk

<details>
<summary>Answer</summary>

**Correct: A**

Hybrid search is the standard answer for mixed query types: exact-match tokens (SKUs) need lexical/sparse retrieval while conceptual queries need dense embeddings.

**Why the others are wrong:**
- Higher dimensionality doesn't fix exact-token matching; semantic embeddings still generalize SKUs poorly.
- Pure BM25 would degrade conceptual query performance, trading one problem for another.
- Larger chunks don't fix exact-match retrieval failures and can dilute relevance.

</details>

---

**Q6.** A document chunking strategy for a RAG pipeline splits a 50-page technical manual into chunks with zero overlap between consecutive chunks. During evaluation, answers frequently miss context that spans a chunk boundary. What is the BEST fix?

A) Reduce chunk size further to make chunks more precise
B) Introduce chunk overlap (commonly 10-20% of chunk size) so boundary context is preserved across chunks
C) Switch to character-based chunking instead of token-based chunking
D) Remove chunking entirely and embed the whole document as one chunk

<details>
<summary>Answer</summary>

**Correct: B**

Overlap is the standard fix for boundary-context loss, a well-tested exam gotcha in chunking strategy design.

**Why the others are wrong:**
- Smaller chunks with zero overlap make boundary loss worse, not better.
- Character vs token basis has no bearing on overlap/boundary issues.
- Embedding the whole document as one chunk destroys retrieval precision and likely exceeds context/embedding limits.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which statement correctly characterizes the tradeoff between event-driven and batch integration patterns?

A) Event-driven integration always costs less than batch processing regardless of data volume
B) Batch integration is only usable for data under 1MB in size
C) Event-driven integration provides lower latency and freshness at the cost of higher architectural complexity (queues, webhooks, idempotency handling), while batch integration is simpler and more cost-efficient for high-volume, low-freshness-sensitivity data
D) Event-driven and batch integration cannot be used together in the same system

<details>
<summary>Answer</summary>

**Correct: C**

This is the canonical tradeoff — freshness/latency vs complexity/cost — directly tested on the exam.

**Why the others are wrong:**
- Event-driven infrastructure (brokers, webhook endpoints, retry/dedup logic) often costs more to build and operate than scheduled batch jobs.
- This is a fabricated size constraint not tied to any real limitation of batch integration.
- Hybrid architectures combining both patterns per data source are common and often the recommended design.

</details>

---

**Q8.** An enterprise agent needs to pull records from a third-party CRM on behalf of hundreds of different sales reps, each with different record-level permissions in the CRM. Which authentication design should the architecture use?

A) A single service account with CRM admin rights shared across all agent sessions
B) Hardcode each rep's CRM password into a configuration file
C) Per-user delegated OAuth tokens obtained via SSO, so each agent session queries the CRM with that specific rep's permission scope
D) Use the Message Batches API to bypass the CRM's authentication layer

<details>
<summary>Answer</summary>

**Correct: C**

Per-user delegated OAuth via SSO correctly enforces record-level permission boundaries that differ per rep — the standard design for multi-tenant identity propagation.

**Why the others are wrong:**
- A shared admin service account would let any session see data beyond what the requesting rep is authorized to view, a security/compliance failure.
- Storing plaintext passwords is a severe security anti-pattern, not an authentication design.
- The Message Batches API has nothing to do with authentication; it is an inference-batching feature and cannot bypass CRM auth.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

