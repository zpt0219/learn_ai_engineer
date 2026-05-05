# Week 1 Day 3: Prompt Engineering Core Skills 💡

Welcome to Day 3! Today we dive deep into how to release the reasoning potential of large models through more sophisticated Prompt design. This is not only a basic skill for an AI Engineer but also the cornerstone of building complex Agents.

## 📚 Recommended Learning Resources
Before you start answering, these resources might be helpful:
- **Few-shot & CoT:** [Prompt Engineering Guide (Comprehensive & Authoritative)](https://www.promptingguide.ai/techniques/fewshot)
- **Chain of Thought (CoT) Deep Dive:** [DeepLearning.ai Prompt Engineering Course](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)
- **Advanced Strategies (ToT/Self-Consistency):** [Advanced Prompting Techniques](https://www.promptingguide.ai/techniques/consistency)

---

Please try to answer the following 3 questions in your own words:

---

### 1. Few-shot Prompting
**Question:** 
What is **Few-shot Prompting**? Compared to Zero-shot, what is its core advantage? In what scenarios might Few-shot fail or have a negative impact?

**Your Answer:**
> few shot prompting is to provide some example for llm, so llm knows how to resolve user's question based on example's style. The core advantage is that sometimes it is hard to write the correct and precise prompting sentence. By providing example, the LLM learns the styles and rules by itself and will output the result much better.
> 
> In scenarios might few-shot fail, I think it is that the example provided may lead llm to unexpected behaviors and increase the randomness. Also very large example may use lots context window and let llm less focus on user's original prompting.

**AI Feedback:**
*   ✅ **What's Right:** Good job identifying that examples provide "implicit rules" and that they consume context window.
*   🛠 **Needs Improvement:** Mention **Label Bias**. If your examples are biased (e.g., all examples are "Positive"), the model might ignore the actual input and just output "Positive". It's also critical for **Format Adherence** (e.g., outputting valid JSON).

**Corrected & Refined (AI Revision):**
> **Few-shot Prompting** involves providing 2-5 input-output pairs to "prime" the model's behavior.
> *   **Advantage:** It reduces ambiguity. Instead of describing a complex format, you show it. It leverages the model's **In-Context Learning (ICL)** capability.
> *   **When it fails:** 
>     1. **Majority Label Bias:** Models tend to over-predict the most frequent label in the examples.
>     2. **Recency Bias:** The model is more influenced by the *last* example provided.
>     3. **Context Length:** It increases costs and can cause "lost in the middle" issues for the actual task.

**Corrected & Refined (AI Revision):**
> 

---

### 2. Chain of Thought (CoT)
**Question:** 
What is **Chain of Thought (CoT)**? Why can the simple phrase "Let's think step by step" significantly improve the model's performance on mathematical or logical reasoning tasks? What is the underlying principle?

**Your Answer:**
> Chain of Thought is the technique to force LLM to think more details before outputing the final result. Since LLM is generated token by token. Force LLM to decompse the question and show the thinking details can generated more tokens for next token to reference. Which can improve the correctness of logic and reduce halluciation.

**AI Feedback:**
*   ✅ **What's Right:** Your intuition about "generating more tokens for next token to reference" is technically very accurate. This provides the model with **extended working memory**.
*   🛠 **Needs Improvement:** Distinguish between **Zero-shot CoT** (just adding "Let's think step by step") and **Few-shot CoT** (showing the model *how* to reason through examples).

**Corrected & Refined (AI Revision):**
> **Chain of Thought (CoT)** prompts the model to generate intermediate reasoning steps before the final answer.
> *   **The Principle:** In a Transformer, every generated token can attend to all previous tokens. By writing out the steps, the model creates a **"scratchpad"** in its context window. The final answer token is then conditioned on these logical steps, rather than just the question.
> *   **Why it works:** It breaks a complex mapping ($Input \to Output$) into several smaller, manageable mappings ($Input \to Step 1 \to Step 2 \to Output$).

**Corrected & Refined (AI Revision):**
> 

---

### 3. Advanced Prompting Strategies (ToT, Self-Consistency)
**Question:** 
In addition to CoT, please briefly describe how **Tree of Thoughts (ToT)** and **Self-Consistency** work. What limitations of CoT do they each address?

**Your Answer:**
> Tree of thoughts improves the CoT by forcing the LLM to think in a Tree search style, it first go to one branch (option) to explore the possible anwser, then after reaching the desired depth, it goes back to the original question and tries to search other branch, after together more options to the origianl question. It can generally output a more consolidate answer to user.
> 
> I don't know what self-consistency is. Sound like need LLM to think over the thinking output one more time to make sure it aligns with what it thought before.

**AI Feedback:**
*   ✅ **What's Right:** Correct on ToT's search-based nature (exploring multiple paths).
*   🛠 **Needs Improvement:** **Self-Consistency** is actually simpler: you run the *same* CoT prompt multiple times and take the **Majority Vote** (the answer that appears most often). It fixes the "random error" problem where a model follows a perfect logic but makes a single math slip-up.

**Corrected & Refined (AI Revision):**
> *   **Tree of Thoughts (ToT):** Solves the linear limitation of CoT. It allows the model to generate multiple "thoughts" at each step, evaluate them, and **look ahead** or **backtrack** if a path seems wrong.
> *   **Self-Consistency:** Generates multiple independent reasoning paths (e.g., 5-10 different CoT runs). If 7 out of 10 paths reach "Answer: 42", that answer is chosen. This significantly improves reliability in math and symbolic logic.

**Corrected & Refined (AI Revision):**
> 

---

*💡 Tip: When answering Question 2, think about how the model, while generating intermediate reasoning steps, uses these "thought processes" to influence the probability distribution of the final Token.*

---

## 🤖 AI Assistant Review & Deep-Dive Reference

### 1. In-Context Learning (ICL) vs. Fine-tuning
*   **ICL (Few-shot):** No weights are changed. The model "learns" from the context window. It is fast and flexible but limited by context length and token costs.
*   **Fine-tuning:** Model weights are updated. It handles millions of examples and requires no extra tokens per prompt, but it is slow and expensive to perform.

### 2. The Scaling Law of Reasoning (The "Thinker" LLMs)
Recent models like **OpenAI o1** or **DeepSeek-R1** have internalized CoT.
*   They are trained using **Reinforcement Learning (RL)** to reward "good" reasoning chains.
*   They don't just "output" CoT because you asked; they have learned that thinking longer leads to higher rewards. This is often called **"Inference-time Compute"**—spending more time (tokens) to get a better answer.

### 3. Comparison of Prompting Paradigms
| Technique | Best For | Mechanism |
| :--- | :--- | :--- |
| **Zero-shot** | General tasks | Direct instruction. |
| **Few-shot** | Specific formats/styles | Learning by example (ICL). |
| **CoT** | Logic, Math | Sequential "Scratchpad" reasoning. |
| **Self-Consistency** | Math Accuracy | Majority voting over multiple paths. |
| **ToT** | Creative/Strategic Planning | Tree search (DFS/BFS) + Evaluation. |

> [!IMPORTANT]
> **The "Lost in the Middle" Problem:** For Few-shot prompting, research shows that models are best at following the *first* and *last* examples. Middle examples are often ignored. When designing few-shot prompts, put your most important or complex examples at the ends!

---
*Next Step: Week 1 Day 4 - Prompt Safety (Injection/Jailbreak) & Hallucination Mitigation.*
