# Node Failure Investigation

## 1. Purpose

Node incidents can affect many workloads at once. Investigate node conditions, capacity, kubelet/runtime health, networking, storage, and provider-level events before selecting a mitigation.

## 2. Initial commands

```bash
kubectl get nodes -o wide
kubectl describe node <node>
kubectl get pods -A --field-selector spec.nodeName=<node> -o wide
kubectl get events -A --sort-by=.lastTimestamp
kubectl top nodes
```

If authorized, inspect node-level services through the platform's approved access method. Do not assume SSH or direct host access is available.

## 3. Important node conditions

| Condition | Meaning / investigation |
|---|---|
| Ready=False | Kubelet or node health issue; inspect node conditions and provider status |
| Ready=Unknown | Control plane has not received node status; inspect connectivity and kubelet |
| MemoryPressure | Node memory is constrained |
| DiskPressure | Node filesystem or image storage pressure |
| PIDPressure | Process ID capacity pressure |
| NetworkUnavailable | Node networking setup may be incomplete |

## 4. Investigation workflow

1. Confirm affected node and incident time.
2. List workloads scheduled on the node and their criticality.
3. Inspect node conditions, taints, capacity, and allocatable resources.
4. Check pod eviction, scheduling, and volume events.
5. Review kubelet, container runtime, CNI, and CSI health using approved platform telemetry.
6. Correlate node metrics with workload symptoms.
7. Review node pool changes, upgrades, autoscaler activity, and cloud provider events.
8. Recommend cordon, drain, replacement, or escalation only after disruption analysis.

## 5. Cordon and drain: operator-only examples

These commands change cluster state and are not read-only. Run only with the correct approval, maintenance plan, and disruption checks.

```bash
kubectl cordon <node>
kubectl drain <node> --ignore-daemonsets
```

A drain may be blocked by PodDisruptionBudgets or may disrupt workloads if options are chosen carelessly. Review local runbooks and Kubernetes version behavior. Do not use force or delete-emptydir-data flags casually.

## 6. AI agent design

Expose read-only tools for:
- Node object and condition summary.
- Pods scheduled on a node.
- Recent node and pod events.
- Node and workload resource metrics.
- Node pool and cloud event metadata.

The agent should return:
- Affected workloads and estimated blast radius.
- Condition changes and timeline.
- Supporting metrics/events.
- Missing evidence.
- Recommended checks and operator decision points.

Do not expose node SSH, cloud termination, cordon, or drain actions to the diagnostic agent.

## 7. Hands-on lab

1. Use a disposable cluster or node pool.
2. Generate a controlled node pressure condition or simulate unavailable capacity.
3. Observe node conditions and pod scheduling events.
4. Gather metrics and node pool change history.
5. Generate an AI report and verify its timeline.
6. Practice a documented cordon/drain workflow manually in a safe environment.

## 8. Safety considerations

- Check PodDisruptionBudgets and workload replica health.
- Identify stateful workloads and local storage.
- Verify spare capacity before draining.
- Consider topology spread and zone-level failure.
- Keep cloud-provider and cluster autoscaler actions auditable.
- Never terminate a node based solely on an LLM recommendation.

## Further reading

- [Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/)
- [Safely drain a node](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/)
- [Node pressure eviction](https://kubernetes.io/docs/concepts/scheduling-eviction/node-pressure-eviction/)
