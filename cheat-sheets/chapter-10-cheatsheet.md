## Chapter 10 Cheat Sheet: Developer Productivity & Operational Enablement (7%)

### Settings Precedence (highest wins)
| Rank | Source |
|---|---|
| 1 | Enterprise managed policy (IT-deployed) |
| 2 | Command line arguments |
| 3 | Local project settings (.claude/settings.local.json) |
| 4 | Shared project settings (.claude/settings.json) |
| 5 | User settings (~/.claude/settings.json) |

**Exam rule:** Only enterprise managed policy guarantees org-wide enforcement. Everything else is voluntary/overridable by the developer.

### CLAUDE.md Scope Layers
| Layer | Location | Shared via |
|---|---|---|
| Enterprise | IT-managed base memory file | Central deployment |
| Project | ./CLAUDE.md | Git (committed) |
| User | ~/.claude/CLAUDE.md | Personal, all projects |

**Rollout pattern:** Enterprise base + shared template → teams extend with project-specific sections. Never delete CLAUDE.md to fix inconsistency — standardize it instead.

### Subagents vs Skills vs MCP Servers
| Artifact | Location | Shared scope | Purpose |
|---|---|---|---|
| Subagent | .claude/agents/*.md | Project (commit) or user (~/.claude/agents) | Specialized task delegate; project-level overrides user-level |
| Agent Skill | SKILL.md + supporting files | Packaged, versionable, reusable across projects | Reusable capability/workflow package |
| MCP server | .mcp.json (project) or local/user config | Project (committed) vs personal | External tool/data integration |

**Exam rule:** Commit subagents/.mcp.json to repo for team-wide sharing; user-scope configs are personal only.

### Internal MCP Catalog — What It Must Provide
- Discoverable list of approved servers
- Documented capabilities and auth scope
- Onboarding/add instructions
- Central reference (internal dev portal)

### Operational Runbook for Agent Incidents — Required Sections
1. **Detection** — how the incident is flagged
2. **Containment** — revoke credentials, branch protection, pause agent
3. **Rollback** — revert changes, restore state
4. **Escalation** — who is notified, in what order

### Platform Team vs Individual Developer
| Platform team owns | Individual developer owns |
|---|---|
| Enterprise settings/policy | Project-specific CLAUDE.md business logic |
| Shared skill/subagent libraries | Personal CLI preferences |
| MCP catalog & onboarding docs | File/session choices during work |

### Measuring Productivity Impact — Use Outcome Metrics
- Lead time for changes (before vs after rollout)
- PR / change cycle time
- Developer-reported time saved on repetitive tasks
- **Avoid vanity metrics:** token consumption, # of CLAUDE.md files, # of catalog entries — these measure usage/build-out, not impact.

### Common Exam Traps
- Treating CLAUDE.md as an **enforcement** mechanism (it's memory/context, not policy — enterprise settings enforce).
- Fixing inconsistent conventions by **disabling** CLAUDE.md instead of standardizing it.
- Measuring rollout success with usage counts instead of delivery/outcome metrics.
- Confusing Skills (portable packaged capability) with CLAUDE.md content (project/user context, not portable).
