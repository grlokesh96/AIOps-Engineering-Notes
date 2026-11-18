# LLM Architecture

## 1. High-level architecture

A typical text LLM system includes:

1. **Tokenizer** – converts text into token IDs.
2. **Embedding layer** – maps token IDs to vector representations.
3. **Transformer layers** – process contextual relationships.
4. **Attention** – helps the model relate tokens within the context.
5. **Feed-forward blocks** – transform representations within each layer.
6. **Output projection** – maps final hidden representations to vocabulary scores.
7. **Decoder/sampling** – selects tokens to produce the output.

Many modern models use variations of this architecture. Details differ by model family, including dense versus mixture-of-experts (MoE), context length, multimodal components, and training methods.

## 2. Tokenization

Text is divided into tokens. A token can be:
- A whole word.
- Part of a word.
- Punctuation.
- Whitespace or a text fragment.

Tokenization affects:
- Input and output token counts.
- API cost.
- Context-window capacity.
- Latency.

Do not assume one token equals one word.

## 3. Embeddings

Token embeddings represent tokens as numeric vectors. Contextual representations change as tokens pass through model layers, allowing the same word to have different representations depending on context.

Embeddings are also used outside LLM generation for semantic search, similarity, clustering, and retrieval.

## 4. Transformer and attention

Transformers process token representations using attention mechanisms. Attention computes how information from different positions contributes to a representation.

At a conceptual level, attention uses:
- **Query (Q):** what a position is looking for.
- **Key (K):** what information a position offers.
- **Value (V):** the information that can be combined.

Scaled dot-product attention is commonly expressed as:

`Attention(Q, K, V) = softmax(QKᵀ / √d_k) V`

This formula is conceptual shorthand; real implementations may include masks, multiple heads, optimized kernels, and other architectural details.

## 5. Transformer blocks

A transformer block typically combines:
- Multi-head attention.
- Residual connections.
- Layer normalization or related normalization.
- Feed-forward network.

Stacking blocks allows the model to learn increasingly complex representations.

## 6. Autoregressive generation

Many LLMs generate text one token at a time:
1. Process the prompt.
2. Predict a distribution over the next token.
3. Select a token according to decoding settings.
4. Append it to the context.
5. Repeat until stop criteria are met.

The model's generated answer depends on the prompt, model parameters, context, and decoding configuration.

## 7. Decoding parameters

| Parameter | Purpose | Practical note |
|---|---|---|
| Temperature | Changes sampling randomness | Lower often produces more predictable output |
| Top-p | Samples from a probability-mass subset | Controls sampling diversity |
| Max output tokens | Limits response length | Helps control cost and runaway output |
| Stop sequence | Stops generation at specified text | Use carefully |
| Seed | May support reproducibility | Provider/model support varies |

For incident reports and structured extraction, favor constrained output and stable settings, then validate the result. Temperature alone does not guarantee factual accuracy.

## 8. Dense vs. Mixture-of-Experts (MoE)

- **Dense model:** Each token generally passes through the model's full set of feed-forward parameters.
- **MoE model:** A routing mechanism selects a subset of expert networks for a token.

MoE can reduce active computation compared with activating all parameters, but total parameters, memory requirements, routing behavior, serving complexity, and performance vary by implementation.

## 9. Training vs. inference

| Training | Inference |
|---|---|
| Learns/updates parameters | Uses trained parameters to generate outputs |
| Requires training data and optimization | Requires prompt/context and compute |
| Usually resource-intensive | Cost depends on model, hardware, tokens, and serving |
| Produces a trained model | Produces a response |

## 10. LLM serving concepts

- **Model weights:** learned parameters loaded for inference.
- **GPU/accelerator memory:** stores weights and runtime state.
- **KV cache:** caches attention key/value states to reduce repeated work during generation.
- **Batching:** processes multiple requests together where supported.
- **Quantization:** reduces numerical precision/weight size to lower memory or improve throughput, potentially affecting quality.
- **Throughput:** requests or tokens processed per unit time.
- **Latency:** time to complete or begin responding.
- **Concurrency:** number of requests handled simultaneously.

## 11. Architecture in an AI operations agent

User/Alert → Agent service → LLM API or local inference server → Tool calls → Kubernetes/observability APIs → Tool results → LLM → Evidence-based response.

The LLM should not directly hold cluster credentials. A tool service should enforce authorization, input validation, scope, rate limits, and audit records.

## 12. Key takeaways

Understanding tokens, attention, context, decoding, inference, and serving helps engineers estimate costs, diagnose latency, select models, and design reliable LLM-powered systems.
