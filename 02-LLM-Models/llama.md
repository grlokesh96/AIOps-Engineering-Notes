# Meta Llama Models

## 1. Overview

Llama is Meta’s family of open-weight AI models. Depending on the model and license, developers can download weights, run inference in their own environment, adapt models, or use hosted inference services.

**Open-weight does not automatically mean unrestricted or license-free.** Read the applicable license, acceptable-use terms, model card, and deployment requirements before using a model commercially or in production.

## 2. Why consider Llama?

- Self-hosted inference options.
- Greater control over where inference and data processing happen.
- Ability to choose serving stack, quantization, and hardware.
- Potentially predictable cost at sustained workloads.
- Community ecosystem for local and hosted inference.
- Customization and fine-tuning options where supported and permitted.

## 3. Trade-offs

Self-hosting transfers responsibilities to your team:
- GPU/accelerator provisioning.
- Model storage and downloads.
- Inference serving and scaling.
- Updates and vulnerability management.
- Monitoring, reliability, and capacity planning.
- Quality evaluation.
- License and governance review.

A hosted API may be cheaper overall for low or bursty usage once hardware, operations, and idle capacity are included.

## 4. Model selection

Model availability and names change. Choose based on the current Meta model catalog and benchmark your workload.

| Model size / class | General fit | Caveat |
|---|---|---|
| Small models | Lightweight classification, extraction, basic summaries | May struggle with complex reasoning or tool planning |
| Medium models | Coding, summaries, routine agent tasks | Check VRAM and quality |
| Large models | Complex analysis and reasoning | High hardware and serving cost |
| MoE models | Potentially efficient active compute | Total weight memory can still be large |

Do not choose solely by parameter count. Test reasoning, tool calls, response quality, latency, memory, and cost.

## 5. Serving options

Common inference approaches include:
- Ollama for convenient local experimentation.
- vLLM for high-throughput serving and API compatibility in supported configurations.
- Hugging Face Transformers for model loading and development.
- llama.cpp for CPU/GPU-supported quantized inference.
- Hosted inference providers for managed capacity.

The best server depends on model architecture support, GPU, concurrency, quantization, and deployment needs.

## 6. Quantization

Quantization reduces the numerical precision used to represent model weights, which can lower memory usage and sometimes improve performance.

Potential trade-offs:
- Lower VRAM/RAM needs.
- Faster inference depending on hardware and runtime.
- Possible quality loss.
- Compatibility differences between quantization formats and serving engines.

Test quantized models against your target tasks rather than assuming the smallest quantization is best.

## 7. Llama in AIOps

Good initial experiments:
- Summarize sanitized logs.
- Classify alerts.
- Extract incident fields.
- Answer runbook questions through RAG.
- Draft an RCA from tool evidence.

For complex incident investigation, benchmark local models against hosted models. Local inference may have lower marginal cost but may not match the quality, speed, or reliability of a larger hosted model.

## 8. Security checklist

- Verify model provenance and checksums when available.
- Review the license and use restrictions.
- Isolate inference workloads.
- Restrict outbound network access where appropriate.
- Protect model endpoints with authentication and authorization.
- Do not expose local inference ports publicly without controls.
- Redact secrets in prompts and logs.
- Patch runtime and container images.
- Test prompt-injection and tool abuse scenarios.

## 9. Resources

- [Meta Llama](https://www.llama.com/)
- [Llama model downloads and documentation](https://www.llama.com/docs/)
- [Hugging Face](https://huggingface.co/models) (filter for Llama and verify publisher/model card/license)

## 10. Hands-on lab

Run a local Llama model with Ollama, then:
1. Submit a sanitized incident log.
2. Ask for summary and evidence-backed hypotheses.
3. Compare the answer to a hosted model.
4. Record quality, latency, memory use, and cost assumptions.
5. Add the model to a simple read-only incident assistant.
