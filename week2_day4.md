# Week 2 Day 4: Advanced RAG Patterns & Evaluation 📊

As RAG systems move from prototypes to production, simple retrieval is often insufficient. Today, we explore how to make retrieval more "intelligent" through **Contextual Retrieval**, how to measure success using the **RAG Triad**, and how to build self-correcting systems like **Self-RAG** and **CRAG**.

## 📚 Recommended Learning Resources
- **Anthropic: Contextual Retrieval:** [Introducing Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval)
- **RAG Evaluation Guide:** [Pinecone: RAG Evaluation](https://www.pinecone.io/learn/series/rag/rag-evaluation/)
- **Ragas: Evaluation Metrics:** [Ragas Metrics Overview](https://docs.ragas.io/en/stable/concepts/metrics/overview/)
- **Self-RAG Explained:** [Self-RAG: Learning to Retrieve, Generate and Critique](https://arxiv.org/abs/2310.11511)
- **CRAG (Corrective RAG):** [CRAG: Corrective Retrieval Augmented Generation](https://arxiv.org/abs/2401.15884)

---

Please try to answer the following 3 questions in your own words:

---

### 1. Contextual Retrieval (Anthropic's Approach)
**Question:** 
What is **Contextual Retrieval**? How does prepending a "contextual summary" to each document chunk (before embedding) solve the problem of "lost information" in traditional chunking?

**Your Answer:**
> Contextual Retrieval is that for each paragraph, use the LLM model to read the whole doc, and try to write a summary for this paragraph, then append this summary together with paragraph into the embedding pipeline. So the doc has some context about it. Next time user made a query, it has higher possibility to match this paragraph if it has key data needed. It solve the lost information by adding a summary. But the downside is that it is very expensive since every paragraph needs LLM to read whole doc (although LLM can cache it) and generate summary. 

**Corrected & Refined (AI Revision):**
> 1. **核心概念：** 你说得很对。传统的 Chunking 就像撕碎的书页，Contextual Retrieval 给每一页都贴上了“这本书在讲什么”的标签。
> 2. **解决的问题：** 它解决了“语义孤岛”问题。例如一个 Chunk 只写着“销售额增长了10%”，如果没有上下文，检索模型不知道这是指哪家公司、哪个季度。
> 3. **工程优化：** 确实昂贵，但正如你提到的，**Prompt Caching (如 Anthropic 的功能)** 是关键。通过缓存文档全文，为几千个 Chunk 生成总结的成本可以降低 90% 以上。

---

### 2. The RAG Triad
**Question:** 
Explain the three pillars of the **RAG Triad**: **Context Relevance**, **Groundedness** (Faithfulness), and **Answer Relevance**. If a system has high Answer Relevance but low Groundedness, what is likely happening?

**Your Answer:**
> Context Relevance is whether the paragraph retrieved match the user's query. Groundedness is whether RAG tried to generate wrong or non-exist data to user. Answer relevance is whether the final answer generated matches user's query. If system has high anwser Relevanc but low groundedness, it is likely hallucination.

**Corrected & Refined (AI Revision):**
> 1. **准确定义：** 
>    - **Context Relevance (检索精度)：** 找回来的东西对不对？
>    - **Groundedness (忠实度)：** 回答是不是完全基于找回来的东西？（不准瞎编）
>    - **Answer Relevance (回答相关性)：** 最终答案是不是用户想要的？
> 2. **场景分析：** 你判断得非常准。如果 **Answer Relevance 高** 但 **Groundedness 低**，说明模型给出了一个“听起来很完美”的答案，但它是靠自己的知识库（甚至幻觉）生成的，而不是基于你提供的私有文档。这在企业级 RAG 中是极其危险的。

---

### 3. Self-Reflective RAG (Self-RAG & CRAG)
**Question:** 
How do **Self-RAG** and **Corrective RAG (CRAG)** improve upon standard RAG? Briefly describe the role of "reflection tokens" in Self-RAG or the "evaluator" in CRAG.

**Your Answer:**
> Self-RAG is a technique that at fine-tuning stage. LLM tries to output the reflection tokens such as </retrieve>,... etc. and when these token if generated, the LLM tries to retrieve doc from RAG data base. If no such token generated, LLM tries to answer user directly.
>
> Corrective RAG is that when RAG returns several documents back, LLM first tries to evaluate these documents and if the documents doesn't match user's query, it then ignores them and try to do web search instead.

**Corrected & Refined (AI Revision):**
> 1. **Self-RAG (自反思)：** 除了 `[Retrieve]` 令牌，它还有 `[IsRel]` (是否相关) 和 `[IsSup]` (是否有支撑) 令牌。模型不仅决定什么时候搜，还负责评价搜回来的东西好不好，甚至会根据评价结果重新生成。
> 2. **CRAG (纠错检索)：** 你的理解很到位。CRAG 的核心是 **“外部知识评估”**。它将检索结果分为：Correct (直接用)、Ambiguous (模糊，需结合 Web 搜索) 和 Incorrect (抛弃，完全依赖 Web 搜索)。
> 3. **本质区别：** Self-RAG 是让模型“自省”；CRAG 是在流程中加入了一个“裁判”来纠偏。

---

## 🤖 AI Assistant Review & Deep-Dive Reference

### The RAG Triad Deep-Dive
| Metric | Focus | What it detects |
| :--- | :--- | :--- |
| **Context Relevance** | Query ↔ Context | Is the retrieval system actually finding the right documents? |
| **Groundedness** | Context ↔ Response | Is the LLM making things up (hallucinating) or sticking to the facts provided? |
| **Answer Relevance** | Query ↔ Response | Did the LLM actually answer the user's question? |

### Contextual Retrieval Workflow
1. **Full Document Context:** Take the entire document.
2. **Chunking:** Break it into chunks (e.g., 500 tokens).
3. **Contextualization:** For each chunk, use a cheap model (like Claude 3 Haiku) to generate a brief summary (e.g., "This chunk is from a legal contract regarding termination clauses...").
4. **Prepending:** Prepend this summary to the chunk.
5. **Embedding:** Embed the *contextualized* chunk.
**Benefit:** When a user asks a specific question, the embedding now contains global document context, significantly improving retrieval accuracy for small, specific details.

### Self-RAG vs. CRAG
*   **Self-RAG:** Trains the model to output **Reflection Tokens** (e.g., `[Retrieve]`, `[Relevant]`, `[Critique]`). The model itself decides when to search and how to judge its own work.
*   **CRAG:** Uses a **separate evaluator** to score retrieved documents. If scores are low, it triggers alternative actions (like a web search) to "correct" the missing knowledge before the generation step.

---
*Next Step: Week 2 Day 5 - GraphRAG & Knowledge Graphs.*
