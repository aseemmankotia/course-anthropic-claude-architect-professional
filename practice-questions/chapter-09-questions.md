## Chapter 9: Claude Models, Prompting, and Context Engineering at Scale — Practice Questions

**Q1.** A production support bot classifies incoming tickets into 5 categories using a short prompt, needs sub-second latency, and runs millions of times per day. Which model family selection BEST fits this production requirement?

A) Opus, because classification accuracy matters most
B) Haiku, because it is optimized for fast, cheap, high-volume simple tasks
C) Sonnet with extended thinking enabled for consistency
D) Opus with a reduced max_tokens to lower cost

<details>
<summary>Answer</summary>

**Correct: B**

Haiku is positioned for high-throughput, low-latency, cost-sensitive tasks like simple classification where deep reasoning isn't required.

**Why the others are wrong:**
- Opus is the slowest and most expensive model family — overkill for a 5-category classifier at millions of calls/day.
- Extended thinking adds latency and cost and is meant for multi-step reasoning, not simple classification.
- Reducing max_tokens on Opus doesn't fix the core latency/cost mismatch of using the largest model for a trivial task.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An enterprise agent must plan a multi-step financial audit involving cross-referencing numbers across several documents before producing a final recommendation. Which configuration is the BEST fit?

A) Haiku with a large max_tokens value
B) Any model with thinking disabled but a longer system prompt
C) Opus with thinking enabled but budget_tokens set to the documented minimum
D) Sonnet or Opus with extended thinking enabled and an adequate budget_tokens value

<details>
<summary>Answer</summary>

**Correct: D**

Multi-step reasoning across cross-referenced data benefits from extended thinking with a sufficient token budget on a capable model (Sonnet/Opus).

**Why the others are wrong:**
- Haiku is tuned for speed/cost, not deep multi-step reasoning quality.
- A longer system prompt does not substitute for the model's internal step-by-step reasoning process that extended thinking enables.
- Setting budget_tokens to the bare minimum constrains the reasoning depth needed for a complex cross-referencing task.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A long-running enterprise agent conversation is approaching the model's context window limit after many turns of tool calls and results. What is the BEST context engineering strategy?

A) Stop the conversation and start a brand-new session with no prior context
B) Keep sending the full raw transcript every time and let the API truncate automatically
C) Switch to a smaller model so more tokens are 'available'
D) Periodically summarize/compact older turns into a condensed memory block while retaining recent turns verbatim

<details>
<summary>Answer</summary>

**Correct: D**

Compaction/summarization of older history while preserving recent turns verbatim is the standard pattern for managing context window limits in long-running agents.

**Why the others are wrong:**
- Starting a new session loses important task state and continuity needed for the agent's ongoing work.
- Relying on automatic truncation risks losing critical early instructions or facts unpredictably.
- Switching model size does not change the underlying context window limit tied to the conversation's accumulated tokens.

</details>

---

**Q4.** Which model family is positioned as the default balanced choice for most production agentic workloads requiring a mix of reasoning quality, latency, and cost?

A) Haiku
B) Opus
C) Sonnet
D) Whichever model has the largest context window

<details>
<summary>Answer</summary>

**Correct: C**

Sonnet is designed as the balanced, general-purpose production default between Haiku (speed/cost) and Opus (maximum capability/cost).

**Why the others are wrong:**
- Haiku trades reasoning depth for speed and cost, making it unsuitable as the general default for mixed workloads.
- Opus is the highest-cost, highest-latency option, reserved for tasks requiring maximum reasoning capability.
- Context window size is not the deciding factor for family selection; it is independent of the Opus/Sonnet/Haiku tradeoff.

</details>

---

**Q5.** A downstream system requires guaranteed valid JSON matching a strict schema from every Claude response, with no free-text deviation. Which approach BEST enforces this?

A) Use a forced tool_choice pointing at a single tool whose input schema defines the required JSON structure
B) Add instructions in the system prompt asking the model to 'always respond in JSON'
C) Lower the temperature to 0 and hope the formatting is consistent
D) Parse the response text with a regex and discard anything that isn't valid JSON

<details>
<summary>Answer</summary>

**Correct: A**

Forcing tool_choice to a specific schema-defining tool makes the model's input conform to that schema, giving reliable structured output for downstream consumption.

**Why the others are wrong:**
- Prompt-only instructions are a soft constraint and can still produce malformed or extra text around the JSON.
- Temperature affects randomness, not structural guarantees — malformed output can still occur.
- Regex-based filtering discards failures rather than preventing them, which isn't acceptable for production reliability.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** What is the minimum value that must be set for the thinking budget (budget_tokens) when enabling extended thinking?

A) 0 tokens
B) 256 tokens
C) 4096 tokens
D) 1024 tokens

<details>
<summary>Answer</summary>

**Correct: D**

Anthropic's extended thinking requires a minimum budget_tokens value of 1024 tokens; lower values are rejected.

**Why the others are wrong:**
- 0 would disable thinking entirely, not configure a valid budget.
- 256 is below the documented minimum and is not an accepted value.
- 4096 is a reasonable working budget but not the documented minimum floor.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which tool_choice configuration forces the model to call exactly one named tool rather than letting it decide freely or call any tool?

A) tool_choice: "auto"
B) tool_choice: "any"
C) tool_choice: "none"
D) tool_choice: {"type": "tool", "name": "<tool_name>"}

<details>
<summary>Answer</summary>

**Correct: D**

Specifying the tool type with a name forces the model to invoke that exact tool, which is the pattern used for structured-output enforcement.

**Why the others are wrong:**
- "auto" lets the model freely choose whether and which tool to call.
- "any" forces some tool call but allows the model to pick which one, not a specific named tool.
- "none" disables tool use entirely for that request.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** In context engineering for multi-turn enterprise agents, what distinguishes 'compaction' from simply retrieving relevant documents via RAG?

A) Compaction and RAG are the same technique with different names
B) Compaction condenses the agent's own accumulated conversation/tool history to fit the context window, while RAG retrieves external knowledge relevant to the current query
C) Compaction only applies to system prompts, while RAG only applies to user messages
D) RAG is used exclusively with Haiku, while compaction is used exclusively with Opus

<details>
<summary>Answer</summary>

**Correct: B**

Compaction manages the growing internal conversation/tool-call history, while RAG supplies external facts/documents on demand — both are complementary context engineering techniques.

**Why the others are wrong:**
- They solve different problems (internal history size vs. external knowledge injection) and are not interchangeable.
- Compaction and RAG are not restricted to specific prompt components in this way.
- Neither technique is tied to a specific model family; both can be used with any Claude model.

</details>

---

