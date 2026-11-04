## Chapter 8: Stakeholder Communication and Lifecycle Management — Practice Questions

**Q1.** A fintech company's engineering team wants to migrate from an older Claude model generation to a newer one. The business stakeholders only ask: 'Will this cost more and will it feel different to users?' Which approach BEST translates the technical migration decision for this audience?

A) Present the full benchmark suite (MMLU, GPQA, HumanEval scores) comparing both models in raw numbers
B) Tell them the migration is purely an engineering decision and does not require business sign-off
C) Postpone the conversation until after the migration is complete to avoid confusing them with technical details
D) Frame the decision as a cost-per-resolved-ticket and latency-per-request delta, tied to the KPIs the business already tracks

<details>
<summary>Answer</summary>

**Correct: D**

Business stakeholders think in terms of outcomes they already measure (cost per unit of work, user-facing latency). Mapping model-level tradeoffs onto existing business KPIs is the core translation skill tested in this domain.

**Why the others are wrong:**
- Raw benchmark scores mean nothing without context and will not answer the stakeholder's actual question about cost and user experience
- Model migrations that change cost or behavior always require business sign-off, especially in regulated industries like fintech
- Withholding the conversation until after launch violates change-management practice and removes the stakeholder's ability to approve risk

</details>

---

**Q2.** An architect is writing an Architecture Decision Record (ADR) to justify switching from a single large prompt to a multi-agent orchestration pattern. Which section should appear immediately after the decision statement, and BEFORE listing consequences?

A) The alternatives considered and the rationale for rejecting each one against stated criteria
B) A list of every alternative ever considered, with no evaluation criteria
C) A changelog of all future planned agent updates
D) The rollback plan for reverting to the single-prompt design

<details>
<summary>Answer</summary>

**Correct: A**

Standard ADR structure is: Title/Status → Context → Decision → Alternatives Considered (with rationale) → Consequences. Alternatives with rationale must precede consequences so the reader understands why this option won before reading tradeoffs.

**Why the others are wrong:**
- Listing alternatives without evaluation criteria provides no defensible rationale and fails the 'defending decisions' exam objective
- A changelog belongs in a separate versioning/roadmap artifact, not inline in the ADR's core decision logic
- Rollback plans belong under Consequences or a separate runbook, not ahead of the alternatives analysis

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A production agent built on an older Claude model generation is scheduled for deprecation. The team has 10,000 cached prompt templates tuned to that model's response style. Which migration strategy BEST minimizes business risk during the transition?

A) Hard cutover: switch 100% of traffic to the new model version on the deprecation date
B) Freeze the deprecated model's endpoint indefinitely by self-hosting weights to avoid any migration work
C) Ask end users to manually select which model version they want per request
D) Run both model versions in parallel behind a routing layer, shift traffic incrementally while comparing output quality against defined acceptance thresholds

<details>
<summary>Answer</summary>

**Correct: D**

Phased/canary migration with a routing layer and quality gates lets the team validate the new model against real traffic and roll back safely — the standard pattern for model-version migration planning tested in this domain.

**Why the others are wrong:**
- Hard cutover on a deprecation date with no validation window maximizes risk if the new model's output quality or format differs
- Self-hosting Anthropic model weights is not possible since Claude is only available via API/managed services, making this option technically invalid
- Pushing the decision to end users abandons the architect's responsibility for a controlled, measurable rollout

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which deployment pattern involves routing a small percentage of production traffic (e.g., 5%) to a new model or prompt version, monitoring quality metrics, and only increasing the percentage if thresholds are met?

A) Canary release
B) Blue-green deployment
C) Feature flagging
D) Shadow traffic testing

<details>
<summary>Answer</summary>

**Correct: A**

Canary release is defined by gradually increasing the percentage of live traffic routed to the new version based on monitored success criteria.

**Why the others are wrong:**
- Blue-green deployment switches all traffic at once between two fully separate environments, it does not use incremental percentages
- Feature flagging toggles functionality on/off per user segment but is not inherently percentage-based or tied to gradual rollout monitoring
- Shadow traffic testing sends a copy of real traffic to the new version without serving its responses to users, so no live percentage is 'rolled out'

