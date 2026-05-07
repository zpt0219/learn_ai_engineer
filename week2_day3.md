# Week 2 Day 3: Advanced Retrieval Strategies 🔍

Retrieval is not just about "finding the top K vectors." In production RAG, we often need to combine different search methods and refine the results to ensure the LLM gets the most relevant context. Today, we focus on **Hybrid Search**, **Reranking**, and **Query Optimization**.

## 📚 Recommended Learning Resources
- **Hybrid Search Deep Dive:** [Pinecone - Hybrid Search](https://www.pinecone.io/learn/hybrid-search/)
- **Reranking & Cross-Encoders:** [Cohere - Rerank: Say Goodbye to Irrelevant Results](https://txt.cohere.com/rerank/)
- **Understanding RRF:** [Elasticsearch - Reciprocal Rank Fusion (RRF)](https://www.elastic.co/guide/en/elasticsearch/reference/current/rrf.html)
- **HyDE (Hypothetical Document Embeddings):** [LlamaIndex - HyDE Query Transform](https://docs.llamaindex.ai/en/stable/examples/query_transformations/HyDEQueryTransformDemo.html)

---

Please try to answer the following 3 questions in your own words:

---

### 1. Hybrid Search (Lexical + Semantic)
**Question:** 
What is **Hybrid Search**? Why is combining **BM25 (Sparse Retrieval)** and **Vector Search (Dense Retrieval)** often better than using either alone? Briefly explain **Reciprocal Rank Fusion (RRF)**.

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 2. Reranking & Cross-Encoders
**Question:** 
In a RAG pipeline, we often retrieve 50-100 documents and then "rerank" them to pick the top 5. Why do we need this two-stage process? What is the technical difference between a **Bi-Encoder** (used for initial retrieval) and a **Cross-Encoder** (used for reranking) in terms of performance and accuracy?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 3. Query Optimization: HyDE
**Question:** 
Explain the concept of **HyDE (Hypothetical Document Embeddings)**. How does it solve the "gap" between a short user query and long, detailed documents? What are its potential risks (e.g., hallucinations)?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

## 🤖 AI Assistant Review & Deep-Dive Reference

### Bi-Encoder vs. Cross-Encoder
| Feature | Bi-Encoder (Embedding) | Cross-Encoder (Reranker) |
| :--- | :--- | :--- |
| **Input** | Query and Document separately | Query and Document together |
| **Speed** | Very fast (Pre-computed vectors) | Slow (Must process pairs at runtime) |
| **Accuracy** | Good (Approximate) | Excellent (Fine-grained interaction) |
| **Scale** | Can search millions of docs | Limited to tens/hundreds of docs |

### Reciprocal Rank Fusion (RRF) Formula
$$Score(d) = \sum_{r \in R} \frac{1}{k + rank(r, d)}$$
Where $k$ is a constant (usually 60). This formula ensures that documents appearing at the top of *multiple* search results get a significant boost, regardless of the raw scores from different algorithms.

---
*Next Step: Week 3 Day 1 - AI Agent Basics.*
