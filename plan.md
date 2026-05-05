# AI Engineer Interview Preparation Plan 🚀

Welcome to your AI Engineer (AIE) learning journey! Today's AI Engineers often need to combine Large Language Models (LLMs) for application development. Therefore, interviews not only test traditional Machine Learning/Deep Learning foundations but also lean towards LLM engineering, RAG, Agents, Prompt Engineering, and model fine-tuning/deployment.

This plan divides common interview questions into several core modules and plans a learning pace of **3 questions per day**. You can record your learning progress in this document (e.g., mark `[x]` after a question or include a link to your notes).

## 🧠 Learning Module Breakdown

1. **Module 1: LLM Basics** (Parameter structure, Attention mechanism, Tokenization)
2. **Module 2: Prompt Engineering** (Techniques, Chain of Thought, Injection defense)
3. **Module 3: RAG (Retrieval-Augmented Generation)** (Vector databases, Chunking, Retrieval strategies, Reranking)
4. **Module 4: Agent & Tool Calling** (ReAct, Function Calling, Memory)
5. **Module 5: Evaluation & Engineering** (Evaluation metrics, Inference acceleration, Hallucination mitigation)
6. **Module 6: Fine-tuning Basics** (SFT, PEFT, LoRA concepts)

---

## 📅 Daily Plan

### Week 1: LLM Basics & Prompt Engineering
*Goal: Understand the basic principles of LLMs and how to better interact with them.*

**Week 1 Day 1: Transformer & LLM Foundations**
- [ ] 1. What is the Transformer model? Briefly describe its core architecture (Encoder-Decoder vs. Decoder-only). Why are most of today's LLMs Decoder-only?
- [ ] 2. What are Attention and Self-Attention? What problems do they solve?
- [ ] 3. What are Tokens and Tokenization? What is the difference between BPE and WordPiece algorithms?

**Week 1 Day 2: LLM Context & Mechanisms**
- [x] 1. What is KV Cache? What role does it play in LLM inference?
- [x] 2. What is the Context Window? What are the current technical approaches to extend context length?
- [x] 3. What are Temperature and Top-p / Top-k? How do they affect the model's generated output?

**Week 1 Day 3: Prompt Engineering Core Skills**
- [x] 1. What is Few-shot Prompting? What is its advantage over Zero-shot?
- [x] 2. What is Chain of Thought (CoT)? Why does it improve the model's performance on reasoning tasks?
- [x] 3. Besides CoT, what other advanced prompting strategies do you know? (e.g., Tree of Thoughts (ToT), Self-Consistency)

**Week 1 Day 4: Prompt Safety & Optimization**
- [x] 1. What are Prompt Injection and Jailbreak? How do you defend against them?
- [x] 2. If the model's JSON output is unstable, besides requesting "Output strictly in JSON", what other engineering or model-layer solutions are there? (e.g., System Prompt reinforcement, Function Calling, JSON Schema constraints)
- [x] 3. What are "hallucinations" in large models? What engineering methods are commonly used to reduce or mitigate them?

### Week 2: RAG Core Technologies
*Goal: Master each stage of Retrieval-Augmented Generation (RAG).*

**Week 2 Day 1: RAG Basics & Document Parsing**
- [ ] 1. What is RAG? Briefly describe the standard RAG process (Parsing, Chunking, Vectorization, Retrieval, Generation). What are the pros and cons of RAG compared to direct fine-tuning?
- [ ] 2. What are the strategies for document Chunking? How do you choose an appropriate Chunk Size and Overlap?
- [ ] 3. What are the common pain points in parsing complex PDFs or web pages (containing tables/images)? What are the mainstream parsing tools and optimization ideas?

**Day 6: Vectorization & Embedding**
- [ ] 1. What is an Embedding model? How is it different from traditional Bag-of-Words models?
- [ ] 2. What is a Vector Database? What are some common vector databases (e.g., Chroma, Pinecone, Milvus, Qdrant)? What is the general principle of core retrieval algorithms like HNSW?
- [ ] 3. In multi-language or vertical-specific scenarios, what should you do if open-source Embedding models perform poorly?

**Day 7: Advanced Retrieval Strategies**
- [ ] 1. What is Hybrid Search? (e.g., Sparse Retrieval BM25 + Dense Retrieval) Why use hybrid search?
- [ ] 2. What is Reranking? Why is a Reranker model needed after retrieval for secondary sorting?
- [ ] 3. Briefly describe common Query optimization strategies (e.g., Query Expansion, Query Rewrite, HyDE).

### Week 3: Agent Development & Fine-tuning
*Goal: Understand AI Agent mechanisms and learn model evaluation/fine-tuning basics.*

**Day 8: Agent**
- [ ] 1. What is an AI Agent? Explain the core components of an Agent (e.g., Profile/Persona, Memory, Planning, Tools/Action).
- [ ] 2. What is the ReAct (Reasoning and Acting) framework? What is its workflow?
- [ ] 3. What is Function Calling? How do models learn to call and execute external tools?

**Day 9: Memory & Agent Frameworks**
- [ ] 1. In Agent applications, what is the difference between Short-term Memory and Long-term Memory? How are they usually implemented?
- [ ] 2. Briefly describe the core differences and use cases for LangChain and LlamaIndex.
- [ ] 3. What is a Multi-Agent system? What are some representative frameworks (e.g., AutoGen, CrewAI)? What pain points of single Agents do they solve?

**Day 10: Model Evaluation & Engineering**
- [ ] 1. How should RAG systems be evaluated? (e.g., Faithfulness, Answer Relevance in the RAGAS framework)
- [ ] 2. When deploying LLMs, what is vLLM? What problem does its most important PagedAttention mechanism solve?
- [ ] 3. What is LoRA (Low-Rank Adaptation)? What are its advantages over Full Fine-Tuning?

---

## 📝 How to Use This Document to Record Progress

1. Pick 3 questions each day for in-depth study.
2. You can write your understanding and summary of answers directly under each question, or attach a link to your notes.
3. If you encounter difficulties during your study, call me anytime, and I will provide detailed explanations and easy-to-understand examples!
