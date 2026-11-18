# Google Gemini Models

## 1. Overview

Gemini is Google’s model family with text, multimodal, reasoning, and different speed/cost tiers depending on the release. It can be accessed through supported Google AI services and APIs.

Model names, access methods, quotas, and pricing change. Use official documentation to select a current stable or preview model.

## 2. Model tiers (conceptual)

| Tier | Use | Example workloads |
|---|---|---|
| Flash / fast tier | Low-latency and cost-conscious tasks | Alert triage, extraction, summaries |
| Pro / advanced tier | More demanding tasks | Complex reasoning, coding, analysis |
| Specialized / multimodal | Tasks with supported modalities | Image, audio, video, document analysis |

Check model-specific capability, context, output, rate limit, and pricing documentation before production.

## 3. Use cases for DevOps and AIOps

- Summarize long incident logs.
- Analyze deployment failure evidence.
- Classify alerts and route incidents.
- Explain infrastructure code.
- Analyze supported screenshots or diagrams.
- Search internal runbooks through RAG.
- Generate incident summaries and postmortem drafts.

## 4. API example (Python)

Install the Google Gen AI SDK:

```bash
python -m venv .venv
source .venv/bin/activate
pip install google-genai
```

Configure credentials according to the API/service you are using. Never commit API keys.

```bash
export GEMINI_API_KEY="replace-with-your-key"
```

Example:

```python
import os
from google import genai

client = genai.Client(api_key=os.environ["GEMINI_API_KEY"])

response = client.models.generate_content(
    model="YOUR_CURRENT_GEMINI_MODEL_ID",
    contents="Explain this sanitized Kubernetes error and suggest read-only checks."
)

print(response.text)
```

Use the model ID from the current official documentation. For production systems, review authentication, quotas, region, data controls, and SDK version.

## 5. AIOps model routing example

| Task | Candidate tier |
|---|---|
| Alert deduplication / classification | Fast tier |
| Short log summary | Fast tier |
| Common pod troubleshooting | Fast or mid-tier, based on benchmark |
| Multi-service RCA | Advanced tier |
| Large document/runbook analysis | Model with suitable context and retrieval strategy |
| Incident report | Fast tier after evidence and diagnosis are finalized |

Do not assume long context means the model will accurately attend to every detail. Retrieval, summaries, and evidence references help.

## 6. Cost and latency controls

- Route routine requests to the cheaper fast model.
- Use a stronger model only when investigation complexity warrants it.
- Use RAG to retrieve relevant runbook chunks rather than sending entire repositories.
- Set output-token limits.
- Cache stable context if supported.
- Track retries, token usage, and latency.
- Compare quality on a representative incident test set.

## 7. Safety for agent workflows

The model should request tools through an application layer. The application must:
- Allow only registered tools.
- Validate arguments.
- Enforce permissions.
- Rate-limit calls.
- Limit agent steps.
- Audit tool calls.
- Require approval for production write actions.

## 8. Resources

- [Gemini API documentation](https://ai.google.dev/gemini-api/docs)
- [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)
- [Google Cloud Vertex AI documentation](https://cloud.google.com/vertex-ai/docs)

## 9. Hands-on lab

Create an alert triage classifier:
1. Prepare sanitized alert payloads with labels and annotations.
2. Ask the model to output category, severity recommendation, affected service, and required evidence.
3. Validate the response against a JSON schema.
4. Test normal alerts, noisy alerts, ambiguous alerts, and malformed inputs.
5. Measure accuracy, latency, and cost per alert.
