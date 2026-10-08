# Week 6 Day 1: Deep Reasoning Models & Test-Time Compute (DeepSeek-R1 & o1) 🧠

### 📚 Curated Resources
- [arXiv: DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning](https://arxiv.org/abs/2501.12948)
- [GitHub: DeepSeek-R1 Official Repository](https://github.com/deepseek-ai/DeepSeek-R1)

---

### 1. Test-Time Compute Scaling & Reasoning RL
**Question:** What is **Test-Time Compute Scaling** (System 2 reasoning), and how does large-scale Reinforcement Learning (such as DeepSeek-R1-Zero using GRPO without initial supervised warm-up/SFT) incentivize emergent self-reflection, backtracking, and "Aha moments"?

**Your Answer:**


**Corrected & Refined:**

### 1. Test-Time Compute Scaling（推理阶段算力扩展 / 系统 2 思维）
- **核心定义**：传统自回归模型（系统 1 / 快思考）在生成每个 token 时计算开销固定，依赖预训练时内化的直觉模式。**Test-Time Compute Scaling** 是指在**推理（测试）阶段**允许模型消耗更多动态算力（生成更长隐式思维链 CoT、搜索多条解题路径、自我验证与纠错），将“思考时间/Token 预算”直接转化为高难度任务（竞赛数学、复杂算法、长链逻辑推理）的准确率提升。
- **Scaling 范式转移**：从单纯依赖模型参数量与预训练数据的 Pre-training Scaling，拓展至 **Inference-Time Scaling（思考越久、探索越深，表现越好）**。

---

### 2. 纯强化学习机制（DeepSeek-R1-Zero 与 GRPO）
DeepSeek-R1-Zero 证明了**无需冷启动监督微调（Zero Cold-Start SFT）**，仅通过大规模强化学习即可激发出极致推理能力：
1. **基于规则的奖励建模（Rule-Based Reward Modeling）**：
   - 在数学与编程等客观可验证领域，彻底弃用容易被钻空子（Reward Hacking）且含噪声的神经判别器（Critic / Neural Reward Model）。
   - 采用纯规则判定：**准确率奖励（Accuracy Reward）**（如 LeetCode 测试用例、数学最终答案匹配）+ **格式奖励（Format Reward）**（强制要求 `<think>...</think>` 结构）。
2. **群组相对策略优化（GRPO, Group Relative Policy Optimization）**：
   - 对每个 Query 采样一组输出（如 $G$ 个候选 Trajectories），以该组的奖励均值与标准差计算相对优势（Relative Advantage），省去显存极其沉重的 Critic 网络。

---

### 3. 自我反思、回溯与“顿悟时刻（Aha Moment）”的涌现机制
- **涌现原因（Incentive Loop）**：
  - 高难度任务单向前进极易步入死胡同，单次成功率低。
  - 在大范围随机探索中，偶尔产生的**“回溯检查、推翻错误假设、切换策略”**的轨迹最终拿到了 +1 的奖励，而一条路走到黑的轨迹拿到 0。
  - 策略梯度（Policy Gradient）强力放大这类带有“反思与验证”模式的生成概率，导致模型的推理序列（CoT）自发膨胀（从数百 token 暴涨至数万 token）。
- **现象表现（Aha Moment）**：
  - 模型在没有任何人类示范的情况下，自发学会了人类式的元认知话术（如 *"Wait, wait, let me re-evaluate this step..."*、*"Wait, this contradicts lemma 1, let me backtrack."*），甚至中途否定前文结论并开辟全新解法，呈现出“顿悟”式的自我纠错行为。

---

### 💡 面试核心速记（30 秒答题总结）
> “Test-Time Compute Scaling 是将计算预算转移到推理端，通过长思维链提升复杂逻辑能力。DeepSeek-R1-Zero 通过‘纯规则奖励（如数学/单测客观对错）+ GRPO 算法’，在没有人类 SFT 示范的情况下进行纯 RL 探索。因为只有具备自我检验与回溯纠错的思维路径才能最终解对难题并获得奖励，模型自发涌现出了长思维链、主动回溯与‘顿悟’式的反思能力。”
