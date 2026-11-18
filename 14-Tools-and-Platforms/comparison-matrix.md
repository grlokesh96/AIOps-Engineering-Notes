# AI, AIOps, and Observability Tools — Comparison Matrix

## How to use this matrix
This is a shortlist framework, not a universal ranking. Capabilities, licensing, pricing, packaging, and regional availability change. Score candidates against your own workload, security requirements, and proof of concept.

## AIOps and observability platforms

| Tool | Primary category | Strong fit | Watch-outs |
|---|---|---|---|
| Prometheus | Metrics and alerting | Kubernetes and cloud-native metrics | Long-term storage and HA need design |
| Grafana | Visualization | Dashboards and exploration | Not a complete backend by itself |
| Loki | Log aggregation | Kubernetes/container logs | Label cardinality needs careful design |
| OpenTelemetry | Telemetry standard/tooling | Instrumentation and collection | Requires backend and pipeline operations |
| Jaeger | Distributed tracing | Trace collection and exploration | Storage and scaling design |
| Datadog | Full-stack observability | Managed metrics, logs, APM, cloud monitoring | Ingestion, retention, and add-on costs |
| Dynatrace | Full-stack observability/AIOps | Topology and automation in large environments | Licensing and implementation complexity |
| New Relic | APM/observability | Application performance and telemetry | Pricing and ingest controls |
| Splunk Observability | Observability | Enterprise telemetry and analytics | Total platform cost |
| ServiceNow ITOM | IT operations/event management | Service-centric enterprise operations | Fit depends on ServiceNow investment |

## Agent frameworks and integration

| Tool | Category | Useful for | Evaluate carefully |
|---|---|---|---|
| Google ADK | Agent framework | Structured agent development | Version compatibility and deployment |
| LangGraph | Stateful orchestration | State, branching, persistence, HITL | Graph and state complexity |
| OpenAI Agents SDK | Agent SDK | Tool use, handoffs, tracing | Provider ecosystem and requirements |
| LlamaIndex | Data/RAG framework | Retrieval-heavy applications | Retrieval quality and complexity |
| CrewAI | Multi-agent framework | Role/task-based prototypes | Coordination overhead |
| MCP | Tool/context protocol | Standardized tool/resource integration | Server trust, auth, and permissions |
| Temporal | Durable workflow engine | Long-running reliable workflows | Operational footprint |

## LLM platforms

| Platform | Hosting model | Good fit | Key checks |
|---|---|---|---|
| OpenAI API | Hosted | General-purpose model and agent applications | Data controls, availability, quotas |
| Anthropic Claude | Hosted | Reasoning and tool-use workloads | Data terms, limits, region |
| Google Gemini / Vertex AI | Hosted/managed cloud | Google Cloud and multimodal workloads | Developer API versus enterprise platform |
| Amazon Bedrock | Managed AWS | AWS-native model access and governance | Region/model availability and pricing |
| Azure AI Foundry | Managed Azure | Azure enterprise AI workloads | Deployment, quota, regional availability |
| Ollama | Local runtime | Developer workstation and local experiments | Hardware and production suitability |
| vLLM | Self-hosted inference | High-throughput serving | GPU capacity and model compatibility |

## Weighted selection scorecard

Score each candidate from 1 (poor) to 5 (excellent), then multiply by the weight.

| Criterion | Suggested weight | Questions |
|---|---:|---|
| Technical fit | 20% | Does it support required telemetry, tools, models, and workloads? |
| Security and governance | 20% | Can it enforce identity, access, residency, audit, and retention? |
| Reliability | 15% | Does it meet availability and recovery requirements? |
| Integration effort | 10% | How easily does it integrate with AWS/Azure/Kubernetes/GitHub? |
| Total cost | 15% | What is realistic 12-month cost at expected scale? |
| Operational complexity | 10% | Can the team operate and upgrade it? |
| Portability | 10% | Can data and workflows move to another provider? |

```text
weighted_score = sum((criterion_score / 5) * criterion_weight)
```

Record evaluator, date, product version, assumptions, and proof-of-concept evidence.

## Suggested stacks by scenario

### Cost-conscious Kubernetes lab
- Prometheus + Alertmanager
- Grafana
- Loki
- OpenTelemetry Collector
- Jaeger or Tempo
- FastAPI + LangGraph or Google ADK
- PostgreSQL + pgvector

### AWS-centric enterprise
- Amazon Bedrock
- CloudWatch and AWS-native telemetry
- OpenTelemetry where portability is required
- EKS with workload identity
- AWS Secrets Manager
- Durable workflow engine and separately secured tool gateway

### Azure-centric enterprise
- Azure AI Foundry
- Azure Monitor and Application Insights
- AKS and Microsoft Entra ID
- Key Vault
- OpenTelemetry instrumentation
- Existing enterprise approval and workflow systems

### Multi-cloud / vendor-neutral platform
- OpenTelemetry
- Prometheus-compatible metrics
- Grafana
- Loki or OpenSearch
- Jaeger or Tempo
- Provider-neutral model gateway
- MCP or typed REST tools with centralized policy enforcement

## Proof-of-concept checklist
- [ ] Use the same representative dataset and scenarios
- [ ] Test Kubernetes, CI/CD, cloud, and incident workflows
- [ ] Measure accuracy, evidence grounding, and false positives
- [ ] Measure p95 latency and throughput under load
- [ ] Calculate cost per successful task
- [ ] Test provider outage and rate limiting
- [ ] Test tenant isolation and access enforcement
- [ ] Test prompt injection in logs and retrieved documents
- [ ] Verify audit export and data deletion
- [ ] Document ownership and exit strategy

## Recommendation
Choose the smallest stack that meets reliability, security, and operational outcomes. For a learning and portfolio environment, OpenTelemetry + Prometheus + Grafana + Loki, paired with one agent framework and a constrained tool gateway, is a strong starting point. For enterprise adoption, run a workload-specific proof of concept and compare total cost, governance, support, and integration.

## Official documentation
- https://opentelemetry.io/docs/
- https://prometheus.io/docs/
- https://grafana.com/docs/
- https://docs.datadoghq.com/
- https://docs.dynatrace.com/
- https://google.github.io/adk-docs/
- https://langchain-ai.github.io/langgraph/
- https://modelcontextprotocol.io/
- https://docs.aws.amazon.com/bedrock/
- https://learn.microsoft.com/azure/ai-foundry/
