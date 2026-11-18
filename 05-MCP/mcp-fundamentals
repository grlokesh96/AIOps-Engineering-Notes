# Model Context Protocol (MCP) Fundamentals

## What is MCP?
MCP is an open protocol that standardizes how AI applications connect to external tools and context sources. It provides a common interface to Kubernetes, Prometheus, databases, source control, cloud APIs, and internal services. MCP does not define how an LLM reasons; it standardizes capability and context exchange.

## Core participants
| Component | Responsibility |
|---|---|
| MCP Host | AI application managing model interaction and MCP connections |
| MCP Client | Host component communicating with one MCP server |
| MCP Server | Program exposing tools, resources, and prompts |
| External system | Kubernetes, Prometheus, GitHub, databases, cloud APIs |

## Server capabilities
- **Tools:** Callable operations such as `get_pod_status`, `query_prometheus`, and `search_runbooks`.
- **Resources:** Retrievable context identified by URIs, such as runbooks and service catalogs.
- **Prompts:** Reusable templates for common tasks, such as incident investigation.

## Transports
- **stdio:** Common for a local server launched as a subprocess; communication uses standard input/output.
- **Streamable HTTP:** Used for network-accessible servers with HTTP-based communication and supported streaming behavior.

Check compatibility between the selected host, server, SDK, and transport.

## Typical interaction
1. Host connects to an MCP server.
2. Client discovers server capabilities.
3. Client lists tools, resources, or prompts.
4. The model may request a tool call.
5. Client sends the request to the server.
6. Server validates and authorizes the operation.
7. Server calls the external system and returns a result.
8. Host makes the result available to the model and presents the answer.

Lifecycle details are protocol-version-specific. Use the current specification rather than copying outdated handshake examples.

## MCP vs API vs function calling
| Technology | Purpose |
|---|---|
| REST/gRPC API | Application-to-application interface |
| Model function/tool calling | Model requests a function defined by its host application |
| MCP | Standardized protocol for hosts/clients to discover and use external tools and context |

An MCP server can wrap an existing API; it does not replace that API.

## DevOps/AIOps use cases
- Query Kubernetes workload health.
- Retrieve pod events and bounded logs.
- Query Prometheus metrics.
- Inspect GitHub Actions failures.
- Retrieve Terraform plan summaries.
- Search incident runbooks.
- Create tickets after approval.

## Security principles
- Connect only to trusted MCP servers.
- Use least-privilege credentials.
- Validate tool inputs on the server.
- Restrict namespaces, accounts, and environments.
- Keep secrets out of prompts and tool outputs.
- Treat retrieved content as untrusted data.
- Require approval for high-impact actions.
- Audit tool invocation and authorization decisions.

## Hands-on lab
Design an MCP server exposing two read-only tools: `get_pod_status(namespace, pod_name)` and `query_prometheus(query_name, time_range)`. Start with mock data. Test schemas, invalid inputs, access denial, timeouts, and output redaction.

## References
- https://modelcontextprotocol.io/docs/learn/architecture
- https://modelcontextprotocol.io/specification/
- https://github.com/modelcontextprotocol/python-sdk
