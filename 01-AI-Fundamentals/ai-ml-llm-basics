# AI, Machine Learning & LLM Basics

## 1. Overview

Artificial Intelligence (AI) is the broader field of building systems that perform tasks associated with human intelligence, such as reasoning, perception, prediction, language understanding, and decision support.

Machine Learning (ML) is a subset of AI in which systems learn patterns from data rather than relying only on explicitly written rules.

Deep Learning (DL) is a subset of ML that uses multi-layer neural networks to learn complex representations.

Large Language Models (LLMs) are deep-learning models trained on large amounts of text and other data to understand and generate language. They are especially useful for summarization, question answering, code assistance, reasoning, and tool-assisted workflows.

## 2. AI vs. ML vs. Deep Learning vs. LLM

| Term | Meaning | Example |
|---|---|---|
| AI | Broad field of intelligent computer systems | Incident investigation assistant |
| ML | Learns patterns from data | Detecting abnormal CPU usage |
| Deep Learning | ML using multi-layer neural networks | Speech or image recognition |
| Generative AI | Produces new content | Generating an incident report |
| LLM | Language-focused foundation model | Summarizing logs and runbooks |

## 3. Common AI approaches

- **Rule-based systems:** Explicit rules, such as alerting when CPU exceeds a threshold.
- **Supervised learning:** Learns from labeled examples; e.g., classifying incidents by severity.
- **Unsupervised learning:** Finds patterns without labels; e.g., clustering similar alerts.
- **Reinforcement learning:** Learns through actions, feedback, and reward.
- **Generative AI:** Generates text, code, images, audio, or other content.
- **Agentic AI:** Uses a model, tools, context, and a control loop to work toward a goal.

## 4. Important LLM terms

| Term | Definition |
|---|---|
| Token | A chunk of text processed by the model; not necessarily a whole word |
| Context window | Amount of input/output context a model can process in one request |
| Inference | Running a trained model to produce an output |
| Training | Updating model parameters using data |
| Fine-tuning | Further training a model for a specific task or behavior |
| Temperature | Sampling control that affects output variability |
| Hallucination | Output that is unsupported or factually incorrect |
| Multimodal | Model handles more than one modality, such as text and images |
| Tool calling | Model requests a structured action from an external tool |
| Grounding | Connecting an answer to evidence or external data |

## 5. How LLMs are used in DevOps

- Explain build and deployment errors.
- Summarize long logs.
- Draft Terraform, Kubernetes YAML, and CI workflows.
- Search runbooks and incident history.
- Correlate evidence from metrics, logs, events, and traces.
- Suggest incident remediation.
- Generate postmortems and change summaries.

**Important:** Model output should be reviewed and validated. Never treat generated shell commands, Kubernetes manifests, or infrastructure changes as safe by default.

## 6. AI/ML lifecycle

1. Define the operational problem and success metric.
2. Collect and govern relevant data.
3. Prepare and evaluate data quality.
4. Select an approach (rules, ML, LLM, agent, or combination).
5. Test against representative cases.
6. Deploy behind controls.
7. Monitor quality, latency, cost, safety, and drift.
8. Improve based on feedback.

## 7. Example: Kubernetes alert classification

Input:
- Alert: `KubePodCrashLooping`
- Namespace: `payments`
- Restart count: 14
- Recent deployment: 8 minutes ago

A classifier or LLM can categorize the incident as high priority and route it to an investigation workflow. It should not conclude the deployment caused the failure without collecting evidence such as container logs, events, and deployment changes.

## 8. Where deterministic automation is better

Use scripts and rules for exact, repeatable tasks:
- Restarting a service under a predefined policy.
- Checking disk thresholds.
- Validating required labels.
- Enforcing RBAC.
- Running tests.

Use AI when unstructured evidence, ambiguity, summarization, or multiple possible explanations make reasoning valuable.

## 9. Key takeaways

- AI is a broad category; ML and DL are subsets.
- LLMs are useful for language-oriented tasks but are not authoritative databases.
- AI can augment DevOps workflows; deterministic controls should execute critical operations.
- Evaluate quality on realistic operational scenarios before production use.

## 10. Further learning

- Learn basic probability, statistics, and data quality.
- Understand tokens, context windows, embeddings, inference, and evaluation.
- Experiment with a hosted model and a local model.
- Build a log summarizer before building an autonomous incident agent.
