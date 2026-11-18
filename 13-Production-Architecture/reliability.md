# Reliability Engineering for AI Agents

## Objective

Make AI agent services predictable under provider outages, tool failures, malformed model output, noisy telemetry, and unexpected inputs. Reliability includes both service availability and the correctness of the result.

## Failure domains

| Failure | Expected behavior |
|---|---|
| LLM provider timeout | Bounded retry, approved fallback, or safe failure |
| Rate limiting | Backoff, queue, and concurrency reduction |
| Tool/API outage | Circuit breaker and partial result where safe |
| Invalid model output | Schema validation and bounded repair attempt |
| Retrieval unavailable | Continue only if task policy permits without retrieval |
| Queue/database outage | Reject or pause work; do not lose task state |
| Malicious or malformed input | Validate, isolate, and audit |
| Budget exhausted | Stop safely and report incomplete analysis |

## Reliability patterns

### Timeouts and deadlines

Every request, tool call, retrieval query, and model call needs a timeout. Propagate a single end-to-end deadline so retries do not extend work indefinitely.

### Retries

Retry only transient failures. Use exponential backoff with jitter and a maximum attempt count. Do not retry invalid requests, authorization failures, or non-idempotent mutations automatically.

### Circuit breakers

Open a circuit after a configured failure threshold, reject or defer calls during the open interval, then test recovery with limited half-open traffic.

### Bulkheads

Separate worker pools and concurrency budgets for different providers and tools. A failing log backend should not consume capacity needed for critical Kubernetes investigations.

### Idempotency

Give every task and mutating action an idempotency key. Persist action state so a worker restart does not accidentally repeat a disruptive operation.

## Graceful degradation

Examples:
- If the LLM is down, return deterministic evidence collection and rule-based findings.
- If vector search is down, use a small approved static runbook set if allowed.
- If one telemetry backend is unavailable, clearly label the evidence gap.
- If a budget is reached, stop further model calls and return partial results.

Never present partial analysis as complete.

## SLOs and error budgets

Define service-level indicators (SLIs) such as:
- Successful task completion rate
- p95 end-to-end latency
- Evidence retrieval success
- Valid structured output rate
- Tool-call success rate
- Cost per successful task

Example initial SLOs (illustrative; tune to your use case):
- 99.5% of accepted requests complete or return a clear controlled failure within the deadline.
- 95% of interactive investigations return within 30 seconds.
- 99% of successful reports contain traceable evidence references.

Do not define an SLO that rewards fast but incorrect answers. Include quality and evidence-grounding measures.

## Evaluation and regression tests

Maintain a versioned evaluation set containing:
- Known incidents and expected evidence
- Ambiguous cases where the correct response is uncertainty
- Prompt injection in logs and retrieved documents
- Missing and contradictory telemetry
- Tool permission denials
- Provider timeouts and malformed output
- Out-of-scope user requests

Run evaluations for every prompt, model, retrieval, and tool change. Use deterministic tests for policy and authorization; use scored evaluation for probabilistic reasoning.

## Chaos and resilience testing

In a non-production environment, simulate provider timeouts, `429` responses, slow tool APIs, queue backlog, database failover, and worker termination. Verify bounded recovery, no duplicate actions, correct audit records, and clear operator communication.

## Operational runbooks

Create runbooks for provider outage, queue backlog, model quality regression, unexpected spend, retrieval corruption, credential rotation, and suspected prompt injection. Include owners, escalation paths, safe-mode procedures, and rollback instructions.

## Observability

Propagate trace IDs through API, orchestrator, model gateway, tool calls, and storage. Record model name/version, prompt version, token counts, latency, and outcome, but avoid logging secrets or full sensitive prompts. Alert on SLO burn rate, queue age, provider errors, and abnormal cost.

## References

- https://sre.google/sre-book/monitoring-distributed-systems/
- https://sre.google/workbook/alerting-on-slos/
- https://opentelemetry.io/docs/
