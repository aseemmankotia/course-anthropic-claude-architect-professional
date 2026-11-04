## Chapter 5: Solution Design II: Resilience, Scaling, and Reference Architectures — Practice Questions

**Q1.** An enterprise RAG assistant experiences a 5x traffic spike during a product launch and begins receiving HTTP 429 responses from the upstream LLM provider. Which resilience strategy BEST maintains service availability without silently dropping user requests?

A) Immediately fail the request and return an error to the user so the client can retry manually
B) Implement exponential backoff with jitter, queue excess requests, and fall back to a smaller/cheaper model tier if the primary model's quota is exhausted
C) Increase the request timeout value so the client waits longer for the same model call to succeed
D) Disable retries entirely to avoid amplifying load on the already-throttled provider

<details>
<summary>Answer</summary>

**Correct: B**

Exponential backoff with jitter prevents retry storms, queuing smooths bursty load, and tiered model fallback preserves availability by trading quality for throughput when the primary tier is saturated.

**Why the others are wrong:**
- Failing immediately pushes the burden to the client and produces a poor user experience with no graceful degradation
- Increasing timeout does nothing to address a 429 (quota/rate-limit) response — the request is being rejected, not slow
- Disabling retries entirely removes a core resilience mechanism; the issue is uncontrolled retries, not retries themselves

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A financial services firm needs multi-region high availability for its AI assistant but must keep EU customer data within EU borders due to regulatory constraints. Which architecture BEST satisfies both availability and data residency requirements?

A) Single active-active deployment replicating all customer data synchronously across US and EU regions
B) A single global region hosting all traffic with a CDN cache in front for EU users
C) Active-passive deployment per region, with each region's data isolated locally and only non-sensitive operational metadata replicated cross-region
D) Active-active deployment with EU data asynchronously replicated to a US region for disaster recovery

<details>
<summary>Answer</summary>

**Correct: C**

Per-region active-passive isolates regulated data within its region of origin while still providing in-region failover, and only replicating non-sensitive metadata avoids cross-border data transfer violations.

**Why the others are wrong:**
- Synchronous cross-region replication of customer data directly violates EU data residency requirements
- A single global region does not provide regional failover/HA and may not satisfy residency requirements at all
- Async replication of EU data to a US region is the same residency violation, just delayed rather than synchronous

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A system sends thousands of requests per day that share an identical long system prompt but have unique user queries appended. Which combination of techniques BEST reduces both cost and latency for this workload?

A) Use prompt caching to avoid reprocessing the static prefix on every call, combined with a semantic cache for repeated/near-duplicate full queries
B) Increase the model's context window so the system prompt only needs to be sent once per session
C) Switch to a smaller model for all requests regardless of query complexity
D) Store all past responses in a relational database and perform exact string matching on the full prompt before every call

<details>
<summary>Answer</summary>

**Correct: A**

Prompt caching reuses the processed prefix (the static system prompt) to cut compute and latency, while a semantic cache catches repeated or paraphrased user intents that exact-match caching would miss.

**Why the others are wrong:**
- A larger context window doesn't eliminate reprocessing cost for stateless API calls; the prefix is still sent and billed each time without caching
- Blanket downgrade to a smaller model sacrifices quality and doesn't specifically address the repeated-prefix cost problem
- Exact string matching misses the vast majority of real-world near-duplicate queries, unlike semantic caching

</details>

---

**Q4.** In a circuit breaker implementation protecting a call to an external tool API, which transition correctly describes how the breaker moves from OPEN back to allowing traffic?

A) After a cooldown timeout elapses, the breaker enters HALF-OPEN and allows a limited number of trial requests to determine if the dependency has recovered
B) The breaker stays OPEN permanently until manually reset by an operator
C) The breaker immediately returns to CLOSED as soon as one request happens to succeed, with no trial period
D) The breaker alternates between OPEN and CLOSED on every single request regardless of success or failure

<details>
<summary>Answer</summary>

**Correct: A**

The standard circuit breaker pattern is CLOSED → OPEN (after failure threshold) → HALF-OPEN (after cooldown, limited trial traffic) → CLOSED or back to OPEN depending on trial results.

