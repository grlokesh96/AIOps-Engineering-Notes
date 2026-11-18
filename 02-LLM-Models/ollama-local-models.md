# Ollama & Local LLMs

## 1. What is Ollama?

Ollama is a tool for downloading, running, and serving supported language models locally. It provides a convenient CLI and local HTTP API for experimentation and applications.

Local execution can improve control over data flow and reduce per-token API charges, but it does **not** guarantee privacy automatically. Secure the host, endpoints, logs, and any tools connected to the model.

## 2. Install on Linux (Ubuntu)

Use the official installation instructions and review the script before running it in sensitive environments.

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Check installation and service:

```bash
ollama --version
systemctl status ollama
```

If the service is not running, use systemd to start it:

```bash
sudo systemctl enable --now ollama
```

## 3. Pull and run a model

First check the Ollama library for current model names and hardware requirements.

```bash
ollama pull MODEL_NAME
ollama run MODEL_NAME
```

Examples of model categories to explore (availability changes; use exact current tags from the library):
- Small instruct model for classification and summarization.
- Coding model for pipeline and configuration assistance.
- Larger reasoning model if hardware can support it.

List installed models and remove one:

```bash
ollama list
ollama rm MODEL_NAME
```

## 4. Local API

Ollama commonly exposes a local API at `http://localhost:11434`. Avoid exposing this port to the internet.

Example request:

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "MODEL_NAME",
  "prompt": "Summarize this sanitized CI error and suggest safe checks.",
  "stream": false
}'
```

Example chat request:

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "MODEL_NAME",
  "messages": [
    {"role": "user", "content": "Explain Kubernetes Pending pods and safe diagnostic checks."}
  ],
  "stream": false
}'
```

## 5. Python integration

```bash
pip install ollama
```

```python
import ollama

response = ollama.chat(
    model="MODEL_NAME",
    messages=[
        {"role": "user", "content": "Explain this sanitized Kubernetes event."}
    ],
)

print(response["message"]["content"])
```

Use a model tag available locally. Add error handling, timeouts, and input limits in production.

## 6. Hardware sizing considerations

No universal RAM/VRAM requirement exists. Requirements depend on:
- Parameter count.
- Quantization.
- Context length.
- KV-cache size.
- Model architecture.
- Runtime and GPU support.
- Concurrent requests.
- Throughput target.

A small quantized model may run on a laptop, while larger models may require multiple GPUs or hosted inference. Check the model's published requirements and measure memory during your workload.

## 7. Operational considerations

- Monitor CPU, RAM, GPU, VRAM, disk, and latency.
- Limit concurrent requests.
- Use model-specific timeouts.
- Keep models and runtime updated.
- Back up model configuration and prompt templates.
- Secure local API access and network exposure.
- Avoid sending secrets into prompts.
- Monitor model quality and resource exhaustion.

## 8. Local vs hosted inference

| Local / self-hosted | Hosted API |
|---|---|
| Control over deployment and data path | No GPU fleet to operate |
| No per-token provider billing | Usage-based costs |
| Hardware and operations cost | Provider service dependency |
| Capacity limited by your hardware | Capacity and rate limits depend on provider |
| You maintain updates, availability, scaling | Provider maintains serving infrastructure |

Calculate total cost of ownership (TCO), not only token prices.

## 9. Hands-on lab: local incident summarizer

1. Install Ollama on a Linux VM or workstation.
2. Pull a model suitable for your hardware.
3. Submit sanitized Kubernetes logs.
4. Prompt for summary, observed evidence, hypotheses, and read-only checks.
5. Compare output with a known incident.
6. Measure latency and memory.
7. Test how the model handles missing or conflicting evidence.

## 10. Resources

- [Ollama website](https://ollama.com/)
- [Ollama documentation](https://docs.ollama.com/)
- [Ollama model library](https://ollama.com/library)

## 11. Key takeaway

Local models are useful for learning, privacy-conscious workflows, and controlled environments. They still require security, monitoring, evaluation, hardware capacity, and operational maintenance.
