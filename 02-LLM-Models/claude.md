# Anthropic Claude Models

## 1. Overview

Claude is Anthropic’s family of language and multimodal models available through its API and supported platforms. Claude model tiers generally provide different trade-offs in capability, latency, and price.

Always check the current model lineup and API IDs in the official documentation; model versions and aliases can change.

## 2. Model tiers (conceptual)

| Tier | Intended use | Examples of workloads |
|---|---|---|
| Haiku-class | Fast, cost-sensitive | Classification, extraction, simple summaries |
| Sonnet-class | Balanced capability and cost | Coding, investigation, tool-driven workflows |
| Opus-class | Higher-capability complex tasks | Difficult reasoning, complex RCA, planning |

The names and availability of tiers may evolve. Confirm the currently supported models before choosing.

## 3. Strengths to evaluate

- Multi-step analysis and reasoning.
- Coding and code review.
- Tool use in agent workflows.
- Large-context document analysis (limits vary by model).
- Summarization and structured task outputs.
- Following detailed constraints.

No model is guaranteed to be accurate. Test it on real engineering tasks and verify its evidence.

## 4. DevOps and AIOps use cases

- Analyze Kubernetes events and sanitized logs.
- Investigate deployment or CI/CD failures.
- Review Terraform plans and Kubernetes manifests.
- Summarize security scan findings.
- Draft incident RCAs and postmortems.
- Query tools through an agent framework, with permissions enforced by the tool layer.

## 5. API workflow (Python example)

Install the SDK:

```bash
python -m venv .venv
source .venv/bin/activate
pip install anthropic
```

Set your key securely (do not commit it):

```bash
export ANTHROPIC_API_KEY="replace-with-your-key"
```

Example:

```python
import os
import anthropic

client = anthropic.Anthropic(api_key=os.environ["ANTHROPIC_API_KEY"])

response = client.messages.create(
    model="YOUR_CURRENT_CLAUDE_MODEL_ID",
    max_tokens=500,
    messages=[
        {
            "role": "user",
            "content": "Explain this sanitized Kubernetes error and list safe diagnostic checks."
        }
    ],
)

print(response.content[0].text)
```

Replace the placeholder with a model ID listed in the current API docs. Do not put credentials in source code, logs, or notebooks.

## 6. Tool use in an incident agent

A tool-enabled workflow can:
1. Receive an alert.
2. Ask the model to select an allowed diagnostic tool.
3. Validate the tool name and arguments.
4. Execute the tool through a separate service.
5. Return the result to the model.
6. Repeat within a bounded step budget.
7. Produce a report with evidence and next checks.

Example read-only tools:
- `get_pod`
- `describe_pod`
- `get_events`
- `get_previous_logs`
- `query_prometheus`
- `search_loki`

Keep the tool implementation, RBAC, approval checks, and audit logging outside the LLM.

## 7. Prompting tips

- Define the agent’s role and task.
- Supply the environment and relevant evidence.
- Require facts and hypotheses to be separated.
- Require source evidence for claims.
- Specify a strict output schema.
- Tell the model when to stop and request more information.

## 8. Cost controls

- Use a fast tier for alert classification.
- Escalate only complex tasks.
- Trim irrelevant logs and retrieve targeted chunks.
- Limit output tokens.
- Use prompt caching where available and appropriate.
- Track token usage per incident and per agent.

## 9. Safety

- Do not grant the model unrestricted cluster access.
- Prefer read-only tools for investigation.
- Validate commands and arguments.
- Require approvals for production changes.
- Treat logs and retrieved documents as untrusted input.
- Redact secrets and sensitive customer data.

## 10. Useful resources

- [Claude documentation](https://docs.anthropic.com/)
- [Claude API overview](https://docs.anthropic.com/en/api/overview)
- [Claude pricing](https://docs.anthropic.com/en/docs/about-claude/pricing)

## 11. Hands-on lab

Build a Claude-powered incident report generator:
1. Collect a sanitized incident event/log bundle.
2. Send evidence with a prompt that forbids invented observations.
3. Request summary, evidence, hypotheses, missing data, and next checks.
4. Evaluate its output against a known root cause.
5. Track cost and accuracy.
