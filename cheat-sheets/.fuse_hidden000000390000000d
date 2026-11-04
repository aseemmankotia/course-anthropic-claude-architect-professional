# Chapter 3 Cheatsheet: Retrieval, Identity, Event-Driven Patterns

## RAG Pipeline Design
| Component | Rule of Thumb | Exam Trap |
|---|---|---|
| Chunking | 10-20% overlap between chunks | Zero overlap → lost boundary context |
| Retrieval | Hybrid (dense + BM25) for mixed queries | Pure semantic search fails on SKUs/IDs/codes |
| Freshness | Match sync cadence to source's own change rate | 'Nightly batch for everything' is rarely the best answer |

## Freshness/Sync Strategy Decision
| Data changes... | Use |
|---|---|
| Every few minutes (price, stock) | Event-driven webhook → immediate re-embed |
| Rarely (policy docs, 1-2x/year) | Trigger on publish, or periodic low-frequency batch |
| High volume, low urgency | Message Batches API |

## Identity & Auth for Agents
| Pattern | Use When | Risk if Misused |
|---|---|---|
| Delegated OAuth (user-scoped token) | Per-user permission boundaries must be enforced (CRM, calendar, email) | N/A — this is the default-correct pattern |
| Service account | Agent needs its own fixed, narrow permission (e.g., read-only system metrics) | Over-privileging if used for per-user data |
| Shared admin service account | Rarely correct | Breaks least-privilege, cross-user data leakage |
| MCP remote server auth | OAuth 2.1 authorization flow | Confusing with local/stdio MCP (host-credential based) |

## Event-Driven vs Batch Integration
| Factor | Event-Driven | Batch |
|---|---|---|
| Latency/freshness | Low latency, near real-time | Latency bound by schedule |
| Complexity | Higher — queues, webhooks, idempotency/dedup | Lower — scheduled jobs |
| Cost at scale | Can be higher (always-on infra) | Efficient for high-volume, low-urgency data |
| Best fit | Frequently changing, consumer-facing data | Legacy exports, bulk reconciliation, archives |

## Message Batches API
- Max **100,000 requests** per batch
- Max **256MB** total batch size
- Results typically within **24 hours**
- Up to **50% cost discount** vs synchronous calls
- Use for: bulk classification, large-scale re-embedding prep, non-latency-sensitive workloads
- NOT a substitute for real-time auth, queries, or webhooks

## Legacy System Integration
| Constraint | Pattern |
|---|---|
| Mainframe, flat file, FTP, no API | Scheduled polling matched to source export cadence |
| No webhook support | Never assume you can force event-driven; poll instead |
| Fixed nightly batch export | Pipeline ingestion job synced to same cadence |

## Quick Gotchas
- 'Re-embed everything nightly' is a trap when any source changes faster than daily.
- Service accounts are not inherently wrong — they're wrong when data requires per-user scoping.
- Hybrid search beats both pure dense and pure sparse search for mixed query types.
- Overlap fixes boundary context loss; smaller chunks alone do not.
