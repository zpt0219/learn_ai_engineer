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
> BM25 is traditional retriving method by match key words. And vector search is using LLM to get the meaning of the paragraph. They all have their good and week points. And Reciprocal Rank funsion is to merge score by using alpha* score1 + (1-alpha)* score2. so higher score means the paragraph retrieved meets user's request better.

**Corrected & Refined (AI Revision):**
> 1. **BM25 vs. Vector:** 你说得非常对，BM25 擅长“精确匹配”（如产品 ID、专有名词），而向量搜索擅长“语义理解”。
> 2. **RRF 纠错：** 注意你提到的 `alpha * score1 + ...` 实际上是 **线性加权 (Linear Weighting)**。
> 3. **什么是 RRF？** 倒数排名融合 (Reciprocal Rank Fusion) 使用的是 **排名** 而不是原始分数。公式大致为 `Score = 1 / (60 + Rank_BM25) + 1 / (60 + Rank_Vector)`。
> 4. **为什么要用 RRF？** 因为 BM25 的分数范围（通常 >0）和向量搜索的分数范围（通常 0-1）完全不同，直接相加没有意义。RRF 通过只看排名，完美解决了不同搜索算法分数难以融合的问题。

---

### 2. Reranking & Cross-Encoders
**Question:** 
In a RAG pipeline, we often retrieve 50-100 documents and then "rerank" them to pick the top 5. Why do we need this two-stage process? What is the technical difference between a **Bi-Encoder** (used for initial retrieval) and a **Cross-Encoder** (used for reranking) in terms of performance and accuracy?

**Your Answer:**
> Because the top 50-100 documents may contains too much noise and vector database only approximately retrieve the possible results back for first retrival. And for re-ranking step. we are using LLM to compare user's request to these documents word by word. so LLM can judge whether these doc is really useful or not. With the 50-100 documents retrieval step, the LLM doesn't need to read whole database.

Bi-Encoder only check the similarity between user's vector from query to the vector in vector database. so it is very fast but less accurate. The Cross Encoder compare the word of paragraph retrieved to user's query. so it is very accurate.

**Corrected & Refined (AI Revision):**
> 1. **“漏斗式”检索：** 你的理解很到位。先用低成本、高并发的模型将几百万文档缩小到 100 个，再用高精度的模型进行“精筛”。
> 2. **Bi-Encoder (双编码器)：** 它将 Query 和 Doc **独立计算** 向量。只有在最后计算余弦相似度时才有交互。
> 3. **Cross-Encoder (交叉编码器)：** 它将 Query 和 Doc **拼接在一起** 输入模型。这意味着模型可以利用“自注意力机制 (Self-Attention)”让 Query 中的每一个词都去和 Doc 中的每一个词进行比对。这是它极其精准但速度慢的原因（无法预计算向量）。

---

### 3. Query Optimization: HyDE
**Question:** 
Explain the concept of **HyDE (Hypothetical Document Embeddings)**. How does it solve the "gap" between a short user query and long, detailed documents? What are its potential risks (e.g., hallucinations)?

**Your Answer:**
> HyDE is the method when receiving user's request,first try to generate a response using LLM ,it doesn't care whether the generated response is accurate or not, it then use this response, not the user original query to search the vector database. To retrieve the most similar paragraph from database and the use these retrieved data to generate formal response back to user. 
The potential risks are , if generated response has wrong directions, the wrong data from database could be retrieved and can produce worse response than using traditional method.

**Corrected & Refined (AI Revision):**
> 1. **解决“Query-Doc Gap”：** 用户的问题通常很短，而文档很长。在向量空间中，短问题和长答案往往距离较远。
> 2. **HyDE 的本质：** 通过生成“伪答案”，我们将搜索从“问题找答案”变成了“答案找答案”。在向量空间里，答案和答案的距离通常更近。
> 3. **幻觉风险：** 没错。如果 LLM 生成了一个非常自信但“错误”的伪文档，向量搜索会顺着这个错误在数据库里找到错误的证据，从而导致“确认偏误”形式的幻觉。

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
