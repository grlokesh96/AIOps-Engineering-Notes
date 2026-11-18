# Generative AI

## 1. What is Generative AI?

Generative AI (GenAI) is a category of AI systems that creates new content based on patterns learned during training. Output may include text, code, images, audio, video, or structured data.

Examples:
- Generate a deployment troubleshooting summary.
- Draft a Kubernetes manifest.
- Summarize a GitHub Actions failure.
- Create an incident postmortem.
- Generate an explanation of a Terraform plan.

## 2. Predictive AI vs. Generative AI

| Predictive AI | Generative AI |
|---|---|
| Predicts a label, value, or outcome | Creates new content |
| Often uses structured features | Often accepts natural-language or multimodal input |
| Example: predict anomaly likelihood | Example: explain why an anomaly may matter |

These approaches can be combined. A detection system may identify an anomaly, while a generative model explains the evidence and suggests investigation steps.

## 3. How generation works (high-level)

1. The input is converted into tokens or other model representations.
2. The model processes the input using learned parameters.
3. It estimates likely next tokens or output elements.
4. Decoding selects the next output.
5. The process repeats until a stop condition is reached.

The output is generated probabilistically. A fluent answer is not necessarily a correct answer.

## 4. Common capabilities

- **Text generation:** explanations, reports, summaries.
- **Code generation:** scripts, queries, configuration drafts.
- **Summarization:** condensing long logs or documents.
- **Question answering:** responding based on model knowledge or supplied evidence.
- **Extraction:** turning unstructured text into structured fields.
- **Multimodal analysis:** interpreting supported combinations of text, images, audio, or video.
- **Reasoning assistance:** exploring hypotheses and plans.

## 5. GenAI in DevOps / AIOps

| Workflow | GenAI assistance |
|---|---|
| Incident response | Summarize evidence and possible causes |
| CI/CD | Explain failing stages and suggest fixes |
| Observability | Describe telemetry trends and correlate signals |
| Security | Explain scan findings and prioritize investigation |
| FinOps | Explain cost anomalies and suggest investigation |
| Documentation | Draft runbooks and postmortems |

### Example: log summary prompt

Input: several hundred lines of application logs.

Expected structured response:
- Summary
- Time window
- Error patterns
- Suspected dependencies
- Evidence (exact log excerpts or line references)
- Hypotheses (clearly labeled)
- Next read-only checks

Do not allow a model to invent evidence. Require references to the supplied log lines or tool results.

## 6. GenAI system patterns

### Direct generation
User prompt → model → response.

### Retrieval-augmented generation (RAG)
Question → retrieve relevant documents → model uses retrieved evidence → grounded response.

### Tool-augmented generation
Question → model decides whether a tool is needed → tool runs → result is returned to model → response.

### Agent workflow
Goal → plan/decide → tool calls → observe results → continue or finish under limits.

## 7. Limitations and risks

- Hallucination or unsupported conclusions.
- Outdated model knowledge.
- Prompt injection from untrusted logs, tickets, webpages, or repository content.
- Sensitive data leakage through prompts or tool outputs.
- Inconsistent output formatting.
- Latency and inference costs.
- Non-deterministic results.
- Incorrect code or commands.

## 8. Production controls

- Minimize and redact sensitive data.
- Separate trusted instructions from untrusted retrieved content.
- Validate structured outputs against schemas.
- Restrict tools and permissions.
- Require approval for high-impact actions.
- Log inputs, tool calls, decisions, and outputs with appropriate privacy controls.
- Evaluate against a test set of real tasks.
- Monitor cost, latency, quality, and failure rates.

## 9. Practical lab

Build a **CI Log Explainer**:
1. Provide a sanitized CI failure log.
2. Ask the model to summarize the failure.
3. Require output fields: `failure_stage`, `evidence`, `likely_cause`, `confidence`, `next_checks`.
4. Validate the JSON output.
5. Compare model conclusions against the actual failure.
6. Record incorrect diagnoses and improve the prompt.

## 10. Key takeaways

Generative AI is a useful interface for unstructured engineering information and drafting. It should be paired with evidence, validation, permissions, and deterministic execution for reliable operations.
