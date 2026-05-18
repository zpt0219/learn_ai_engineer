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
> The pre-training step, LLM mainly uses data from internet to train, it learns basic logics but is not fine-grained. The SFT stage, researchers will prepare accurate input and expected output to further tuning the weights, so the LLM learns how to answer question in a expected way. So pre-training is building the foundation, post-training using SFT is teaching LLM to be a expert in specific area.

**Corrected & Refined (AI Revision):**
> Your answer captures the essence well! Pre-training indeed builds the foundation using massive internet data. SFT uses high-quality, structured Q&A pairs. One minor correction: SFT isn't just about making it an "expert in a specific area", but primarily about teaching the model *how to act like a helpful assistant* and follow instructions, shifting it from a mere text auto-completer to a dialogue agent.

---

### 2. Reinforcement Learning from Human Feedback (RLHF)
**Question:** 
Explain the core mechanism of **RLHF**. Briefly describe its three main stages: SFT, Reward Modeling, and RL Optimization (PPO).

**Your Answer:**
> Reinforcement Learning from Human feedback is for post-training, at this stage, the model already has basic logic ability, but needs human to prepare perfect Q&A data to train the model to know how to answer questions properly. The RLHF has 4 components. A full weight model for tuning, a original model for referencing. A reward model for judging and a value model for predicting since it uses PPO for training. for each input, human forces model to make several output, and use reward model to score each output. compare the output with reference model's output so it will not over-tune. and when model is outputing, use value value to estimate the score, compare with final score from reward model. if estimated score is low and reward model give high score, means this is a good find! if both high score, meaning this is normal find. By these methods. a pre-trained model can be smarter.

**Corrected & Refined (AI Revision):**
> This is a very technically detailed answer! You correctly identified the four models involved during the PPO phase. A few clarifications: The unique part of RLHF is that humans don't write perfect answers; instead, they *rank* multiple model outputs to train the Reward Model. During the PPO stage, the Reward Model automatically scores new outputs, and the model updates its weights while using the Reference model to prevent "reward hacking" (over-tuning). Your description of the value model and reward model interplay is a good intuitive attempt at describing advantage estimation in PPO.

---

### 3. Direct Preference Optimization (DPO)
**Question:** 
What is **DPO (Direct Preference Optimization)**? Why has it become a highly popular alternative to standard RLHF in modern LLM training pipelines?

**Your Answer:**
> DPO is different way for post training, it still collect lots high quality Q&A data, but it tries to let LLM to make both good output and bad output, then try to make these 2 output as far as possible in score. So model knows which way is better, which way is worse. It saves lots memory since it only need full tuning model and reference model (same as RLHF, to avoid the over-tuning). but doesn't need reward model and value model. so it is more efficiency. Also, you can use Lora to replace the full tuning model, only use reference model + lora. further reduce gpu memory needed.

**Corrected & Refined (AI Revision):**
> Excellent answer! You nailed the core mechanic and the main advantage of DPO. By directly using preference data (chosen vs. rejected responses) to update the model weights, DPO completely eliminates the need for the separate Reward and Value models, making it far more memory-efficient and stable. Your point about pushing the probabilities of the good and bad outputs apart perfectly describes the DPO loss function. Mentioning LoRA as a further optimization is also a fantastic practical insight!

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
