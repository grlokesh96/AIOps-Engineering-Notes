# Google ADK — Tools

## 1. Tool concept
Tools are functions or integrations the agent may request. The application/runtime executes the tool and returns the result. The model does not inherently execute the code.

Potential tools:
- Kubernetes read-only query functions
- Prometheus query wrapper
- Log search
- GitHub workflow lookup
- Cloud inventory APIs
- Runbook search
- Ticket creation

## 2. Example Python tool

```python
def summarize_service_status(service_name: str) -> dict:
    """Return status for a service in an approved allowlist."""
    allowed = {"frontend", "api", "worker"}
    if service_name not in allowed:
        return {"error": "Service is not allowed"}

    # Replace this stub with a real, authorized query.
    return {
        "service": service_name,
        "status": "unknown",
        "source": "demo"
    }
```

Register tools using the supported mechanism for your ADK version.

## 3. Tool design
- Narrow and purpose-specific
- Typed input/output
- Server-side validation
- Independent authorization
- Timeout and output limits
- Structured errors
- Secret redaction
- Safe audit logs

## 4. Read-only Kubernetes tool ideas

| Tool | Purpose |
|---|---|
| `get_pod_status` | Pod state, readiness, restarts |
| `get_pod_events` | Recent events |
| `get_pod_logs` | Bounded log retrieval |
| `get_deployment_state` | Replica and rollout status |
| `query_prometheus` | Approved metric query |

Use a dedicated service account with least-privilege RBAC and restrict namespaces.

## 5. Example structured result

```json
{
  "tool": "get_pod_status",
  "status": "success",
  "namespace": "test",
  "pod": "api-abc123",
  "phase": "Running",
  "ready": false,
  "restart_count": 3
}
```

## 6. Lab
Implement a read-only query tool and test valid input, invalid namespace, missing resource, timeout, denied access, and excessive output. Ensure the agent reports results truthfully.

## 7. Security checklist
- [ ] Allowlisted tool names
- [ ] Typed argument schema
- [ ] Server-side validation
- [ ] Authorization checks
- [ ] Least privilege
- [ ] Timeouts and output limits
- [ ] Safe errors
- [ ] Audit trail
- [ ] No secrets in model context
