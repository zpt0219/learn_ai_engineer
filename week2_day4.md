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
> 

**Corrected & Refined (AI Revision):**
> 

---

### 2. The RAG Triad
**Question:** 
Explain the three pillars of the **RAG Triad**: **Context Relevance**, **Groundedness** (Faithfulness), and **Answer Relevance**. If a system has high Answer Relevance but low Groundedness, what is likely happening?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 3. Self-Reflective RAG (Self-RAG & CRAG)
**Question:** 
How do **Self-RAG** and **Corrective RAG (CRAG)** improve upon standard RAG? Briefly describe the role of "reflection tokens" in Self-RAG or the "evaluator" in CRAG.

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

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
