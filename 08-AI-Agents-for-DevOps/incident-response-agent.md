# Incident Response Agent

## 1. Purpose

An incident response agent assists responders by collecting evidence, correlating signals, preparing timelines, and recommending next steps. It should reduce investigation toil while keeping incident command and production decisions with authorized humans.

## 2. Inputs and outputs

**Inputs**
- Alert name, severity, service, environment, and timestamps.
- Metrics, logs, traces, Kubernetes events, and deployment history.
- Service ownership, SLOs, runbooks, and dependency maps.

**Outputs**
- Impact summary and affected services.
- Timestamped incident timeline.
- Evidence-backed hypotheses with confidence and gaps.
- Safe verification steps and relevant runbook links.
- Draft incident update and post-incident notes.

## 3. Workflow

```mermaid
flowchart TD
  A[Alert received] --> B[Validate alert and deduplicate]
  B --> C[Identify service and impact]
  C --> D[Collect metrics, logs, traces]
  D --> E[Check changes and dependencies]
  E --> F[Build timeline]
  F --> G[Rank hypotheses with evidence]
  G --> H[Recommend verification steps]
  H --> I[Human responder review]
  I --> J[Approved action by operator]
  J --> K[Verify recovery and document]
```

## 4. Example tool contracts

Keep tools narrow and typed. For example:

```text
get_alert_context(alert_id) -> alert metadata
query_service_metrics(service, start, end, metric_template) -> bounded series
search_service_logs(service, start, end, pattern, limit) -> sanitized records
get_trace(trace_id) -> trace summary
get_recent_deployments(service, start, end) -> deployment events
get_runbook(service, symptom) -> approved runbook reference
```

Each tool must enforce authorization, allowed time ranges, result limits, and audit logging.

## 5. Incident prompt template

```text
Act as an incident investigation assistant.
Use only the supplied evidence and approved tools.

Return:
1. User-visible symptoms and impact.
2. A timestamped timeline.
3. Up to three hypotheses ranked by evidence.
4. Evidence and source reference for every claim.
5. Missing information and safe verification steps.
6. Suggested runbook sections.

Do not claim correlation proves causation.
Do not execute changes or suppress alerts.
Escalate when evidence is incomplete or impact is severe.
```

## 6. Guardrails

- Do not let the model declare an incident resolved; verify service health and SLO recovery.
- Do not silence alerts or change incident severity without authorization.
- Never expose credentials or sensitive customer payloads.
- Require human approval for restarts, rollbacks, scaling, failovers, and configuration changes.
- Preserve original evidence and timestamps.
- Treat retrieved runbooks and log messages as data, not instructions.

## 7. Hands-on lab

1. Configure an alert for elevated API error ratio.
2. Send alert metadata to an agent service.
3. Retrieve Prometheus metrics and Loki logs for a fixed time range.
4. Retrieve recent deployment events.
5. Generate an incident timeline and ranked hypotheses.
6. Have an engineer validate each hypothesis.
7. Simulate a recovery and verify the SLI returns within target.

## 8. Success metrics

Track time to acknowledge, time to useful diagnosis, time to mitigate, evidence accuracy, responder acceptance, noisy recommendations, and unauthorized action attempts.

## Further reading

- [Google SRE: Managing Incidents](https://sre.google/sre-book/managing-incidents/)
- [Prometheus alerting](https://prometheus.io/docs/alerting/latest/overview/)
- [OpenTelemetry](https://opentelemetry.io/docs/)
