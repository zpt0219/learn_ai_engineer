# AI Engineer 面试八股学习计划 🚀

欢迎开启 AI Engineer 学习之旅！现在的 AI Engineer (AIE) 往往需要结合大语言模型 (LLM) 进行应用开发，因此面试不仅考察传统的机器学习/深度学习基础，更偏向于大模型工程化、RAG、Agent、Prompt Engineering 以及模型微调部署。

本计划将常见的面试题划分为多个核心模块，并为你规划了**每天 3 题**的学习节奏。你可以在此文档中记录你的学习进度（例如在题目后标记 `[x]` 或记录笔记链接）。

## 🧠 学习模块划分

1. **Module 1: 大语言模型基础 (LLM Basics)** (参数结构、注意力机制、Token化)
2. **Module 2: 提示词工程 (Prompt Engineering)** (技巧、思维链、防注入)
3. **Module 3: RAG (检索增强生成)** (向量数据库、Chunking、召回策略、重排)
4. **Module 4: Agent (智能体) 与工具调用** (ReAct、Function Calling、Memory)
5. **Module 5: 评测与工程化** (评测指标、大模型推理加速、幻觉处理)
6. **Module 6: 模型微调基础** (SFT、PEFT、LoRA 概念)

---

## 📅 每日学习打卡 (Daily Plan)

### 第 1 周：大模型基础与提示词工程
*目标：理解 LLM 的基本原理及如何更好地与其交互。*

**Day 1: Transformer & LLM 基础**
- [ ] 1. 什么是 Transformer 模型？简述其核心架构（Encoder-Decoder vs Decoder-only）。为什么现在的 LLM 大多是 Decoder-only？
- [ ] 2. 什么是 Attention (注意力机制) 和 Self-Attention？它们解决了什么问题？
- [ ] 3. 什么是 Token 和 Tokenization？BPE、WordPiece 算法有什么区别？

**Day 2: LLM 上下文与机制**
- [ ] 1. 什么是 KV Cache？它在 LLM 推理中起到什么作用？
- [ ] 2. 大模型的 Context Window (上下文窗口) 是什么？目前有哪些扩展上下文长度的技术方案？
- [ ] 3. 什么是 Temperature (温度) 和 Top-p / Top-k？它们如何影响模型生成的输出？

**Day 3: Prompt Engineering 核心技巧**
- [/] 1. 什么是 Few-shot Prompting (少样本提示)？相比 Zero-shot 的优势是什么？
- [/] 2. 什么是 Chain of Thought (CoT, 思维链)？它为什么能提升大模型在推理任务上的表现？
- [/] 3. 除了 CoT，你还知道哪些进阶提示词策略？(如 Tree of Thoughts (ToT), Self-Consistency)

**Day 4: Prompt 安全与优化**
- [/] 1. 什么是 Prompt Injection (提示词注入) 和 Jailbreak (越狱)？如何防御？
- [/] 2. 如果大模型输出的 JSON 格式不稳定，除了要求 "Output strictly in JSON"，还有哪些工程或模型层的解决办法？(如 System Prompt 强化、Function Calling、JSON Schema 约束)
- [/] 3. 什么是大模型的“幻觉” (Hallucination)？在工程上通常用哪些手段来减少或缓解幻觉？

### 第 2 周：RAG 核心技术详解
*目标：掌握 Retrieval-Augmented Generation (检索增强生成) 的各个环节。*

**Day 5: RAG 基础与文档解析**
- [ ] 1. 什么是 RAG？请简述 RAG 的标准流程 (解析、切分、向量化、检索、生成)。相比直接微调，RAG 有什么优缺点？
- [ ] 2. 文档切分 (Chunking) 的策略有哪些？如何选择合适的 Chunk Size 和 Overlap？
- [ ] 3. 在处理 PDF 或复杂网页 (含表格、图片) 时，常遇到哪些解析痛点？有哪些主流的解析工具和优化思路？

**Day 6: 向量化与 Embedding**
- [ ] 1. 什么是 Embedding 模型？它和传统的词袋模型有什么区别？
- [ ] 2. 什么是向量数据库？有哪些常见的向量数据库 (如 Chroma, Pinecone, Milvus, Qdrant)？其核心检索算法 (如 HNSW) 的大致原理是什么？
- [ ] 3. 在多语言场景或特定垂直领域，如果开源的 Embedding 模型效果不好，应该如何处理？

**Day 7: 高阶检索策略**
- [ ] 1. 什么是混合检索 (Hybrid Search)？(如 稀疏检索 BM25 + 密集检索 Dense Retrieval) 为什么要用混合检索？
- [ ] 2. 什么是 Rerank (重排)？为什么在检索后需要接一个 Reranker 模型进行二次排序？
- [ ] 3. 简述常见的 Query 优化策略（如 Query Expansion(查询扩展), Query Rewrite(查询重写), HyDE）。

### 第 3 周：Agent 开发与微调部署
*目标：理解 AI Agent 的运作机制，了解模型评估及微调基础。*

**Day 8: Agent (智能体)**
- [ ] 1. 什么是 AI Agent？请说明 Agent 的核心组件 (如 Profile/Persona, Memory, Planning, Tools/Action)。
- [ ] 2. 什么是 ReAct (Reasoning and Acting) 框架？它的工作流是怎样的？
- [ ] 3. 什么是 Function Calling (工具调用)？大模型是如何学会并且执行外部工具的？

**Day 9: Memory 与 Agent 框架**
- [ ] 1. 在 Agent 应用中，短期记忆 (Short-term Memory) 和长期记忆 (Long-term Memory) 有什么区别？通常如何实现？
- [ ] 2. 简述 LangChain 和 LlamaIndex 这两个框架的核心区别及适用场景。
- [ ] 3. 什么是 Multi-Agent (多智能体) 系统？有哪些代表性框架 (如 AutoGen, CrewAI)？它们解决单 Agent 的什么痛点？

**Day 10: 模型评测与工程部署**
- [ ] 1. RAG 系统的效果应该如何评测？(如 RAGAS 框架中的 Faithfulness, Answer Relevance 等指标)
- [ ] 2. 在部署大语言模型时，vLLM 是什么？它最重要的 PagedAttention 机制解决了什么问题？
- [ ] 3. 什么是 LoRA (Low-Rank Adaptation) 微调？相比于全量微调 (Full Fine-Tuning)，它的优势是什么？

---

## 📝 如何使用此文档记录进度

1. 每天挑选 3 个问题进行深入学习。
2. 可以在每个问题下方直接写下你的理解和答案总结，也可以附上参考链接。
3. 如果当天学习遇到了难点，随时呼叫我，我会为你详细解答并提供通俗易懂的例子！