</details>

---

**Q5.** A team wants to test a new system-prompt version against production traffic without any risk of it affecting real user responses, purely to compare output quality offline. Which technique should they use?

A) Canary release at 1% of traffic
B) Shadow traffic testing (dark launch)
C) Immediate full rollout with a feature flag kill switch
D) A/B testing with user-visible variant labels

<details>
<summary>Answer</summary>

**Correct: B**

Shadow/dark-launch testing duplicates live requests to the new prompt version and logs the output for comparison, without ever serving that output to the user — zero user-facing risk.

**Why the others are wrong:**
- Canary release at 1% still exposes real users to the new version's responses, which contradicts the 'no effect on real users' requirement
- A feature flag kill switch mitigates risk but still serves the new version live before any output is validated
- A/B testing with visible variant labels is user-facing by design and is the opposite of a risk-free offline comparison

</details>

---

**Q6.** A company's production agent uses prompt templates versioned as v2.3.1. The team adds a new optional output field without breaking any existing integrations. Following semantic versioning conventions for prompt/agent artifacts, what should the new version number be?

A) v3.0.0, because any change requires a major version bump
B) v2.3.2, because it is treated as a bug fix
C) v2.3.1-beta, because untested changes must stay in beta indefinitely
D) v2.4.0, because it is a backward-compatible, additive change

<details>
<summary>Answer</summary>

**Correct: D**

Under semantic versioning, backward-compatible additive changes (new optional field, new capability) bump the MINOR version. MAJOR is reserved for breaking changes, PATCH for bug fixes.

**Why the others are wrong:**
- A major bump is reserved for breaking changes that require consumers to update their integration, which is not the case here
- A patch bump implies a fix to existing broken behavior, not the addition of new functionality
- There is no requirement to keep additive, non-breaking changes in beta indefinitely; that blocks legitimate incremental delivery

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** What is the key architectural difference between a 'hard cutover' deprecation strategy and a 'backward-compatibility shim/adapter layer' strategy when retiring an older model or API version?

A) A hard cutover maintains the old interface indefinitely, while a shim removes it immediately
B) There is no meaningful difference; both terms describe the same migration approach
C) A shim strategy is only applicable to database schema changes, not model or API versioning
D) A hard cutover immediately removes the old version and forces all consumers to update at once, while a shim translates old-format requests/responses so legacy consumers keep working during a transition period

<details>
<summary>Answer</summary>

**Correct: D**

A hard cutover forces an immediate, all-at-once transition with no compatibility layer. A shim/adapter decouples legacy consumers from the new interface by translating calls, extending the effective deprecation window and reducing business disruption.

**Why the others are wrong:**
- This reverses the definitions — a hard cutover is the one that removes the old interface immediately
- The two terms represent distinct, exam-tested risk tradeoffs (speed vs. compatibility) and are not interchangeable
- Shims are a general software architecture pattern applicable to any interface change, including model/API versioning, not just databases

</details>

---

**Q8.** Six months after launching an agentic workflow, the architect notices gradual quality drift in production outputs with no code or prompt changes deployed. Which post-launch practice should have caught this earliest?

A) A one-time acceptance test performed before launch
B) Relying solely on customer support ticket volume as the quality signal
C) Continuous monitoring of output quality metrics (e.g., sampled human/automated eval scores, user feedback signals) against a baseline, with alerting on drift
D) Annual architecture review meetings

<details>
<summary>Answer</summary>

**Correct: C**

Post-launch lifecycle management requires ongoing, automated quality monitoring with a defined baseline and alerting thresholds, since model behavior, underlying data distributions, or usage patterns can drift even without explicit changes.

**Why the others are wrong:**
- A one-time pre-launch test cannot detect drift that occurs after launch
- Support ticket volume is a lagging, noisy indicator and will catch drift far later than direct quality monitoring
- Annual reviews are far too infrequent to catch gradual quality degradation before it causes material business impact

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

