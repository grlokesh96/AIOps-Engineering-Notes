# Tool Calling

## 1. What Is Tool Calling?

Tool calling allows an LLM to request a predefined function, API, or operation. The application executes that tool and returns its result to the model. The model does not inherently execute the function itself; the host application controls execution.

Tool calling can connect an agent to:
- Kubernetes API or safe `kubectl` wrappers
- Prometheus query endpoints
- Log search
- GitHub Actions APIs
- Cloud provider APIs
- Ticketing systems
- Internal runbooks

## 2. Tool-Calling Lifecycle

1. Application sends user task and available tool definitions to model.
2. Model returns a normal response or a tool request.
3. Application validates the tool name and arguments.
4. Authorization and policy checks run.
5. Application executes the tool.
6. Tool result is added to the conversation/workflow state.
7. Model interprets result and continues or completes.

## 3. Example Tool Schema

A read-only tool might accept a namespace and workload name:

```json
{
  "name": "get_pod_status",
  "description": "Retrieve status for a pod in an approved namespace",
  "parameters": {
    "type": "object",
    "properties": {
      "namespace": {"type": "string"},
      "pod_name": {"type": "string"}
    },
    "required": ["namespace", "pod_name"],
    "additionalProperties": false
  }
}
```

The application must still validate namespace allowlists, pod-name syntax, caller permissions, and response size. A schema is not an authorization system.

## 4. DevOps Tool Design

Prefer small, narrowly scoped tools over a generic shell tool.

| Tool | Purpose | Permission |
|---|---|---|
| `get_pod_status` | Read pod state | Read-only |
| `get_pod_events` | Retrieve events | Read-only |
| `get_container_logs` | Retrieve bounded logs | Read-only |
| `query_prometheus` | Run approved queries | Read-only |
| `get_deployment_history` | Inspect rollout history | Read-only |
| `create_incident_report` | Save report | Limited write |
| `request_rollback_approval` | Start approval flow | No direct rollout |

Avoid unrestricted commands such as `shell(command)` or `kubectl(command)` exposed directly to the model.

## 5. Tool Security

- Validate arguments server-side.
- Enforce authentication and authorization.
- Use least-privilege service accounts.
- Restrict namespaces and resources.
- Set timeouts and output limits.
- Sanitize or redact sensitive outputs.
- Treat tool results as untrusted data.
- Audit tool name, caller, arguments, result status, and timing.
- Prevent arbitrary URL access and command execution.
- Use approval gates for destructive actions.

## 6. Failure Handling

Tool errors should be structured, for example:

```json
{
  "status": "error",
  "error_code": "TIMEOUT",
  "retryable": true,
  "message": "Prometheus query timed out"
}
```

Do not expose internal credentials, stack traces, or sensitive infrastructure details to the model or user unless required and authorized.

## 7. Hands-on Lab

Build three read-only tools:
1. Get pod status
2. Get recent pod events
3. Get container logs

Test:
- Valid inputs
- Invalid namespace
- Missing pod
- Excessive log range
- API timeout
- RBAC denied

Then ask the agent to investigate a simulated failure and cite the tool evidence in its report.

## 8. Tool Calling vs. MCP

- **Tool/function calling:** A model-facing mechanism for requesting functions defined by the application.
- **MCP:** A protocol for exposing tools, resources, and other capabilities across compatible clients and servers.

MCP can be one way to connect an agent to tools, but it does not replace authorization, validation, or security controls.
