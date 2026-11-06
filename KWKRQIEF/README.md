# TTT / 大模型在线调权重 —— 论文清单（含综述）

> 主题：模型在**使用中（推理/服务时）更新参数**，而不是训练完就冻结。
> 下载时间：2026-11-06 ｜ 全部为 arXiv 官方 PDF，已逐篇校验标题

---

## 零、综述（先读这些建立框架）

| 文件 | 论文 | 覆盖范围 |
|---|---|---|
| `00a_Survey-TTA_2303.15361.pdf` | **A Comprehensive Survey on Test-Time Adaptation under Distribution Shifts**（Liang, He, Tan；IJCV 2024） | **最系统的 TTA 综述**（33 页）：把 TTT 作为其中一族方法，讲清"测试时用什么信号、更新什么、如何评估" |
| `00d_Survey-Beyond-Model-Adaptation_2411.03687.pdf` | **Beyond Model Adaptation at Test Time: A Survey**（2024-11） | 比 TTA 更宽：测试时的**模型适配**全景（含输入侧/输出侧/参数侧） |
| `00e_Survey-CL-LLMs-2024_2404.16789.pdf` | **Continual Learning of Large Language Models: A Comprehensive Survey**（2024） | **LLM 持续学习**：在线调权重、灾难性遗忘、巩固、回放（44 页） |
| `00c_Survey-ContinualLearning-LLMs_2603.12658.pdf` | **Beyond Static Models: An Evolving Framework for Continual Learning in LLMs across Training Stages**（2026-03） | 2026 新综述：按**训练阶段**组织 LLM 持续学习（预训练/指令/对齐/部署） |

> ⚠️ 说明：截至 2026-11，**还没有一篇专门叫"Test-Time Training for LLMs: A Survey"的权威综述**。最接近的是上面四篇（TTA 综述 + 模型适配综述 + 两篇 LLM 持续学习综述）。另外 ICLR 2026 的 **"It's All Connected: Test-Time Memorization, Attentional Bias, Retention, and Online Optimization"** 是一篇"统一视角"的 position paper，适合当"准综述"读。

---

## 一、核心必读（4 篇）

| 文件 | 论文 | 关键点 |
|---|---|---|
| `01_In-Place-TTT_2604.06169.pdf` | **In-Place Test-Time Training**（ICLR 2026 Oral, ByteDance） | 把 **MLP 最后一层投影**当 fast weights，drop-in 给现有 LLM；目标与 next-token prediction 对齐；chunk 级更新；4B 模型 128k 上下文 |
| `02_TTT-Layers_2407.04620.pdf` | **Learning to (Learn at Test Time): RNNs with Expressive Hidden States**（Sun et al.） | **隐藏状态 = 在线训练的小模型**；TTT-Linear / TTT-MLP |
| `03_Titans_2501.00663.pdf` | **Titans: Learning to Memorize at Test Time**（NeurIPS 2025） | 用 **surprise（预测误差）** 触发在线梯度更新；短期+长期记忆；遗忘门 |
| `04_Decision-Theoretic-TTT_2606.15569.pdf` | **A Decision-Theoretic View of TTT: When, How Far, and Which Directions to Adapt**（ICML 2026） | 决策论框架回答"**该不该更新、更新多远、往哪更新**"——稳定性理论化 |

## 二、稳定性 / 防遗忘（4 篇）

| 文件 | 论文 | 关键点 |
|---|---|---|
| `05_LATTA_2510.05530.pdf` | **LATTA: Langevin-Anchored TTA** | Langevin 锚定，防止长序列适应漂移 |
| `06_RASP_2611.03513.pdf` | **RASP: Sparse-Plasticity Continual Learning in a Stationary-Size LoRA** | **稀疏可塑性**：只让"用到的"参数更新/增长——最接近"用进废退" |
| `07_Modular-TTT_2608.07110.pdf` | **Modular TTT: Rethinking TTT as Composable Modules** | 把 TTT 拆成可组合模块 |
| `08_MultiCluster-Memory-TTA_2603.21135.pdf` | **One Pool Is Not Enough: Multi-Cluster Memory for TTA** | 长时间适应下不稳定 → 多簇记忆缓解 |

## 三、应用 / 系统（3 篇）

| 文件 | 论文 | 关键点 |
|---|---|---|
| `09_Policy-of-Thoughts_2601.20379.pdf` | **Policy of Thoughts: Scaling TTT for LLM Reasoning via Online Policy Evolution** | 把测试时训练用在**推理**上 |
| `10_MACE-serving_2510.03283.pdf` | **MACE: LLM Serving with SLO-aware Continuous Retraining Alignment** | 服务端**边服务边持续重训练**，带 SLO 与回滚 |
| `11_TTT-original-2020_1909.13231.pdf` | **Test-Time Training with Self-Supervision**（Sun et al. 2020） | TTT 的**奠基之作** |

---

## 阅读顺序建议

```
综述：00a（TTA 全景）→ 00e（LLM 持续学习）
入门：11（奠基）→ 02（TTT 层）→ 03（Titans）
落地：01（In-Place TTT）          ← 想直接用在 LLM 上就读这篇
稳定：04（理论）→ 05（锚定）→ 06（稀疏可塑性）→ 08（多簇记忆）
系统：07（模块化）→ 09（推理）→ 10（服务化）
```

## 持续跟踪

- **Awesome 列表**：`github.com/dereck0602/awesome_test_time_llms`
- **arXiv 检索建议**：
  - `cat:cs.LG AND (test-time training OR test-time adaptation)`
  - `cat:cs.CL AND (continual learning AND large language models)`
- **会议**：ICLR / NeurIPS / ICML 的 TTA & 持续学习专题；NeurIPS 2026 还有 "Towards Test-Time Continual Learning Agents" workshop

## 备注

- 01 是当前最值得先读的落地工作（ICLR 2026 Oral，代码开源：`github.com/ByteDance-Seed/In-Place-TTT`）；
- 06（RASP）与 05（LATTA）是"防止破坏已有知识"的最新代表；
- 若还需要 **持续学习经典**（EWC / SI / PackNet / CoTTA / EATA），可以再补一批。
