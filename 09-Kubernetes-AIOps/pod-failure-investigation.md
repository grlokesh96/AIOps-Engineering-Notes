# Pod Failure Investigation

## 1. Purpose

This guide provides a structured method for investigating pod lifecycle and container failures. Pod status is a symptom; events, container state, logs, resource metrics, and dependencies provide the evidence needed to understand it.

## 2. Quick inspection commands

```bash
kubectl get pods -n <namespace> -o wide
kubectl describe pod <pod> -n <namespace>
kubectl get events -n <namespace> --sort-by=.lastTimestamp
kubectl logs <pod> -n <namespace> --all-containers --tail=200
kubectl logs <pod> -n <namespace> --all-containers --previous --tail=200
kubectl get pod <pod> -n <namespace> -o yaml
```

Be careful when sharing YAML: it can contain sensitive environment values or configuration references.

## 3. Failure-state playbook

### Pending
Check:
- Scheduler events and unschedulable reasons.
- Node capacity and allocatable resources.
- CPU/memory requests, taints, tolerations, affinity, and selectors.
- PVC binding, namespace quota, and admission policy.

### CrashLoopBackOff
Check:
- Previous container logs and termination exit code.
- OOMKilled or other termination reason.
- Liveness/readiness/startup probes.
- Application configuration and dependency connectivity.
- Recent image or deployment changes.

### ImagePullBackOff / ErrImagePull
Check:
- Image name, tag, digest, and architecture.
- Registry reachability and image existence.
- `imagePullSecrets` and service account references.
- Registry rate limits and authentication errors.

Never ask an agent to retrieve or print secret values.

### OOMKilled
Check:
- Container memory limit and working-set trend.
- Traffic increase, memory leak indicators, and recent changes.
- Pod restarts and node memory pressure.
- Whether requests/limits reflect measured workload needs.

### CreateContainerConfigError
Check:
- Referenced ConfigMaps and Secrets exist.
- Required keys and volume mounts are present.
- Service account and admission configuration.

### Failed probes
Check:
- Probe path, port, scheme, timeout, and thresholds.
- Application startup time and dependency readiness.
- Whether the probe tests liveness or readiness appropriately.

## 4. AI agent workflow

1. Receive namespace and pod identifier.
2. Verify caller authorization and resource existence.
3. Fetch pod status, owner references, events, and bounded logs.
4. Retrieve relevant container metrics.
5. Inspect the owning Deployment/StatefulSet and recent rollout.
6. Classify the failure state.
7. Produce facts, hypotheses, and read-only verification steps.
8. Escalate uncertain or high-impact cases to an engineer.

## 5. Example structured output

```text
Pod: checkout-7c9d... / production
State: CrashLoopBackOff
Observed:
- Container restarted 6 times in 10 minutes.
- Previous termination reason: OOMKilled.
- Memory working set approached the configured limit.
Hypothesis:
- Container memory limit may be insufficient, or memory use is growing unexpectedly.
Verification:
- Compare memory trend with request rate and previous release.
- Inspect heap/profile data if available.
No resource changes were made.
```

## 6. RBAC considerations

Grant only the resources required for investigation, such as pods, pod logs, events, and owning workload objects. Avoid broad cluster-admin access and Secret reads. Validate effective permissions with `kubectl auth can-i`.

## 7. Hands-on lab

1. Deploy a test application with a deliberately incorrect probe.
2. Observe pod events and status.
3. Capture current and previous logs.
4. Correct the probe and verify recovery.
5. Repeat with a controlled memory limit issue in a disposable namespace.
6. Compare an AI-generated report with a human diagnosis.

## 8. Guardrails

- Never delete a pod to hide the symptom before collecting evidence.
- Bound log volume and redact sensitive fields.
- Do not execute `kubectl exec` or arbitrary shell commands through the agent.
- Require human approval for resource changes.
- Verify rollout and SLO health after any approved remediation.

## Further reading

- [Debugging Pods](https://kubernetes.io/docs/tasks/debug/debug-application/)
- [Pod lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)
