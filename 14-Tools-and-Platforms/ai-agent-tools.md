# AI Agent Tools and Frameworks

## Overview
Production agents combine a model API, orchestration framework, typed tools, retrieval, policy enforcement, evaluation, and observability.

## Frameworks and SDKs

| Tool | Main purpose | Good fit |
|---|---|---|
| Google ADK | Agent development kit, tools, workflow patterns | Google ecosystem and structured agent applications |
| LangGraph | Stateful graph-based workflows | Explicit state, branching, persistence, human approval |
| OpenAI Agents SDK | Agents, tools, handoffs, tracing | Applications using OpenAI models and supported integrations |
| Microsoft Semantic Kernel / Agent Framework | Agent and orchestration ecosystem | Microsoft and .NET/Python enterprise integrations |
| LlamaIndex | Data ingestion, retrieval, agentic RAG | Knowledge-heavy agents |
| CrewAI | Role/task-oriented multi-agent workflows | Prototyping coordinated specialist agents |
| Haystack | Search, RAG, and agent pipelines | Retrieval-centric applications |

Framework APIs evolve. Check official docs for supported versions and production status.

## Tool integration
- MCP (Model Context Protocol): standardized tools, resources, and prompts.
- REST/GraphQL APIs: direct integration with explicit schemas.
- Function/tool calling: model requests structured invocation; application validates and executes it.
- Webhooks and event buses: event-driven triggers.
- Temporal and workflow engines: durable execution, retries, and long-running tasks.

## Supporting components

| Component | Options | Role |
|---|---|---|
| API layer | FastAPI, Flask | Request validation and service endpoints |
| Validation | Pydantic, JSON Schema | Typed inputs and outputs |
| Queue | Redis Streams, RabbitMQ, Kafka, cloud queues | Async jobs and backpressure |
| State | PostgreSQL, Redis | Durable task state and caching |
| Vector retrieval | pgvector, Qdrant, Weaviate, Milvus, Chroma | Semantic retrieval |
| Secrets | AWS Secrets Manager, Azure Key Vault, HashiCorp Vault | Credential storage and rotation |
| Evaluation | Ragas, DeepEval, promptfoo | Quality and regression evaluation |
| Telemetry | OpenTelemetry, Langfuse, Arize Phoenix | Tracing and agent observability |

## Example DevOps tool contract

```python
from pydantic import BaseModel, Field

class PodQuery(BaseModel):
    namespace: str = Field(min_length=1, max_length=63)
    pod_name: str = Field(min_length=1, max_length=253)

def get_pod_status(query: PodQuery) -> dict:
    """Return selected, read-only status fields for an authorized Pod."""
    # Validate caller scope, query Kubernetes with read-only identity,
    # normalize response, and never return Secret data.
    raise NotImplementedError
```

Prefer narrow functions over unrestricted shell commands, arbitrary HTTP requests, or general-purpose cloud credentials.

## Build versus buy
Build custom code for unique policy, domain logic, or controlled tool boundaries. Prefer managed services for commodity identity, queues, databases, model hosting, and telemetry when they satisfy requirements.

## Evaluation checklist
- Are workflows inspectable and testable?
- Is state durable across worker restarts?
- Are retries, deadlines, and cancellation supported?
- Can tools be restricted per user and tenant?
- Is human approval available for sensitive actions?
- Are model and tool calls traced?
- Can providers be changed without rewriting business logic?
- Is there an active maintenance and security process?

## References
- https://google.github.io/adk-docs/
- https://langchain-ai.github.io/langgraph/
- https://openai.github.io/openai-agents-python/
- https://modelcontextprotocol.io/
- https://docs.temporal.io/
