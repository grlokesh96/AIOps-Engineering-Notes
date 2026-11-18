# MCP Servers

## What is an MCP server?
An MCP server is a program that exposes capabilities to compatible MCP clients. It may wrap a local process, remote API, database, monitoring platform, or internal service.

A server can expose **tools** (callable operations), **resources** (retrievable context), and **prompts** (reusable interaction templates).

## Common server categories
| Category | Example capabilities |
|---|---|
| Source control | Search repositories, inspect pull requests and workflow runs |
| Filesystem | Read permitted files and directory listings |
| Database | Read schema and execute constrained queries |
| Kubernetes | Inspect workloads, events, rollout status |
| Observability | Query metrics, logs, traces, alerts |
| Cloud | Inspect resource inventory and configuration |
| ITSM | Search incidents and create approved tickets |

A server appearing in a registry does not mean it is secure or production-ready. Review source, maintainer, permissions, releases, and threat model.

## Choosing a server
1. Is it official, vendor-maintained, or community-maintained?
2. Is the repository active?
3. Which protocol versions and transports are supported?
4. Which permissions are requested?
5. Does it expose read and write tools?
6. Can tools be disabled or allowlisted?
7. Does it log sensitive data?
8. Can it be pinned to a reviewed version?
9. Is it production-ready or only a demo?

## Local stdio configuration concept
A host commonly needs a command, arguments, working directory, controlled environment, and secure secret injection. Exact configuration is host-specific.

```json
{
  "mcpServers": {
    "example": {
      "command": "python",
      "args": ["server.py"],
      "env": {"APP_MODE": "development"}
    }
  }
}
```
Do not assume this format works in every host. Keep credentials in a secret manager or approved environment injection mechanism.

## Remote server considerations
For Streamable HTTP deployments: use TLS; authenticate clients and authorize each operation; validate origins/hosts where applicable; rate-limit requests; apply request/response limits; protect against SSRF and unsafe URL access; monitor availability and errors.

## Operational checklist
- [ ] Trusted source and pinned version
- [ ] Documented tools and schemas
- [ ] Least-privilege identity
- [ ] Separate environment credentials
- [ ] Input validation
- [ ] Timeouts and rate limits
- [ ] Secret redaction
- [ ] Audit logging
- [ ] Security testing
- [ ] Upgrade and rollback plan

## Lab
Run a local example server from a trusted source in a disposable environment. Inspect its tools before use. Test invalid input and verify it cannot access files or systems outside its scope.

## References
- https://modelcontextprotocol.io/docs/learn/server-concepts
- https://github.com/modelcontextprotocol/servers
- https://registry.modelcontextprotocol.io/
