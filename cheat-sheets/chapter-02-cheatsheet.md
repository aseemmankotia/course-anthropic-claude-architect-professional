## Chapter 2 Cheat Sheet — Integration: APIs, Middleware, MCP Server Design

### MCP Core Primitives
| Primitive | Purpose | Side Effects? | Example |
|---|---|---|---|
| Resource | Expose contextual/read-only data | No | Product catalog, file contents |
| Tool | Callable function | Yes (often) | place_order, create_ticket |
| Prompt | Reusable interaction template | No | "summarize_ticket" template |

### MCP JSON-RPC Methods
| Category | Discover | Use |
|---|---|---|
| Tools | tools/list | tools/call |
| Resources | resources/list | resources/read |
| Prompts | prompts/list | prompts/get |

### Transports
| Transport | Use Case |
|---|---|
| stdio | Local subprocess, single client (desktop) |
| Streamable HTTP | Remote, multi-client, enterprise deployments |

### Claude API stop_reason Values
| Value | Meaning |
|---|---|
| end_turn | Model finished naturally |
| max_tokens | Hit output token limit |
| stop_sequence | Hit a defined stop sequence |
| tool_use | Model wants to call a tool (check content block) |

### Middleware Patterns — When to Use
| Pattern | Purpose | Exam Trigger Phrase |
|---|---|---|
| Gateway | Centralize auth/logging/throttling across MANY backend services | "single entry point for multiple APIs" |
| Adapter | Convert ONE interface/protocol to another | "SOAP to REST", "legacy protocol wrapper" |
| Anti-Corruption Layer (ACL) | Translate domain model/semantics between legacy and modern system | "legacy field names/formats don't match" |

### Least-Privilege Tool Design Checklist
- Narrow, single-purpose tools (NOT generic "call_any_operation")
- Explicit JSON Schema: required fields, types, enums
- Read-only operations exposed as Resources, not Tools, when no side effect exists
- Backend permissions scoped per-tool, not per-server
- Input validated at the MCP server boundary before hitting legacy system

### Backpressure & Rate Limiting Toolkit
- Token-bucket / leaky-bucket limiter at the MCP server
- Request queue with bounded size
- Circuit breaker to fail fast when downstream is unhealthy
- Exponential backoff + jitter on retryable errors (e.g., 429, 5xx)

### Common Exam Traps
- Choosing a generic catch-all tool over narrowly-scoped tools (violates least privilege)
- Confusing Adapter (interface translation) with ACL (domain/semantic translation)
- Using stdio transport for a remote, multi-user enterprise deployment
- Treating Prompts as if they can perform actions (they cannot — that's a Tool's job)
- Skipping input_schema validation and relying only on tool descriptions
