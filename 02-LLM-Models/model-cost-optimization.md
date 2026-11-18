# LLM Model Cost Optimization

## 1. Why AI costs grow

LLM cost is commonly driven by:
- Input tokens.
- Output tokens.
- Model tier.
- Context size.
- Tool-call loops and retries.
- Repeated context.
- Concurrency and throughput.
- Hosted service or infrastructure costs.

An agent that calls tools repeatedly or sends entire logs and runbooks can cost more than a model's advertised token rate suggests.

## 2. Basic token cost formula

For API token pricing:

`Input cost = input_tokens / 1,000,000 × input_price_per_1M`

`Output cost = output_tokens / 1,000,000 × output_price_per_1M`

`Total token cost = input cost + output cost + applicable cache/batch/tool charges`

Example (illustrative rates only):
- 80,000 input tokens at $0.50 per million = $0.04.
- 8,000 output tokens at $2.00 per million = $0.016.
- Total = $0.056 per run, excluding other fees.

Always use the exact model's current pricing and actual usage data.

## 3. Model routing

Use different models for different task difficulty.

| Task | Routing strategy |
|---|---|
| Alert deduplication | Small/fast model or deterministic code |
| Alert classification | Low-cost model |
| Log summary | Small/fast model, escalate if unclear |
| Routine incident investigation | Mid-tier model |
| Complex multi-service RCA | Stronger model |
| Final report formatting | Smaller model or deterministic template |

Do not escalate based only on a model's self-reported confidence. Use objective signals such as unresolved evidence gaps, failed tool calls, conflicting results, or low evaluation scores.

## 4. Reduce prompt size

- Filter logs by time, service, namespace, and severity.
- Remove repeated stack traces and duplicate lines.
- Retrieve relevant runbook chunks instead of full documents.
- Summarize large tool outputs before adding them to context.
- Pass only relevant deployment history.
- Use metadata and query filters in retrieval.
- Keep system prompts concise and stable.

## 5. Output control

- Specify a clear report format.
- Set a reasonable output-token limit.
- Avoid asking for multiple redundant explanations.
- Use structured fields for machine-consumed responses.
- Keep final summaries shorter than investigation evidence.

## 6. Caching

Where supported:
- Cache stable system instructions or repeated reference context.
- Reuse common runbook content.
- Cache only when freshness and security rules permit.
- Check provider-specific cache minimums, pricing, TTL, and invalidation behavior.

Do not cache rapidly changing incident state as if it were current.

## 7. Batching

Batch is useful for independent, non-urgent tasks such as:
- Overnight incident report summaries.
- Historical ticket classification.
- Offline log labeling.
- Large evaluation runs.

Do not batch interactive, urgent incident-response tasks if it violates latency requirements.

## 8. Agent loop controls

Limit:
- Maximum tool calls per incident.
- Maximum model turns.
- Maximum tokens per request and per incident.
- Retry count.
- Timeout per tool.
- Overall incident-analysis time.
- Escalation count.

Detect duplicate tool calls and stop when the evidence does not change.

## 9. Cost dashboard metrics

Track:
- Input and output tokens per model.
- Cost per incident.
- Cost per alert.
- Average and p95 latency.
- Tool calls per incident.
- Retry rate.
- Escalation rate.
- Cache hit rate.
- Incidents handled per month.
- Cost per resolved incident (when resolution attribution is reliable).

## 10. Example monthly estimate

Assumptions (illustrative only):
- 1,000 incidents per month.
- Each incident averages 20,000 input tokens and 3,000 output tokens.
- Input rate: $0.50 per million.
- Output rate: $2.00 per million.

Per incident:
- Input: 20,000 / 1,000,000 × $0.50 = $0.010.
- Output: 3,000 / 1,000,000 × $2.00 = $0.006.
- Total = $0.016.

Monthly model-token estimate:
- 1,000 × $0.016 = **$16/month**.

This excludes hosting, monitoring, retrieval, tool execution, caching, and additional agent calls. Actual rates and token volumes can vary significantly.

## 11. Optimization checklist

- [ ] Use deterministic code for simple tasks.
- [ ] Route tasks to the cheapest model that meets quality targets.
- [ ] Escalate only when evidence indicates complexity.
- [ ] Deduplicate alerts before invoking LLMs.
- [ ] Filter and summarize logs before sending.
- [ ] Use retrieval instead of stuffing entire documents into context.
- [ ] Set input/output and agent-step limits.
- [ ] Reuse/cache stable context where supported.
- [ ] Track actual token usage and cost.
- [ ] Benchmark quality before downgrading models.
- [ ] Revisit model prices and IDs regularly.

## 12. Hosted vs self-hosted cost

Hosted API:
- Pay for token usage and applicable service charges.
- No need to maintain inference GPUs.
- Cost depends on traffic and model selection.

Self-hosted:
- GPU/CPU infrastructure, power, storage, networking, operations, and capacity planning.
- Potentially economical at sustained utilization.
- Can be expensive when hardware is idle or needs scaling for peaks.

Compare total cost of ownership over your expected utilization—not token price alone.

## 13. Resources

- [OpenAI API pricing](https://openai.com/api/pricing/)
- [Anthropic pricing](https://docs.anthropic.com/en/docs/about-claude/pricing)
- [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- [Ollama](https://ollama.com/)
- [Meta Llama](https://www.llama.com/)

## 14. Key takeaways

The most effective cost strategy is architecture-level optimization: route intelligently, reduce unnecessary context, limit agent loops, cache stable data, batch non-urgent tasks, and measure cost per successful outcome.
