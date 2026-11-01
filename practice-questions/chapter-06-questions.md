## Chapter 6: Evaluation, Testing, and Optimization — Practice Questions

**Q1.** A team sends thousands of API calls per day that each begin with the same 5,000-token reference document followed by a short unique question. They want to cut input-token cost without changing models. Which approach BEST achieves this?

A) Switch to a smaller, cheaper model
B) Submit all requests through the Message Batches API
C) Lower max_tokens on every request
D) Place the static document first and enable prompt caching with a cache breakpoint after it

<details>
<summary>Answer</summary>

**Correct: D**

Prompt caching lets the static prefix be cached once; subsequent cache reads cost roughly 90% less than base input price, directly cutting cost for the repeated 5,000-token document.

**Why the others are wrong:**
- Changing model affects output quality and doesn't target the repeated-prefix cost problem
- Batches give a flat 50% discount for async bulk jobs but don't exploit the repeated static content the way caching does, and these calls are presumably synchronous
- max_tokens controls output length, not the cost of the repeated input prefix

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A company must classify 80,000 support tickets overnight with no real-time response requirement, and cost per token is the top priority. Which approach is BEST?

A) Message Batches API
B) Standard Messages API with prompt caching on the system prompt only
C) Standard Messages API with streaming enabled
D) Standard Messages API with max_tokens reduced

<details>
<summary>Answer</summary>

**Correct: A**

Message Batches API gives a flat 50% discount on input and output tokens for asynchronous, non-urgent workloads and supports up to 100,000 requests per batch, matching the 80,000-ticket volume.

**Why the others are wrong:**
- Caching helps repeated prefixes but doesn't provide the flat volume discount and still runs synchronously at standard per-call cost
- Streaming affects delivery latency, not per-token cost
- Reducing max_tokens only limits output length and doesn't provide a comparable cost reduction at this scale

</details>

---

**Q3.** During LLM-as-judge evaluation, a judge model consistently scores whichever response is listed FIRST higher, regardless of its actual content. What is this bias and the correct mitigation?

A) Verbosity bias — instruct the judge to prefer shorter answers
B) Position bias — randomize or swap the order of responses across trials and average the scores
C) Self-preference bias — replace the judge with a model from a different vendor
D) Length bias — truncate both responses to equal length before judging

<details>
<summary>Answer</summary>

**Correct: B**

This is classic position bias; swapping presentation order across repeated trials and averaging neutralizes the ordering effect.

**Why the others are wrong:**
- Verbosity bias concerns response length, not placement order
- Self-preference bias is about a judge favoring outputs from its own model family, unrelated to ordering
- Truncating length doesn't address an ordering-based bias

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** What is the minimum prefix length (in tokens) required to enable prompt caching on Claude Opus and Sonnet-class models?

A) 256 tokens
B) 2048 tokens
C) 1024 tokens
D) 4096 tokens

<details>
<summary>Answer</summary>

**Correct: C**

1024 tokens is the minimum cacheable prefix length for Opus/Sonnet-class models.

**Why the others are wrong:**
- 256 tokens is below the actual threshold
- 2048 tokens is the threshold for Haiku-class models, not Sonnet/Opus
- 4096 tokens is not a defined caching threshold

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** What is the default time-to-live (TTL) of an ephemeral prompt cache entry if it is not reused?

A) 1 minute
B) 1 hour
C) 24 hours
D) 5 minutes

<details>
<summary>Answer</summary>

**Correct: D**

The default ephemeral cache TTL is 5 minutes, refreshed on each cache hit; a 1-hour extended cache is available as a separate, higher-cost option.

**Why the others are wrong:**
- 1 minute is too short and not the actual default
- 1 hour is the extended cache option, not the default TTL
- 24 hours refers to the Message Batches processing window, not cache TTL

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Why should a held-out golden dataset be used instead of iteratively tuning prompts directly against the same evaluation set?

A) To increase the total number of labeled examples available
B) To reduce the human labeling effort required
C) To prevent overfitting prompt or agent changes to the eval set so improvements generalize to production traffic
D) To comply with API rate limits during testing

<details>
<summary>Answer</summary>

**Correct: C**

A held-out set that is never used for tuning ensures measured gains reflect true generalization rather than fitting to known test cases.

**Why the others are wrong:**
- Dataset size is unrelated to the overfitting concern
- Labeling effort doesn't change based on holding out data
- Rate limits are an infrastructure concern, not an evaluation-design reason

</details>

---

**Q7.** A new prompt version scores 82% pass rate versus 78% for the old version on the SAME 50-example golden set. What is the correct way to decide if this is a meaningful improvement?

A) Accept the new version since 82% is higher than 78%
B) Run a paired statistical test (e.g., McNemar's test) on the matched examples and check significance/confidence interval
C) Immediately expand the golden set to 50,000 examples before drawing any conclusion
D) Re-run the same 50 examples multiple times at temperature 0 to confirm the score

<details>
<summary>Answer</summary>

**Correct: B**

With a small paired sample, a proper paired significance test is needed to distinguish real improvement from noise.

**Why the others are wrong:**
- A raw 4-point difference on only 50 examples can easily be statistical noise
- Scaling the dataset to 50,000 is disproportionate as an immediate next step compared to applying the correct statistical test now
- Re-running at temperature 0 produces identical deterministic outputs and doesn't generate independent evidence for a significance test

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A team needs to debug why a multi-step agent regressed after a prompt change, including which tool call produced bad results and how long each step took. Which approach is BEST?

A) Log only the final output text of each run
B) Implement full agent trace logging capturing every tool call, its arguments, results, and per-step timestamps
C) Rely solely on the golden-set aggregate pass/fail score
D) Review Message Batches API logs for the affected requests

<details>
<summary>Answer</summary>

**Correct: B**

Full trace-level observability of tool calls and timing is required to pinpoint exactly where in a multi-step agent chain a regression originated.

**Why the others are wrong:**
- Final-output-only logging hides which intermediate step failed
- Aggregate pass/fail scores show that something broke but not where or why
- Batch API logs record request/response metadata, not internal agent tool-call traces

</details>

---

