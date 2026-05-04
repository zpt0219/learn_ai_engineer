# Day 4: Prompt 安全与优化 🛡️

欢迎来到第 4 天！今天我们将关注 Prompt 的“实战落地”：如何让模型输出更稳定、如何抵御恶意攻击，以及如何处理大模型最令人头疼的问题——幻觉。

## 📚 推荐学习资源
在开始回答之前，这些资源可能会对你有所帮助：
- **Prompt 安全:** [OWASP Top 10 for LLM (权威的 LLM 安全指南)](https://genai.owasp.org/llm-top-10/)
- **结构化输出 (JSON):** [OpenAI Structured Outputs Guide](https://platform.openai.com/docs/guides/structured-outputs)
- **幻觉 (Hallucination) 缓解:** [Survey of Hallucination in LLMs (论文综述/博客摘要)](https://huggingface.co/blog/zh/not-all-hallucinations-are-equal)

---

请尝试用你自己的语言回答以下 3 个问题：

---

### 1. Prompt Injection (注入) 与 Jailbreak (越狱)
**问题：** 
什么是 **Prompt Injection** 和 **Jailbreak**？它们之间有什么区别？作为开发者，你有哪些手段可以防御这些攻击？

**你的回答：**
> 

**Corrected & Refined (AI 修订):**
> 

---

### 2. 结构化输出稳定性 (JSON)
**问题：** 
在开发 AI 应用时，如果模型输出的 JSON 经常格式错误或丢失字段，你会采取哪些工程手段来确保输出的稳定性？（请至少列出 3 种方案，如 Function Calling, JSON Schema 约束等）

**你的回答：**
> 

**Corrected & Refined (AI 修订):**
> 

---

### 3. 幻觉处理 (Hallucination Mitigation)
**问题：** 
什么是大模型的**“幻觉”**？在工程实践中，除了优化 Prompt，还有哪些主流的技术手段（如 RAG, N-step verification, 外部工具校验）可以显著缓解幻觉？

**你的回答：**
> 

**Corrected & Refined (AI 修订):**
> 

---

*🛡️ 小贴士：对于安全问题，可以思考一下“数据”与“指令”在 LLM 输入中是如何混合在一起的，这正是注入攻击的根源。*
