# Cost Optimization for Production AI Agents

## Objective

Reduce total cost per successful task without compromising correctness, security, or reliability. Optimize the complete system—not only LLM token prices.

## Cost components

| Component | Cost drivers |
|---|---|
| LLM inference | Input/output tokens, model tier, retries |
| Embeddings and reranking | Document volume, query volume, re-indexing |
| Retrieval | Vector/database compute, storage, network |
| Tools and APIs | Cloud API usage, observability query volume |
| Agent runtime | CPU, memory, replicas, queue workers |
| Data platform | Logs, traces, metrics, retention |
| Human review | Approval and exception handling time |

## Optimization sequence

1. Measure cost per successful task and per tenant.
2. Remove duplicate prompts and repeated context.
3. Retrieve only relevant evidence and cap result counts.
4. Use compact structured outputs.
5. Route simple tasks to lower-cost models after evaluation.
6. Cache safe, reusable results with clear expiry and access scope.
7. Limit agent iterations, tool calls, retries, and maximum output tokens.
8. Batch offline work where latency permits.
9. Tune worker concurrency and autoscaling.
10. Review retention and telemetry cardinality.

## Cost formula

```text
total_task_cost =
  model_input_cost
  + model_output_cost
  + embedding_cost
  + retrieval_cost
  + tool_api_cost
  + allocated_runtime_cost
  + human_review_cost
```

```python
def llm_cost(input_tokens: int, output_tokens: int,
             input_price_per_million: float,
             output_price_per_million: float) -> float:
    return (
        input_tokens * input_price_per_million
        + output_tokens * output_price_per_million
    ) / 1_000_000
```

Keep price data versioned with provider, model, currency, effective date, and source URL.

## Practical controls

### Context management

- Use metadata filters before vector search.
- Deduplicate retrieved chunks.
- Prefer concise runbook excerpts over entire documents.
- Summarize long histories once and preserve source references.
- Set maximum context size and reject oversized requests.

### Caching

Cache deterministic outputs such as parsed metadata, normalized events, and stable embeddings. Cache model answers only when the task, data permissions, model version, and freshness requirements allow it. Include tenant and authorization scope in cache keys to prevent cross-tenant leakage.

### Agent budgets

```yaml
agent_budget:
  max_steps: 8
  max_tool_calls: 12
  max_input_tokens: 20000
  max_output_tokens: 2000
  max_wall_time_seconds: 90
  max_cost_usd: 0.20
```

Enforce budgets in code at every step. A prompt telling the model to be economical is not a budget control.

## FinOps dashboard

Recommended metrics:
- Cost per successful task
- Cost by agent, task type, tenant, model, and environment
- Tokens per task and average context size
- Retry and fallback cost
- Cache hit ratio
- Tool calls per task
- Human approval rate
- Quality score alongside cost

A lower average token cost is not a win if task success or safety declines.

## Experiment design

Run controlled A/B evaluations for model choice, retrieval top-k, chunk size, prompt length, and output budget. Compare quality, latency, and total cost on the same dataset. Keep a rollback path for any optimization that harms task quality.

## Cloud runtime optimization

For Kubernetes-hosted agents, right-size CPU and memory, scale workers based on queue depth and latency, use separate pools for CPU-heavy ingestion and model orchestration, and apply requests/limits. Avoid scaling solely on CPU when the main bottleneck is provider latency or rate limits.

## Anti-patterns

- Choosing the cheapest model without quality evaluation
- Unlimited agent loops
- Repeatedly attaching entire conversation history
- Caching confidential responses across tenants
- Retrying non-retryable errors
- Hiding cost by excluding embeddings, retrieval, or runtime
- Optimizing token cost while increasing human rework

## References

- https://platform.openai.com/docs/guides/latency-optimization
- https://aws.amazon.com/aws-cost-management/
- https://learn.microsoft.com/azure/cost-management-billing/
