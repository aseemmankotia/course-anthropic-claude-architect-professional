# Chapter 8 Cheat Sheet: Stakeholder Communication & Lifecycle Management

## ADR Structure (memorize the order)
| Section | Purpose |
|---|---|
| Title/Status | Proposed/Accepted/Superseded |
| Context | Problem + constraints |
| Decision | What was chosen |
| Alternatives Considered | Options + rejection rationale |
| Consequences | Tradeoffs, risks, follow-ups |

**Exam trap:** Alternatives + rationale must precede Consequences — defending a decision requires showing what was rejected and why.

## Translating Tradeoffs for Business Stakeholders
| Technical Metric | Business Translation |
|---|---|
| Benchmark score (MMLU, GPQA) | Avoid — meaningless to business audience |
| Token cost / request | Cost per resolved ticket / transaction |
| Response latency | User-facing wait time, SLA impact |
| Output quality variance | Customer satisfaction / escalation rate |

**Rule:** Always map technical change onto a KPI the stakeholder already tracks. Never lead with raw model benchmarks.

## Rollout Pattern Comparison
| Pattern | Traffic Shift | Risk Level | Use When |
|---|---|---|---|
| Shadow/Dark Launch | 0% (copy only) | None | Pure offline quality comparison |
| Canary | Gradual % (e.g. 5→25→100) | Low-Medium | Validating new model/prompt on real traffic |
| Blue-Green | 100% instantly | Medium (fast rollback) | Need fast, fully reversible full cutover |
| A/B Test | Split % (often 50/50) | Medium | Measuring business outcome between variants |
| Feature Flag | Per-segment on/off | Low | Targeted exposure control, instant kill switch |

## Deprecation Strategy Comparison
| Strategy | Speed | Consumer Impact |
|---|---|---|
| Hard Cutover | Fast | High — all consumers must migrate immediately |
| Shim/Adapter Layer | Slower | Low — legacy format translated during transition window |

**Exam trap:** Choose a shim strategy whenever external/enterprise consumers cannot migrate on your internal timeline.

## Semantic Versioning for Prompts/Agents
| Change Type | Version Bump | Example |
|---|---|---|
| Breaking change | MAJOR | v2.3.1 → v3.0.0 |
| Backward-compatible addition | MINOR | v2.3.1 → v2.4.0 |
| Bug fix only | PATCH | v2.3.1 → v2.3.2 |

## Post-Launch Lifecycle Checklist
1. Define quality baseline + drift alert thresholds BEFORE launch.
2. Monitor continuously (not just at launch) — cost, latency, quality signals.
3. Define rollback criteria in advance, not reactively.
4. Require dual sign-off (technical + business) for production prompt/agent changes.
5. Deprecation notices must include: timeline, affected consumers, migration path, rollback plan, business impact of inaction.

## Tradeoff Triangle
Cost ↔ Latency ↔ Quality — every migration/rollout decision memo must explicitly address all three, not optimize one in isolation.
