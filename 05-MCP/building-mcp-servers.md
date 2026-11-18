# Building MCP Servers with Python

## Goal
Build a small MCP server that exposes safe, structured tools. Start with mock or read-only data before connecting cloud or production systems.

## Prerequisites
The official MCP Python SDK documentation currently lists Python 3.11+ and a CLI extra. Confirm the SDK version and examples before implementation because APIs can change between major versions.

```bash
python3 --version
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install "mcp[cli]"
```

## Project structure
```text
devops-mcp/
├── .env
├── .gitignore
├── server.py
└── README.md
```

`.gitignore`:
```gitignore
.env
.venv/
__pycache__/
```

## Minimal Python server example
This illustrates the FastMCP programming model used by the Python SDK. Check current SDK documentation for exact import paths, decorators, and run commands for your installed version.

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("devops-readonly")

@mcp.tool()
def get_service_health(service: str) -> dict:
    """Return demo health information for an allowlisted service."""
    allowed = {"frontend", "api", "worker"}
    if service not in allowed:
        return {"status": "error", "message": "Service is not allowlisted"}
    # Demo only. Replace with an authorized read-only backend query.
    return {"service": service, "health": "unknown", "source": "demo"}

if __name__ == "__main__":
    mcp.run()
```

Do not treat a demo server as production-ready. Add authentication, authorization, validation, logging, limits, and tests before deployment.

## Tool design checklist
- Clear name and description
- Typed arguments
- Input validation and authorization checks
- Bounded execution time and response size
- Structured output and safe errors
- Audit logging

Prefer `get_pod_status(namespace, pod_name)` over `run_kubectl(command)`.

## Add resources or prompts
Tools perform operations. Resources expose retrievable context. Prompts expose reusable templates. Add them when useful and supported by the SDK version. Potential DevOps resources include `runbook://kubernetes/crashloopbackoff`, `service://catalog/api`, and `dashboard://service/api/overview`. Avoid secrets or unrestricted filesystem paths.

## Test with MCP Inspector
MCP Inspector is a development tool for inspecting server capabilities and testing tool calls. Follow current instructions: https://modelcontextprotocol.io/docs/tools/inspector

Test tool discovery, valid/invalid arguments, missing resources, exceptions, large responses, and unauthorized access.

## Local vs remote deployment
- **stdio:** Useful for a local server launched by an MCP host.
- **Streamable HTTP:** Useful for a remote service accessed by multiple clients.

For remote deployment, add TLS, authentication, authorization, request limits, origin/host protections, observability, and secure secret management.

## Production security
- Never trust model-generated arguments.
- Do not expose arbitrary shell execution.
- Use least-privilege identities.
- Separate read and write capabilities.
- Require approval for impactful changes.
- Protect credentials and redact tool output.
- Treat logs and retrieved text as untrusted.
- Record caller, tool, target, decision, and result.
- Pin dependencies and scan the server image.

## Hands-on project: DevOps MCP server
Build `get_pod_status` (read-only Kubernetes status), `get_pod_events` (bounded events query), `query_prometheus` (allowlisted metrics query), and `search_runbook` (approved runbook retrieval). Test against a local cluster and Prometheus instance. Connect the server to an agent and produce an evidence-backed incident report.

## References
- https://github.com/modelcontextprotocol/python-sdk
- https://py.sdk.modelcontextprotocol.io/
- https://modelcontextprotocol.io/docs/tools/inspector
- https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices
