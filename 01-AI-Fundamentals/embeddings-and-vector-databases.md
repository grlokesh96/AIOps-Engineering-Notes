# Embeddings & Vector Databases

## 1. What is an embedding?

An embedding is a numerical vector representing data such as text, an image, or another item. Embeddings are designed so that useful relationships—often semantic similarity—can be measured mathematically.

Example:
- “Pod keeps restarting”
- “Container repeatedly crashes”

These phrases use different words but may have similar meanings. Embeddings can help retrieve documents relevant to both.

## 2. Why embeddings are useful

Embeddings support:
- Semantic search.
- Similar-document retrieval.
- Clustering.
- Recommendation.
- Retrieval-Augmented Generation (RAG).
- Finding similar incidents and runbooks.

They are not truth scores. Similarity does not prove that two incidents share a root cause.

## 3. Vector basics

A vector is an ordered list of numbers, for example:

`[0.12, -0.08, 0.41, ...]`

Embedding dimensions depend on the model. Applications store vectors alongside metadata and source content or references.

## 4. Similarity methods

### Cosine similarity
Measures the angle relationship between vectors:

`cosine_similarity(A, B) = (A · B) / (||A|| ||B||)`

Often used to rank semantic similarity. Exact interpretation depends on the embedding model and normalization.

### Euclidean distance
Measures straight-line distance between vectors.

### Dot product
Measures the product of vector magnitudes and alignment. The appropriate ranking method depends on how the embedding model was trained and whether vectors are normalized.

## 5. What is a vector database?

A vector database or vector-search engine stores embeddings and supports similarity search, often combined with metadata filters and keyword search.

Common technologies include:
- pgvector (PostgreSQL extension).
- Qdrant.
- Milvus.
- Weaviate.
- Pinecone (managed service).
- Elasticsearch/OpenSearch vector search capabilities.

Choose based on deployment model, scale, filtering, operations, latency, cost, security, and integration needs—not hype alone.

## 6. RAG workflow

1. Collect documents (runbooks, incident reports, architecture docs).
2. Clean and split documents into chunks.
3. Generate embeddings for each chunk.
4. Store vectors, text references, and metadata.
5. Embed a user question.
6. Retrieve similar chunks.
7. Optionally rerank results.
8. Send relevant context to the LLM.
9. Generate an answer with source references.
10. Evaluate retrieval and answer quality.

## 7. Chunking

Chunking divides large documents into smaller retrievable pieces.

Consider:
- Chunk size.
- Overlap.
- Section/heading boundaries.
- Document type.
- Metadata.
- Keeping procedures and prerequisites together.

Very small chunks may lose context. Very large chunks may dilute relevance and waste context. Test chunking against real questions.

## 8. Metadata

Useful metadata fields:
- `document_id`
- `source`
- `title`
- `section`
- `created_at` or `updated_at`
- `environment`
- `service`
- `namespace`
- `incident_type`
- `access_scope`

Metadata filtering can restrict retrieval to the relevant service, environment, team, or document type. Enforce access permissions before exposing retrieved content.

## 9. RAG example for AIOps

Knowledge sources:
- Kubernetes runbooks.
- Previous incident RCAs.
- Service ownership information.
- Deployment history.
- Troubleshooting guides.

Question:
“Why might a payments pod be failing readiness checks?”

Retrieval should find relevant readiness probe guidance, service-specific runbooks, and similar incident records. The LLM should summarize the retrieved evidence, distinguish it from assumptions, and propose read-only checks.

## 10. Hybrid search

Vector search captures semantic similarity. Keyword search helps with exact terms such as error strings, pod names, alert IDs, versions, and config keys.

Hybrid retrieval combines both approaches. A reranker can then re-order candidate results using relevance.

For operations knowledge, hybrid search can be useful because incident queries often contain both semantic descriptions and exact technical identifiers.

## 11. Vector database vs. traditional database

| Traditional database | Vector search system |
|---|---|
| Exact filters and structured queries | Similarity-based retrieval |
| SQL, keys, indexes | Embedding vectors and ANN indexes |
| Great for records and transactions | Great for semantically related content |
| Exact equality/range queries | Approximate or exact nearest-neighbor search |

Many applications use both. Keep source-of-truth operational records in appropriate databases and use vector search as a retrieval layer.

## 12. Security and operations

- Do not embed secrets, tokens, private keys, or unnecessary sensitive data.
- Apply access controls to documents and retrieval results.
- Track document versions and remove stale knowledge.
- Consider tenant/team isolation.
- Protect against prompt injection embedded in retrieved documents.
- Log retrieval sources and evaluate relevance.
- Re-embed when models or document content change, as required.
- Monitor storage, index performance, latency, and retrieval quality.

## 13. Hands-on lab: Runbook RAG

Suggested stack:
- Python.
- An embedding model.
- Qdrant or pgvector.
- An LLM API or local inference.
- FastAPI (optional for an API).

Steps:
1. Add 5 sanitized Kubernetes runbooks.
2. Chunk them by headings and paragraphs.
3. Generate embeddings.
4. Store text, vectors, and metadata.
5. Ask questions about pod scheduling, CrashLoopBackOff, probes, and resource limits.
6. Display retrieved source sections.
7. Compare answers with and without retrieval.
8. Record missed retrievals, irrelevant results, and hallucinations.

## 14. Key takeaways

Embeddings enable semantic retrieval. Vector databases make similarity search practical at scale. RAG connects models to relevant external knowledge, but quality depends on source quality, chunking, retrieval, permissions, grounding, and evaluation.
