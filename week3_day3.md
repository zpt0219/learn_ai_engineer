# Week 3 Day 3: Model Evaluation & Engineering 📊🚀

Moving beyond prototyping, taking AI systems to production requires rigorous evaluation and efficient deployment techniques. Today, we cover quantitative evaluation for RAG, high-throughput inference serving, and cost-effective model fine-tuning.

## 📚 Recommended Learning Resources
- **RAG Evaluation (RAGAS):** [RAGAS Documentation](https://docs.ragas.io/en/stable/)
- **High-Throughput Serving (vLLM):** [vLLM: Easy, Fast, and Cheap LLM Serving](https://vllm.ai/)
- **PagedAttention Mechanism:** [PagedAttention Explained](https://blog.vllm.ai/2023/06/20/vllm.html)
- **PEFT & LoRA:** [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685) / [HuggingFace PEFT](https://huggingface.co/docs/peft/index)

---

Please try to answer the following 3 questions in your own words:

---

### 1. Evaluating RAG Systems
**Question:** 
How should RAG systems be evaluated? (e.g., Explain metrics like **Faithfulness** and **Answer Relevance** in the RAGAS framework)

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 2. High-Performance LLM Deployment
**Question:** 
When deploying LLMs, what is **vLLM**? What problem does its most important **PagedAttention** mechanism solve?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 3. Parameter-Efficient Fine-Tuning
**Question:** 
What is **LoRA** (Low-Rank Adaptation)? What are its core advantages over **Full Fine-Tuning**?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

## 🤖 AI Assistant Review & Deep-Dive Reference

### 1. RAG Evaluation Frameworks (e.g., RAGAS)
Because LLMs generate free-form text, traditional metrics like BLEU or ROUGE are ineffective. Modern evaluation uses "LLM-as-a-Judge" to score specific aspects of the generation and retrieval pipeline.
* **Faithfulness:** Measures if the generated answer is entirely grounded in the retrieved context. It penalizes "hallucinations" where the LLM invents facts not present in the chunks.
* **Answer Relevance:** Measures how well the generated answer directly addresses the original user query (ignoring context).
* **Context Precision/Relevance:** Evaluates the *retriever's* performance. Did it fetch the correct, high-signal information without dragging in noisy, irrelevant chunks?

### 2. vLLM and PagedAttention
* **vLLM:** A high-throughput, memory-efficient open-source engine for serving LLMs in production.
* **The Problem:** In standard inference, the KV Cache (which stores keys and values of previous tokens to avoid recomputation) causes massive memory fragmentation because requests have unpredictable lengths. Up to 60-80% of KV cache memory is wasted.
* **PagedAttention:** Inspired by virtual memory and paging in OS, PagedAttention divides the KV cache into fixed-size blocks (pages) that can be stored non-contiguously in GPU memory. This virtually eliminates memory fragmentation, allowing the server to batch significantly more requests concurrently, leading to massive throughput improvements.

### 3. LoRA (Low-Rank Adaptation)
* **What it is:** LoRA is a highly popular Parameter-Efficient Fine-Tuning (PEFT) technique. Instead of updating all billions of parameters in a pre-trained model (Full Fine-Tuning), LoRA freezes the original weights and injects small, trainable "rank decomposition matrices" into specific layers (usually Attention blocks).
* **Advantages over Full Fine-Tuning:**
    * **Memory Efficiency:** Dramatically reduces GPU memory requirements (often fitting on a single consumer GPU).
    * **Speed:** Faster training times because fewer gradients are computed.
    * **Modularity:** The base model remains intact. You can swap small "LoRA adapter" files (often just megabytes in size) in and out for different tasks without hosting entirely separate models.
    * **Mitigates Forgetting:** Reduces the risk of catastrophic forgetting, where the model loses its general knowledge during fine-tuning.

---
*Next Step: Keep up the great work! You are now mastering the complete lifecycle of LLM application development.*