**Why the others are wrong:**
- Requiring manual reset removes the automated self-healing behavior that makes circuit breakers useful at scale
- Skipping the HALF-OPEN trial phase risks immediately overwhelming a still-recovering dependency
- Toggling on every request is not a circuit breaker at all — it provides no protection against cascading failures

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A multi-system agentic workflow has an orchestrator agent calling a flaky third-party inventory API through a tool call. The API intermittently times out under load. Which design BEST limits the blast radius of this failure on the overall workflow?

A) Let the orchestrator retry the tool call indefinitely until it succeeds
B) Remove the inventory tool from the agent entirely so the workflow never depends on it
C) Increase the number of concurrent agents calling the inventory API to improve overall throughput
D) Wrap the tool call in a per-dependency circuit breaker with bounded retries and a fallback to last-known-good cached inventory data

<details>
<summary>Answer</summary>

**Correct: D**

Isolating the flaky dependency behind its own circuit breaker with bounded retries prevents cascading failures, and a cached fallback keeps the workflow functional in degraded mode instead of blocking entirely.

**Why the others are wrong:**
- Indefinite retries can cause request pile-up and worsen load on an already-struggling dependency
- Removing the tool eliminates required functionality rather than solving the reliability problem
- Adding more concurrent callers to a struggling API increases load and makes timeouts more likely, not less

</details>

---

**Q6.** A workload needs to scale horizontally past a single provider API key's rate limit without exceeding per-key quotas. Which approach is MOST appropriate?

A) Request a single larger quota increase and continue routing all traffic through one API key
B) Reduce the number of application servers so fewer requests are generated overall
C) Deploy a gateway layer that pools multiple API keys/quota allocations and distributes requests using per-key token-bucket rate limiting
D) Cache every response indefinitely so no new requests are ever sent to the provider

<details>
<summary>Answer</summary>

**Correct: C**

A gateway that pools multiple keys/quota pools and enforces per-key rate limiting (token bucket) enables true horizontal scaling of throughput while staying within each individual quota.

**Why the others are wrong:**
- A single larger quota still caps throughput at one ceiling and doesn't provide the resilience of multiple independent pools
- Reducing server count addresses capacity, not the rate-limit/quota-management problem, and hurts the workload's own scalability
- Caching indefinitely is unrealistic for unique/dynamic queries and doesn't solve the scaling problem generally

</details>

---

**Q7.** A retail company expects predictable seasonal peak traffic (e.g., a scheduled sale event) for its AI assistant. Which capacity planning approach is MOST appropriate?

A) Pre-warm additional capacity and provisioned throughput ahead of the known peak window, combined with autoscaling for unexpected overflow
B) Rely solely on reactive autoscaling triggered after load has already begun climbing
C) Permanently provision for peak capacity year-round regardless of average load
D) Take no special action since autoscaling alone always responds instantly to any load pattern

<details>
<summary>Answer</summary>

**Correct: A**

For known, predictable spikes, pre-warming capacity avoids cold-start/scale-up lag, while autoscaling still handles unexpected variance beyond the forecast — a hybrid approach is the enterprise best practice.

**Why the others are wrong:**
- Purely reactive autoscaling introduces lag (cold starts, queueing) that can cause failures right at the start of a known spike
- Permanent peak provisioning wastes cost during normal periods and ignores the fact that the spike is time-bound and predictable
- Autoscaling has inherent latency to provision new capacity; it is not instantaneous, which the exam tests directly

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement BEST describes a graceful degradation (fallback) strategy across model tiers?

A) When the primary model fails, the system immediately returns a generic HTTP 500 error to preserve consistency
B) The system always routes every request to the cheapest available model to avoid any risk of failure
C) Fallback only applies to network-level failures and never to quota or rate-limit conditions
D) When the primary model is unavailable or over quota, the system automatically falls back through progressively cheaper/faster options — a smaller model, then a cached or templated response — before failing outright

<details>
<summary>Answer</summary>

**Correct: D**

A proper degradation ladder progressively trades quality for availability (large model → small model → cache/template → error), maximizing the chance of a useful response instead of an outright failure.

**Why the others are wrong:**
- Returning a hard error immediately skips all intermediate degradation options and is the opposite of graceful degradation
- Always using the cheapest model abandons quality unnecessarily even when the primary model is healthy
- Rate-limit and quota exhaustion are among the most common triggers for fallback in production systems, not an exception to it

</details>

---

