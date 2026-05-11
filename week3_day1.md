# Week 3 Day 1: AI Agent Foundations & Reasoning 🤖🧠

Welcome to Week 3! Having mastered RAG, we now step into the world of **AI Agents**. While RAG gives models "knowledge," Agents give them "agency"—the ability to reason, plan, and interact with the world through tools. Today, we cover the core architecture of Agents, the legendary **ReAct** framework, and the technical backbone of tool use: **Function Calling**.

## 📚 Recommended Learning Resources
- **Lilian Weng's Blog (Must Read):** [LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/) (The "Bible" of Agent design)
- **ReAct Paper:** [ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629)
- **OpenAI Documentation:** [Function Calling](https://platform.openai.com/docs/guides/function-calling)
- **LangChain Concepts:** [Agents](https://python.langchain.com/docs/concepts/agents/)
- **Hugging Face Blog:** [Open-Source Agents](https://huggingface.co/blog/open-source-llms-as-agents)

---

Please try to answer the following 3 questions in your own words:

---

### 1. The Core Architecture of an AI Agent
**Question:** 
What is an AI Agent? Briefly explain its four core components: **Brain (LLM)**, **Planning**, **Memory**, and **Tools/Action**. How do they work together to solve a complex task?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 2. The ReAct Framework (Reasoning + Acting)
**Question:** 
Explain the workflow of the **ReAct** framework. Why is the "Thought" step critical? How does ReAct prevent the "hallucination loops" that simple CoT (Chain of Thought) might fall into when interacting with external environments?

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

### 3. Function Calling Mechanism
**Question:** 
Is **Function Calling** the model "executing code"? Explain the technical process from the developer defining a tool (JSON Schema) to the model generating a response, highlighting the roles of the **Model**, the **Orchestrator (Application Code)**, and the **External API**.

**Your Answer:**
> 

**Corrected & Refined (AI Revision):**
> 

---

## 🤖 AI Assistant Review & Deep-Dive Reference

### 1. Agent Architecture: The "Cognitive Loop"
- **Brain (LLM):** The core reasoning engine. It processes context and decides "what to do next."
- **Planning:** 
    - **Task Decomposition:** Breaking a goal ("Plan a trip to Tokyo") into sub-tasks (Flight -> Hotel -> Itinerary).
    - **Reflection:** Evaluating past actions to correct mistakes (Self-Correction).
- **Memory:** 
    - **Short-term:** The immediate conversation context (Instruction, few-shot examples).
    - **Long-term:** Using a Vector DB to retrieve specific history or domain knowledge (RAG-enhanced memory).
- **Tools (Action):** The agent's hands. APIs, Code Interpreters, or Search Engines that allow it to impact the physical or digital world.

### 2. ReAct: The "Thought-Action-Observation" Loop
ReAct forces the model to verbalize its reasoning before acting.
- **Thought:** "I need to find the current price of NVDA to calculate the portfolio value."
- **Action:** `search_finance(ticker="NVDA")`
- **Observation:** "NVDA is trading at $120.50."
**Why it matters:** Without "Thought," models often jump to conclusions. By grounding the next "Thought" in the "Observation," the model stays tethered to reality.

### 3. Function Calling: The "Structured Handshake"
Function calling is **not** code execution; it is **structured generation**.
1. **Definition:** You send a JSON Schema describing `get_weather(city, unit)`.
2. **Detection:** The model identifies the need for this tool and outputs a structured JSON object: `{"name": "get_weather", "arguments": "{\"city\": \"London\"}"}`.
3. **Execution:** **Your code** (the orchestrator) receives this JSON, calls the real API, and gets the result.
4. **Resubmission:** You send the API result back to the model as a "Tool Message."
5. **Final Output:** The model uses the result to answer the user: "The weather in London is currently 15°C."

---
*Next Step: Week 3 Day 2 - Memory & Agent Frameworks (LangChain vs. LlamaIndex).*
