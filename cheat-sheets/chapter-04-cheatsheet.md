# Chapter 4 Cheat Sheet: Model Selection & Agent Architectures

## Model Tier Decision Table
| Tier | Optimize For | Use When | Avoid When |
|---|---|---|---|
| Opus | Max reasoning quality | Complex, high-value, low-volume tasks; deep synthesis | High-volume, latency-sensitive, simple tasks |
| Sonnet | Balanced quality/cost/speed | Default production workload | Extreme cost-sensitivity at huge scale OR max-reasoning requirement |
| Haiku | Speed and cost | High-volume, simple classification/response tasks | Deep multi-step reasoning needs |

**Exam rule:** quality, cost, and latency form a 3-way tradeoff — you cannot optimize all three simultaneously. Scenario keywords ("millions of requests," "sub-second," "deep research," "no latency constraint") signal which corner of the triangle to optimize.

## Architecture Decision Framework
| Signal in Scenario | Architecture |
|---|---|
| Fixed, known sequence of steps | Deterministic workflow |
| Next step depends on unpredictable results | Autonomous single agent |
| Independent, parallelizable subtasks needing isolated context | Multi-agent / subagent orchestration |
| Task is merely "complex" but linear | Single agent (NOT multi-agent by default) |

**Exam trap:** "complexity" alone does not justify multi-agent. The real trigger is independent, parallelizable decomposition that benefits from isolated context windows.

## Model Tiering (Cascade) Pattern
- Route bulk/simple traffic through cheapest/fastest tier first.
- Escalate only ambiguous/complex cases to a higher tier.
- Minimizes total cost and latency across large volumes while preserving quality on hard cases.

## Extended Thinking Budget
- Controlled via a thinking token budget parameter.
- Scale budget to task complexity — simple classification needs little to none; deep multi-step synthesis benefits from larger budgets.
- **Exam trap:** maxing the budget uniformly across all request types wastes cost/latency with no quality gain.

## Structured Output for Downstream Systems
- Use schema-constrained / tool-call based structured output.
- Avoid free-text output parsed via regex — brittle and failure-prone.
- Required whenever output feeds an automated pipeline expecting strict format conformance.

## Multi-Agent Cost Reality
- Multi-agent = orchestrator cost + sum of subagent costs + aggregation overhead.
- Almost always MORE expensive than single-agent for the same task.
- Justify multi-agent only by parallelism/quality/context-isolation gains — never by "it's cheaper."

## Quick Gotcha List
1. "Multi-agent is cheaper" → FALSE, it's an overhead-adding pattern.
2. "Always use the top-tier model for best results" → FALSE, ignores cost/latency fit.
3. "Max extended thinking budget always = best answer" → FALSE, wastes cost on simple tasks.
4. "Fixed pipeline = needs an autonomous agent" → FALSE, use a deterministic workflow.
