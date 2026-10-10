# Week 6 Day 2: Group Relative Policy Optimization (GRPO) vs. PPO ⚖️

### 📚 Curated Resources
- [arXiv: DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models (GRPO 原论文)](https://arxiv.org/abs/2402.03300)
- [arXiv: DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning](https://arxiv.org/abs/2501.12948)
- [Hugging Face: TRL GRPO Trainer Documentation](https://huggingface.co/docs/trl/main/en/grpo_trainer)

---

### 1. Group Relative Policy Optimization (GRPO) vs. PPO & Rule-Based Reward Modeling
**Question:** How does GRPO eliminate the Critic model in RLHF to drastically save memory, and how does rule-based reward modeling work for verifiable reasoning tasks?

**Your Answer:**


**Corrected & Refined:**

### 1. PPO 的架构痛点与显存瓶颈（The Critic Bottleneck）
在标准强化学习对齐（PPO）框架中，训练通常需要同时维护 **4 个模型实体**：
1. **Actor（策略模型 $\pi_\theta$）**：待训练的生成模型（参数 + 梯度 + 优化器状态）。
2. **Critic（价值模型 $V_\phi$）**：用于估计状态价值基线 $V(s)$ 并计算广义优势估计（GAE）。为了准确估计价值，Critic 通常具有与 Actor **同等规模的参数量**（如 70B 或 671B MoE），需要独立的显存、前向传播与反向传播。
3. **Reference Model（参考模型 $\pi_{ref}$）**：冻结权重的原始模型，用于施加 KL 散度惩罚，防止策略偏离过远。
4. **Reward Model（奖励模型 $R_\psi$）**：冻结权重的神经判别模型，为生成文本打分。

- **显存与计算代价**：Critic 模型的参数和 Adam 优化器状态（单参数需 16 字节 FP32 状态）占据了总显存的 ~50%。同时，在生成数万 token 的长思维链（CoT）时，Critic 难以精准估计每步状态价值，导致值函数训练极不稳定。

---

### 2. GRPO 的核心解法：组内相对优势估计（Group Relative Advantage）
GRPO（DeepSeekMath 提出并在 DeepSeek-R1 中大规模应用）直接**彻底丢弃了 Critic / Value 网络**，其核心逻辑如下：
1. **群体采样（Group Sampling）**：
   - 对于每个输入的 Prompt/Query $q$，策略模型 $\pi_\theta$ 并行采样生成一组候选输出序列：
     $$\mathcal{G} = \{o_1, o_2, \dots, o_G\} \quad (G \text{ 通常取 } 4 \sim 16)$$
2. **组内基线与归一化（In-Group Normalization）**：
   - 计算组内所有样本的标量奖励 $\{r_1, r_2, \dots, r_G\}$。
   - 不再依赖 Critic 预测的标量值 $V(q)$，而是直接以该组样本奖励的**均值**作为基线，**标准差**作为缩放尺度，计算每个样本的相对优势（Advantage）：
     $$A_i = \frac{r_i - \text{mean}(\{r_1, \dots, r_G\})}{\text{std}(\{r_1, \dots, r_G\}) + \epsilon}$$
3. **策略梯度优化**：
   - 优于平均水平的轨迹（$A_i > 0$）被正向强化，劣于平均水平的轨迹（$A_i < 0$）被抑制。
   - 结合 PPO-clip 风格的截断损失和对参考模型 $\pi_{ref}$ 的无偏/直接 KL 惩罚，稳定策略更新。

---

### 3. PPO 与 GRPO 核心架构对比

| 维度 | 传统 PPO (Proximal Policy Optimization) | GRPO (Group Relative Policy Optimization) |
| :--- | :--- | :--- |
| **模型数量** | 4 个 (Actor, Critic, Reference, Reward) | 2 个 (Actor, Reference; 若用规则奖励甚至无神经 Reward) |
| **基线机制 (Baseline)** | 独立 Critic 神经网络估计状态价值 $V(s)$ | 组内 $G$ 个采样的奖励均值 $\text{mean}(r)$ |
| **显存占用** | **极大**（Critic 模型及其优化器占巨量显存） | **节省 ~50% 显存**（无 Critic 参数与优化器状态） |
| **长思维链支持** | 困难（长序列 Value Function 极难收敛且爆显存） | **原生支持长序列**（显存省出来给超长 CoT 探索） |
| **奖励稳定性** | 易受 Critic 估计偏差与值漂移影响 | 组内自归一化，相对梯度更平稳 |

---

### 4. 基于规则的奖励建模（Rule-Based Reward Modeling）
在数学证明、竞技编程、形式化逻辑等客观可验证领域，DeepSeek 摒弃了传统人类偏好神经打分器（Neural RM），采用纯确定性规则判定：

1. **准确率奖励（Accuracy Reward）**：
   - **数学问题**：通过解析器提取答案（如 `\boxed{...}` 中的表达式），使用数学等价性引擎（如 SymPy、Math-Verify）与真值（Ground Truth）比对，完全等价给 $1$，错误给 $0$。
   - **代码问题**：将模型编写的完整代码部署到独立安全沙盒（Docker / WebAssembly），运行全量单元测试用例，全部通过给 $1$，报错/超时/未通过给 $0$。
2. **格式规范奖励（Format Reward）**：
   - 强制模型输出结构化思考过程，例如必须在 `<think>` 与 `</think>` 标签内完成推理，标签外输出最终结论。格式完全正确给正奖励，漏标/语法残缺扣分。
3. **防作弊与规则奖励优势**：
   - **彻底根除 Reward Hacking**：神经奖励模型容易被“长篇废话、谄媚话术、格式假装正确”所欺骗，而编译器和数学验证器无法被忽悠。
   - **极低算力开销**：规则验证为 CPU 轻量执行，无需 GPU 前向传播打分。

---

### 💡 面试核心速记（30 秒答题总结）
> “PPO 在大模型对齐中最沉重的包袱是同等规模的 Critic 网络，占用了近半显存且在长序列下极不稳定。GRPO 通过一次性为每个 Query 采样一组输出，直接用组内奖励均值和标准差归一化出相对优势，从而彻底砍掉了 Critic 模型，节省了超 50% 显存。配合数学和单测的‘纯规则奖励’，既消除了神经奖励模型的 Reward Hacking 漏洞，又将节省出的显存完全释放给长达数万 token 的深层思维链探索。”
