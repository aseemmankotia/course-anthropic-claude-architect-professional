# Chapter 5 Cheat Sheet: Resilience, Scaling, Reference Architectures

## Resilience Pattern Triad
| Pattern | Purpose | Key Detail |
|---|---|---|
| Retry w/ backoff + jitter | Handle transient failures | Jitter prevents synchronized retry storms |
| Circuit breaker | Prevent cascading failure | States: CLOSED → OPEN → HALF-OPEN → CLOSED |
| Fallback/degradation ladder | Preserve availability over quality | Large model → small model → cache/template → error |

## Circuit Breaker State Machine
- **CLOSED**: normal traffic flows; failures counted.
- **OPEN**: triggered after failure threshold; requests short-circuited immediately.
- **HALF-OPEN**: after cooldown, limited trial requests test recovery; success → CLOSED, failure → back to OPEN.

## Rate Limit & Quota Management
| Technique | When to Use |
|---|---|
| Exponential backoff + jitter | Handling 429 responses from provider |
| Respect `Retry-After` header | Provider gives explicit wait time |
| Token bucket limiter | Smooth outbound traffic per API key |
| Multi-key/quota pooling via gateway | Horizontal scale past single-key limits |
| Request queueing/backpressure | Absorb bursts without dropping requests |

## Caching Layers
| Cache Type | What It Caches | Benefit |
|---|---|---|
| Prompt caching | Static prefix (system prompt) tokens | Lower cost + latency on repeated prefix |
| Semantic cache | Full responses matched by meaning | Avoids reprocessing near-duplicate queries |
| Exact-match cache | Identical full queries | Simple but low hit-rate alone |

## Reference Architecture: Enterprise RAG Assistant
Ingestion → Chunk/Embed → Vector DB → Retriever → Reranker → LLM Generation → Guardrails/Filter → Cache Layer → Response

## Reference Architecture: Multi-System Agentic Workflow
Orchestrator → Planner → Tool-Calling Sub-Agents → Shared State/Memory → Message Queue → (Optional) Human-in-the-Loop → Output Aggregator
- Apply circuit breaker + retry **per tool/dependency**, not just at orchestrator level.

## High Availability Across Regions
| Model | Description | Use When |
|---|---|---|
| Active-Active | All regions serve live traffic | Max availability, no strict data residency conflict |
| Active-Passive | One region live, one standby | Simpler ops, satisfies per-region data residency |
- Replicate only non-sensitive metadata across regions when residency laws apply.

## Capacity Planning for Peak Load
- Predictable spikes → **pre-warm capacity** ahead of time + autoscaling for overflow.
- Unpredictable spikes → autoscaling + queueing + fallback tiers.
- Reactive-only autoscaling has inherent scale-up lag — not sufficient alone for known peaks.

## Common Exam Traps
- Choosing backoff *without* jitter as 'correct' — always prefer jitter.
- Forgetting HALF-OPEN state in circuit breaker transitions.
- Picking a hard failure/error response instead of a degradation ladder.
- Synchronous cross-region replication of regulated data (residency violation).
- Assuming autoscaling reacts instantly for known, scheduled peak events.
