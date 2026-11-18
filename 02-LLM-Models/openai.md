# OpenAI Models

## 1. Overview

OpenAI provides models through its API for language, reasoning, coding, multimodal tasks, and agent/tool workflows. Model families and API features evolve frequently.

Choose models by workload and verify the current model ID, supported API, context/output limits, tools, and pricing.

## 2. Capability categories

| Category | Typical task |
|---|---|
| General-purpose | Q&A, summaries, transformations |
| Reasoning-capable | Complex analysis, planning, difficult RCA |
| Smaller / low-cost | Classification, extraction, routing |
| Multimodal | Supported image, audio, or other input/output tasks |

These are broad categories; consult the current model catalog for exact availability.

## 3. DevOps use cases

- Explain CI pipeline failures.
- Review code and infrastructure changes.
- Produce structured incident reports.
- Investigate operational data with tool calls.
- Build RAG assistants for runbooks.
- Help classify and route alerts.
- Generate draft automation code with human review.

## 4. Python API example

Install the SDK:

```bash
python -m venv .venv
source .venv/bin/activate
pip install openai
```

Set the API key:

```bash
export OPENAI_API_KEY="replace-with-your-key"
```

Example using the Responses API:

```python
import os
from openai import OpenAI

client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])

response = client.responses.create(
    model="YOUR_CURRENT_OPENAI_MODEL_ID",
    input="Explain this sanitized Kubernetes error and provide safe diagnostic steps."
)

print(response.output_text)
```

Replace the placeholder model ID with one available in your account and compatible with the API used.

## 5. Tool calling and agent workflow

Conceptually:
1. Send the task and available tools to the model.
2. The model may return a tool call.
3. Your application validates and executes that tool.
4. Send the tool result back to the model.
5. Continue within limits until a final answer is returned.

Tool calling is a request from the model—not permission to execute. Your application must enforce tool allowlists, schemas, authorization, limits, and approval rules.

## 6. Structured outputs

For production workflows, prefer schema-constrained outputs when supported. Validate output in code.

Example incident object:

```json
{
  "incident_summary": "string",
  "confirmed_evidence": ["string"],
  "hypotheses": [
    {
      "cause": "string",
      "confidence": "low | medium | high",
      "evidence": ["string"]
    }
  ],
  "next_checks": ["string"],
  "remediation_requires_approval": true
}
```

Do not interpret model-generated confidence as a calibrated probability unless it has been calibrated and evaluated.

## 7. Cost optimization

- Select the model tier based on measured quality.
- Route simple tasks to smaller models.
- Avoid sending duplicate or irrelevant context.
- Limit output tokens.
- Use caching and batch options when available and appropriate.
- Monitor retries and agent loops.
- Keep a model-cost dashboard.

## 8. Safety and reliability

- Keep API keys in a secret manager or protected environment.
- Redact sensitive data before model calls.
- Validate generated code and commands.
- Restrict tool permissions.
- Require approval for impactful production changes.
- Evaluate model changes before rollout.
- Provide a fallback when the model or API is unavailable.

## 9. Resources

- [OpenAI API documentation](https://platform.openai.com/docs)
- [Models documentation](https://platform.openai.com/docs/models)
- [API pricing](https://openai.com/api/pricing/)
- [Responses API](https://platform.openai.com/docs/api-reference/responses)

## 10. Hands-on lab

Create a CI failure analysis agent:
1. Feed a sanitized CI log.
2. Retrieve a limited set of relevant repository metadata.
3. Ask for failure stage, evidence, likely cause, next checks, and suggested fix.
4. Validate the output schema.
5. Compare against known failures.
6. Keep any write or remediation action behind approval.
