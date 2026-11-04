## Chapter 10: Developer Productivity and Operational Enablement — Practice Questions

**Q1.** A platform team wants every engineer at the company to automatically trust the same set of MCP servers and the same baseline tool permissions, regardless of what each developer configures locally. Which mechanism should they use?

A) Ask each developer to manually copy a shared .mcp.json into their home directory
B) A Slack message instructing developers to run the same claude mcp add commands
C) Enterprise managed settings/policy files deployed by IT that take precedence over user and project settings
D) A shared CLAUDE.md file with a comment asking developers not to change MCP config

<details>
<summary>Answer</summary>

**Correct: C**

Enterprise managed policy settings are deployed centrally (e.g., by IT/MDM) and sit at the top of the settings precedence hierarchy, overriding CLI args, project, and user settings — the only mechanism that guarantees org-wide enforcement rather than voluntary compliance.

**Why the others are wrong:**
- Manual copying is not enforced, doesn't scale, and drifts over time
- Slack instructions are not enforced and are not auditable
- CLAUDE.md is a memory/context file, not a settings or permissions enforcement mechanism

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Which file type is correct for defining a reusable subagent that a team wants every repo contributor to have access to by default when they clone the repo?

A) ~/.claude/agents/*.md (user-level agent)
B) CLAUDE.md with an ## Agents heading
C) .mcp.json with an agents array
D) .claude/agents/*.md committed to the repository (project-level agent)

<details>
<summary>Answer</summary>

**Correct: D**

Project-level subagents live in .claude/agents/ inside the repo and are shared via version control, so anyone who clones the repo gets them automatically; project-level agents also take precedence over user-level agents of the same name.

**Why the others are wrong:**
- User-level agents are personal to one developer's machine and not shared via the repo
- CLAUDE.md is a memory/instruction file, not the mechanism for defining subagents
- .mcp.json configures MCP servers, not subagents

</details>

---

**Q3.** An organization has 40 teams independently writing their own CLAUDE.md files with inconsistent conventions, causing agents to behave unpredictably across repos. What is the BEST rollout strategy to fix this at scale?

A) Have the platform team publish a shared CLAUDE.md template/convention plus an enterprise-level base memory file, and let teams extend it with project-specific sections
B) Mandate that every team delete their CLAUDE.md and rely only on default model behavior
C) Disable CLAUDE.md support company-wide via enterprise settings
D) Ask each team lead to review every other team's CLAUDE.md manually before merge

<details>
<summary>Answer</summary>

**Correct: A**

A layered approach — enterprise/org-level conventions plus a shared template that project CLAUDE.md files extend — gives consistency where it matters while still allowing project-specific context, which is the standard platform-team pattern for scaling Claude Code adoption.

**Why the others are wrong:**
- Deleting CLAUDE.md removes valuable project context and hurts productivity
- Disabling the feature entirely eliminates the benefit of shared memory altogether
- Manual cross-team review doesn't scale to 40 teams and isn't a repeatable process

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A company wants developers across all teams to discover and reuse an internal 'deploy-to-staging' MCP server without each team reinventing it. What should the platform team build?

A) A per-developer local .mcp.json that each engineer configures manually from memory
B) A single enterprise CLAUDE.md file containing the full source code of the MCP server
C) An internal MCP server catalog with documented capabilities, scopes, and onboarding instructions, referenced from a central internal developer portal
D) A company-wide email announcing the server's existence once

<details>
<summary>Answer</summary>

**Correct: C**

A discoverable, documented MCP catalog (capabilities, auth scope, how to add it) is the standard operational-enablement pattern — it lets teams self-serve and reduces duplicated effort, which is exactly what MCP catalogs are for.

**Why the others are wrong:**
- Manual per-developer config from memory doesn't scale and leads to drift/errors
- Embedding source code in CLAUDE.md conflates memory context with infrastructure config and bloats context
- A one-time email isn't durable or discoverable after the fact

</details>

---

**Q5.** Which activity is a responsibility of a PLATFORM team enabling Claude Code adoption, rather than an individual developer?

A) Defining and maintaining the enterprise-level managed settings, shared skill libraries, and MCP catalog used across teams
B) Writing project-specific business logic inside a single repo's CLAUDE.md
C) Choosing which files to open in an individual coding session
D) Deciding personal keyboard shortcuts in their local CLI

<details>
<summary>Answer</summary>

**Correct: A**

Platform teams own cross-cutting, org-wide infrastructure — enterprise settings, shared skills/subagents, and MCP catalogs — so individual teams don't have to reinvent them; this is the core platform-vs-developer split tested on the exam.

**Why the others are wrong:**
- Project-specific business logic in CLAUDE.md is an individual team/developer responsibility
- File selection in a session is an individual developer action
- Personal CLI preferences are individual, not platform-owned

</details>

---

**Q6.** During an incident, an autonomous coding agent pushed a change that broke a production build because it had write access to the main branch. Which artifact should the platform team have had in place to shorten recovery time?

A) A longer CLAUDE.md with more coding style rules
B) A documented operational runbook defining detection, containment (e.g., revoke credentials/branch protection), rollback steps, and escalation path for agent-caused incidents
C) A larger context window configuration
D) An additional subagent dedicated to writing unit tests

<details>
<summary>Answer</summary>

**Correct: B**

Operational runbooks for agent incidents define concrete detection, containment, and rollback steps so teams respond consistently and quickly — this is the operational enablement artifact specifically tested, distinct from preventive coding conventions or unrelated config.

**Why the others are wrong:**
- CLAUDE.md governs behavior during normal operation, not incident response procedure
- Context window size is unrelated to incident response process
- A test-writing subagent is a prevention measure, not an incident runbook

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A VP asks the platform team to report on the business impact of the Claude Code rollout after six months. Which combination of metrics BEST measures developer productivity impact?

A) Only the number of CLAUDE.md files created across repositories
B) Lead time for changes, PR cycle time, and developer-reported time saved on repetitive tasks, tracked before and after rollout
C) The total token count consumed by all Claude Code sessions that month
D) The number of MCP servers registered in the internal catalog

<details>
<summary>Answer</summary>

**Correct: B**

Productivity impact should be measured with delivery/outcome metrics (lead time, cycle time, self-reported time savings) compared to a pre-rollout baseline — these connect tool adoption to actual business value.

**Why the others are wrong:**
- Counting CLAUDE.md files measures adoption artifacts, not productivity outcomes
- Token consumption is a cost/usage metric, not a productivity outcome
- Number of catalog entries measures platform build-out, not developer impact

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary purpose of packaging a repeatable workflow as an Agent Skill rather than leaving the instructions only inside a single developer's CLAUDE.md?

A) Skills execute faster than any instruction in CLAUDE.md
B) Skills package a capability (instructions plus any supporting files) as a discrete, shareable, versionable unit that can be reused across projects and teams
C) Skills are automatically encrypted while CLAUDE.md is not
D) Skills remove the need for any MCP server configuration

<details>
<summary>Answer</summary>

**Correct: B**

Agent Skills bundle instructions and resources into a self-contained, reusable unit that can be distributed and versioned across teams, unlike instructions buried in one project's CLAUDE.md which aren't portable or discoverable elsewhere.

**Why the others are wrong:**
- Skills have no inherent execution speed advantage over CLAUDE.md instructions
- Encryption is not a defining or guaranteed property of Skills
- Skills and MCP servers are complementary, not substitutes — a skill can still call MCP tools

</details>

---

