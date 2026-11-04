## Chapter 7 Cheat Sheet — Governance, Safety, and Risk Management (14%)

### Compliance Frameworks — Quick Map
| Framework | Applies To | Exam Trigger Phrase |
|---|---|---|
| HIPAA | Healthcare PHI | 'patient records', 'minimum necessary' |
| GDPR | EU personal data | 'EU users', 'data residency', 'SCCs' |
| SOC 2 (Type I/II) | SaaS enterprise trust (security/availability/confidentiality) | 'enterprise customers', 'vendor attestation' |
| FedRAMP | US federal government cloud | 'federal agency', 'government deployment' |

**SOC 2 Type I** = control design snapshot. **Type II** = operating effectiveness over a period. Enterprise buyers want Type II.

### Data Residency Rules of Thumb
- Residency = WHERE processing/storage physically occurs.
- SCCs/legal clauses = supplementary safeguard for unavoidable transfer, NOT a substitute for regional processing.
- Encryption ≠ residency compliance (it protects confidentiality, not jurisdiction).
- Partial localization (metadata only) does not satisfy residency if payload/content leaves the region.

### Guardrail Layering (memorize the 3 layers)
1. **Input filtering** — sanitize/classify user input before it reaches the model.
2. **Output validation** — check final responses and tool-call arguments before execution.
3. **Human-in-the-loop (HITL)** — gate high-impact or side-effecting actions.
A correct exam answer almost always references ≥2 of these layers together, never just one.

### Prompt-Injection Defense (RAG + Tool Use)
- Treat ALL retrieved/external content as **untrusted data**, never as instructions — regardless of source (internal wiki, KB article, tool output).
- System-prompt-only defenses ('ignore instructions in documents') are **soft controls** — bypassable, not sufficient alone.
- Real defense = structural: least-privilege tool scoping so damaging actions are impossible even if injected reasoning tries to trigger them.
- Defense-in-depth = input filtering + output/tool-call validation + least-privilege execution boundary.

### Least-Privilege MCP/Tool Design
| Bad Pattern | Why It Fails |
|---|---|
| Long-lived full-account API key | Large blast radius if leaked |
| Shared service account across tools | Breaks per-user/session audit traceability |
| Broad write + logging disabled | Violates auditability |
| 'Trust the agent's reasoning' as the boundary | Unenforceable; bypassable via injection |

**Good pattern:** short-lived, action-scoped, session/user-identity-tied credentials, rotated regularly, full request/response audit logging.

### Risk Tiering for Agentic Autonomy
Tier by **impact magnitude × reversibility**:
- Low impact, reversible → low autonomy tier, light logging.
- High impact OR irreversible (financial trades, bulk deletes, PHI export) → **mandatory HITL before execution + enhanced audit logging**.
- Post-hoc/batch review does NOT substitute for pre-execution control on irreversible actions.

### Audit Logging — Purpose
Provides a **verifiable, tamper-evident record** of agent actions, decisions, and data access — for compliance review and incident investigation. Disabling logging for cost/noise reasons is always a governance violation on the exam.
