## Chapter 4: Solution Design I: Model Selection and Agent Architectures — Practice Questions

**Q1.** A company handles 2 million customer support queries per month. 90% are simple FAQ lookups; 10% require multi-step reasoning and occasional escalation. The team needs to minimize cost and latency without sacrificing quality on hard cases. Which architecture BEST meets these requirements?

A) Route every query through Opus to guarantee maximum quality
B) Use a fast/cheap model tier for first-pass classification and simple responses, escalating only ambiguous or complex queries to a higher tier
C) Build a multi-agent system with an Opus orchestrator and Opus subagents for every query
D) Use a single mid-tier model for all queries regardless of complexity

<details>
<summary>Answer</summary>

**Correct: B**

Tiered routing (cascade pattern) matches model cost/latency to task complexity — cheap tier absorbs the 90% volume, expensive tier handles only the hard 10%, minimizing total cost and latency.

**Why the others are wrong:**
- Routing everything through the highest tier maximizes cost and latency for queries that don't need it
- Multi-agent orchestration with the most expensive tier adds unnecessary coordination overhead and cost for simple queries
- A single mid-tier model doesn't optimize cost for high-volume simple queries nor guarantee quality on the hardest 10%

</details>

---

**Q2.** An architect is designing a research report generator that must synthesize findings across dozens of documents with deep multi-step reasoning. Latency is not a constraint; output quality is the top priority. Which configuration BEST fits this requirement?

A) The fastest/cheapest model tier with extended thinking disabled
B) The highest-capability model tier with extended thinking enabled and an appropriately large budget
C) A balanced mid-tier model with extended thinking disabled to save cost
D) A multi-agent system using only the cheapest tier for all subagents to parallelize work

<details>
<summary>Answer</summary>

**Correct: B**

When latency is not a constraint and quality is the top priority, the highest-capability tier with extended thinking enabled produces the deepest reasoning, which is exactly what this task requires.

**Why the others are wrong:**
- The cheapest/fastest tier is optimized for speed and volume, not deep multi-step reasoning quality
- A mid-tier model without extended thinking sacrifices the reasoning depth the scenario explicitly prioritizes
- Using only the cheapest tier across subagents optimizes for cost/speed, not for the stated quality priority

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A pipeline always executes the same fixed sequence: fetch data, validate it, transform it, then store it. The sequence never changes regardless of intermediate results. Which architecture should be used?

A) An autonomous single agent with tool use
B) A multi-agent orchestrator with subagents
C) A deterministic workflow
D) An agent configured with maximum extended thinking budget

<details>
<summary>Answer</summary>

**Correct: C**

When the control flow is fixed and known in advance, a deterministic workflow is cheaper, more predictable, and easier to test and debug than giving an agent autonomy it doesn't need.

**Why the others are wrong:**
- Autonomy is only valuable when the next step depends on unpredictable intermediate results — not the case here
- Multi-agent orchestration adds coordination overhead with no benefit when there's no parallel, independent decomposition
- Extended thinking budget addresses reasoning depth, not control-flow predictability, and adds unnecessary cost here

</details>

---

**Q4.** A task requires researching 5 unrelated topics in parallel, each needing its own isolated context, with results merged into one final report. Which architecture is the BEST fit?

A) A single agent making sequential tool calls across all 5 topics
B) A multi-agent system with an orchestrator spawning one subagent per topic and aggregating results
C) A deterministic workflow with hardcoded steps for each topic
D) A single high-volume model tier processing all topics in one batch request

<details>
<summary>Answer</summary>

**Correct: B**

Independent, parallelizable subtasks that each need isolated context are the canonical justification for multi-agent/subagent orchestration — each subagent avoids polluting a shared context window.

**Why the others are wrong:**
- Sequential single-agent calls would bloat one context window with 5 unrelated topics and lose parallelism
- A deterministic workflow doesn't fit because the research content per topic is dynamic, not a fixed procedural sequence
- Model tier selection doesn't address the architectural need for isolated, parallel context

</details>

---

**Q5.** A real-time voice assistant needs sub-second response latency for simple intent classification. Which model tier should be selected?

A) The fastest/lowest-latency tier suited to lightweight tasks
B) The highest-capability tier, for maximum accuracy
C) A balanced mid-tier model
D) The highest-capability tier with extended thinking disabled

<details>
<summary>Answer</summary>

**Correct: A**

Sub-second latency requirements for a simple classification task map directly to the fastest, lowest-cost model tier, which is purpose-built for exactly this kind of lightweight, high-speed workload.

**Why the others are wrong:**
- The highest-capability tier trades latency for reasoning depth that this simple task doesn't need
- A mid-tier model is a plausible but suboptimal choice — it still adds latency the task doesn't need to pay for
- Disabling extended thinking on the top tier still leaves the base latency/cost of the highest tier, which is unnecessary here

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** An agent's output feeds directly into an automated downstream pipeline that requires strict schema conformance with zero parsing failures. Which output pattern should be used?

A) Free-form text output parsed downstream with regular expressions
B) The raw extended thinking trace as the final answer
C) Schema-constrained structured output (e.g., tool/function-call based output)
D) Multiple agents voting on free-text answers and picking a majority response

<details>
<summary>Answer</summary>

**Correct: C**

Schema-constrained structured output guarantees the response conforms to a defined format the downstream system can reliably consume, eliminating brittle parsing failures.

**Why the others are wrong:**
- Regex parsing of free text is brittle and breaks whenever model phrasing varies slightly
- Extended thinking output is reasoning scratch space, not a reliable final structured answer
- Majority voting on free-text answers doesn't guarantee schema conformance and adds unnecessary cost/complexity

</details>

---

**Q7.** An architect configures the maximum extended thinking budget for every request type, including simple classification tasks, and observes high cost and latency with no quality improvement. What is the correct fix?

A) Always maximize the thinking budget for every request to guarantee the best possible accuracy
B) Extended thinking budget has no effect on cost, so leave it as configured
C) Scale the extended thinking budget to task complexity — use larger budgets only for genuinely complex reasoning tasks
D) Disable extended thinking entirely across the entire production system

<details>
<summary>Answer</summary>

**Correct: C**

Extended thinking budget should scale with task complexity; applying a large budget uniformly wastes cost and latency on tasks that gain no quality benefit from deeper reasoning.

**Why the others are wrong:**
- Maximizing budget everywhere is the exact misconfiguration causing the problem described
- Thinking budget directly affects token usage and therefore cost and latency
- Disabling it entirely is an overcorrection that removes quality gains needed for genuinely complex tasks

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary factor that determines whether multi-agent orchestration adds genuine value over a single-agent design?

A) Multi-agent designs always outperform single-agent designs because they use more total compute
B) Multi-agent systems are generally cheaper to run than a single agent performing the same task
C) The task decomposes into independent, parallelizable subtasks that benefit from isolated context windows
D) Multi-agent orchestration is required whenever extended thinking is enabled

<details>
<summary>Answer</summary>

**Correct: C**

Multi-agent value comes from genuine task decomposition into independent, parallelizable units that each need their own clean context — not from complexity or compute alone.

**Why the others are wrong:**
- More compute usage doesn't inherently mean better outcomes, and can indicate wasted spend from unnecessary orchestration
- Multi-agent systems typically cost MORE than single-agent due to per-subagent token usage plus orchestration overhead
- Extended thinking and multi-agent orchestration are independent architectural decisions with no required dependency

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

