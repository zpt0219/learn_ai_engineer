# Week 3 Day 4: Model Alignment & Advanced Fine-Tuning 🎯

As LLMs become more powerful, ensuring their outputs align with human intent and safety guidelines is paramount. Today, we explore how models transition from predicting the next word to following instructions and optimizing for human preferences through SFT, RLHF, and DPO.

## 📚 Recommended Learning Resources
- **RLHF Overview:** [Illustrating Reinforcement Learning from Human Feedback (RLHF)](https://huggingface.co/blog/rlhf)
- **DPO Paper/Blog:** [Direct Preference Optimization: Your Language Model is Secretly a Reward Model](https://arxiv.org/abs/2305.18290)
- **SFT vs. Pre-training:** [State of GPT (Andrej Karpathy)](https://karpathy.ai/stateofgpt.pdf)

---

Please try to answer the following 3 questions in your own words:

---

### 1. Supervised Fine-Tuning (SFT)
**Question:** 
What is **SFT (Supervised Fine-Tuning)**? How does it differ conceptually and practically from the initial **Pre-training** phase of an LLM?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 2. Reinforcement Learning from Human Feedback (RLHF)
**Question:** 
Explain the core mechanism of **RLHF**. Briefly describe its three main stages: SFT, Reward Modeling, and RL Optimization (PPO).

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 3. Direct Preference Optimization (DPO)
**Question:** 
What is **DPO (Direct Preference Optimization)**? Why has it become a highly popular alternative to standard RLHF in modern LLM training pipelines?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

## 🤖 AI Assistant Review & Deep-Dive Reference

### 1. Supervised Fine-Tuning (SFT)
* **Pre-training** teaches the model the grammar of the world by predicting the next token over massive, unstructured web data. It creates a "base model" that often just auto-completes text (e.g., Prompt: "What is the capital of France?" Base Model Output: "What is the capital of Germany?").
* **SFT (Supervised Fine-Tuning)** transforms a base model into an "assistant." It uses high-quality, human-annotated instruction-response pairs (Q&A format) to teach the model *how to respond* to user queries safely and helpfully. SFT data is smaller in volume (tens of thousands of examples) but requires high structural quality.

### 2. Reinforcement Learning from Human Feedback (RLHF)
RLHF aligns the model with complex human preferences (e.g., politeness, helpfulness, safety) that are hard to hardcode.
1. **SFT Stage:** First, create a baseline instruction-following model.
2. **Reward Model Stage:** Generate multiple responses for a prompt and have humans rank them (A is better than B). Train a separate "Reward Model" to predict these human preference scores.
3. **RL Optimization (PPO):** Use Proximal Policy Optimization. The SFT model generates responses, the Reward Model scores them, and the LLM updates its weights to maximize the expected reward, all while using a KL-divergence penalty to ensure it doesn't drift too far from the original SFT model (preventing "reward hacking").

### 3. Direct Preference Optimization (DPO)
* **The Problem with RLHF:** PPO is notoriously unstable, resource-intensive, and complex to tune because it requires running an Actor model, a Reference model, a Reward model, and a Value model simultaneously in memory.
* **The DPO Solution:** DPO mathematically proves that you can bypass the Reward Model and the RL optimization step entirely. It frames preference alignment as a simple classification problem. Using the exact same human preference data (Chosen vs. Rejected responses), DPO updates the LLM's weights directly using a specialized loss function.
* **Advantage:** It achieves results comparable to or better than PPO but is significantly more stable, easier to implement, and requires far less VRAM compute overhead.
