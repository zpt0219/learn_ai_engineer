# Week 4 Day 3: Advanced Reasoning & Next-Gen Paradigms

## 1. What is "Test-Time Compute" (or System 2 thinking) in the context of models like OpenAI o1? How does it differ from traditional inference?

**"Test-Time Compute" (System 2 Thinking)** refers to the paradigm where an AI model spends significantly more computational resources *during inference* (test time) to reason through a problem before generating the final output. OpenAI's o1 model is a prime example of this approach.

### How it differs from traditional inference:

*   **Traditional Inference (System 1):** Models like GPT-4 or Llama 3 generate text auto-regressively, predicting the next token based on the context. The compute spent per token is roughly constant, limited by the model's forward pass. It relies on the knowledge internalized during pre-training. It's fast, intuitive, and "reactive" (System 1 thinking).
*   **Test-Time Compute (System 2):** Models like o1 use hidden Chain-of-Thought (CoT), search algorithms (like MCTS - Monte Carlo Tree Search), and reinforcement learning techniques during inference. The model dynamically allocates more compute to difficult problems by generating intermediate reasoning steps, exploring multiple paths, verifying hypotheses, and self-correcting before giving the final answer. It is slower, deliberate, and capable of solving complex multi-step reasoning tasks that traditional inference would fail at.

### Key Advantages:
*   Unlocks performance scaling at inference time (the longer it "thinks," the better the answer).
*   Significantly higher accuracy on complex math, coding, and logic puzzles.

---

## 2. What are Process Reward Models (PRM) vs. Outcome Reward Models (ORM) in reinforcement learning for reasoning tasks?

When training models using Reinforcement Learning (like RLHF) for complex reasoning, we need a Reward Model to evaluate the AI's generated answers. PRM and ORM represent two different reward strategies.

### Outcome Reward Models (ORM)
*   **Concept:** The model is evaluated and rewarded *only at the final outcome*. If the final answer is correct, it gets a positive reward; if incorrect, a negative reward.
*   **Pros:** Easy to implement and collect data for. You only need to know the final answer (e.g., checking if the final math answer is "42").
*   **Cons:** Sparse rewards. For a long, multi-step math problem, if the model makes a calculation error at step 2 but completes 10 steps, the ORM just says "wrong." The model doesn't know *which* step was wrong, making it hard to learn complex, long-horizon reasoning.

### Process Reward Models (PRM)
*   **Concept:** The model is evaluated and rewarded *at each intermediate step* of its reasoning process.
*   **Pros:** Dense and precise feedback. It explicitly encourages correct reasoning paths. If the model makes a mistake at step 2, the PRM penalizes that specific step, preventing the error from cascading. PRMs have been shown to significantly improve performance on complex math and logic tasks compared to ORMs.
*   **Cons:** Expensive and difficult to train. It requires human annotators (or very capable AI) to label the correctness of *every single intermediate step* in a solution, which is highly labor-intensive.

---

## 3. How do you design an AI system for complex math or coding tasks that requires continuous verification and self-correction?

Designing an AI system for rigorous tasks requires moving beyond zero-shot generation and implementing an agentic architecture with continuous feedback loops.

### Key Architectural Components:

1.  **Iterative Reasoning Loop (ReAct / Reflection):**
    *   The system shouldn't just output an answer. It should generate a plan, write intermediate code/math steps, and reflect on its own output.
    *   **Self-Reflection:** Prompt the model to analyze its previous steps for logical errors before proceeding.

2.  **Execution Sandbox (The Verifier):**
    *   For coding: Integrate a secure sandbox (like Docker or WebAssembly) where the generated code can be compiled and executed safely.
    *   For math: Use a Python REPL (e.g., SymPy) or a math engine to perform symbolic calculations, preventing basic arithmetic hallucinations.

3.  **Automated Testing & Feedback:**
    *   **Test-Driven Development (TDD) approach:** First, prompt the LLM to generate unit tests based on the requirements.
    *   Execute the generated code against these tests.
    *   If the tests fail or throw a syntax error, capture the traceback/error message and feed it back to the LLM.

4.  **Self-Correction Mechanism:**
    *   Create a specific prompt template for correction: *"Your previous code failed with this error: `[Error Traceback]`. Analyze the cause and rewrite the code to fix it."*
    *   Set a maximum retry limit (e.g., 5 iterations) to prevent infinite loops.

5.  **Multi-Agent Collaboration:**
    *   **Generator Agent:** Writes the initial solution or code.
    *   **Reviewer/Verifier Agent:** Critiques the code, looks for edge cases, or writes tests.
    *   The two agents iterate until the Reviewer approves or tests pass.

### Workflow Example:
*User Request -> Generator writes Code & Tests -> Sandbox Runs Tests -> Output is Error -> Reviewer analyzes Error -> Generator writes fixed Code -> Sandbox Runs Tests -> Success -> Return Final Answer.*
