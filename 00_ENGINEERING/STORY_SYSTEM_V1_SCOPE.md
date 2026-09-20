# 银河 Story System V1 目标与收口方案 v0.2

**日期：2026-09-20**  
**用途：重启苹果与李光协作，并尽快建立“合格故事生产线”**  
**状态：Owner 一级目标与 V1 产品边界已冻结；工程基线、Validation 分级与模型选择进入执行准备**

---

## 0. 方案结论

本轮不再以“完成完整 Story Production System”为目标，也不再以“把历史 PR 串全部合入 main”为完成标志。

### Owner 已冻结的最高级决策

1. **第一里程碑：好故事。**
2. **单次 Story Production 的输出边界：Accepted Story。**
3. **V1 项目完成条件：达到 M3——能够小批量、可重复地生产 Accepted Story。**

因此必须区分：

> **Accepted Story 是一次生产任务的终点；不是整个 V1 项目的完成宣告。**

V1 的业务目标是：

> **把李晶晶这批教案，以可接受的模型调用次数、人工复审工时和工程介入成本，稳定生产成高质量、有银河世界观、教学内容准确、角色自然、可被内容 Reviewer 接受的故事。**

Image / TTS / Assemble / H5 / Player 继续作为既有下游生产能力保留，但不属于 Story V1 的完成条件。

---

# 1. 为什么现在要收口，而不是继续扩建

现有事实已经足够说明：

- 改造前基线已具备教案 → 文本 → 图 → 音 → deck 的完整生产能力；
- 最近两周没有重做整个系统，40+ HTTP 端点基本保持不变；
- 主要改造集中在 `/pages/generate` 前后的 Contract / Gate / Evidence 层；
- L22 的 15 次付费运行全部使用 `gemini-2.5-flash`，Story Model 变量从未真正测试；
- 多数失败已经不是“系统没有规则”，而是“模型没有稳定遵守已经明确的规则”；
- 当前 Gate 已开始反向塑造故事表达，证明需要重新区分“真实缺陷”和“是否必须硬阻断”；
- “故事是否真的好”仍主要依赖人工判断，而不是现有 Gate。

因此本轮必须从：

> **继续完善完整受控生产系统**

切换为：

> **保留必要工程控制，尽快建立可重复获得 Accepted Story 的生产线。**

---

# 2. 两个不同的“终点”

这是 V1 必须统一的口径。

## 2.1 单次生产输出终点

```text
一次课程生产
    ↓
Accepted Story
```

当一门课得到合格的 `Accepted Story Package`，本次 **Story Authoring** 任务结束。

图片、TTS、页面、H5、App 不影响“这个故事是否已经被接受”。

## 2.2 V1 项目完成条件

V1 不能在 L22 首次 Accepted 后宣布完成。

V1 项目按三个里程碑推进：

- **M1：能做出一篇好故事**
- **M2：同一生产规则连续做出三篇好故事**
- **M3：小批量生产成立**

只有 M3 达成，才认为：

> **银河“合格故事生产线”V1 成立。**

---

# 3. V1 系统边界

```text
李晶晶教案
    ↓
Course Parse / Question Bank
    ↓
Understand
    ↓
Approved Inputs + 必要 Galaxy Context
    ↓
Story Planning / Story Beats
    ↓
Story Generation
    ↓
Validation
    ↓
Human Story Review
    ↓
========================
   ACCEPTED STORY PACKAGE
========================
        ↑
  单次 Story Production 到此结束


Accepted Story Package
    ↓
Page Adapter / Image / TTS / Assemble / H5 / App
    ↓
Downstream Delivery
```

## 3.1 V1 内部

V1 只负责：

- 正确理解课程；
- 保留课程关键内容与题目身份；
- 注入必要世界观、角色与上下文；
- 形成 Story Profile / Story Beats；
- 生成故事；
- 检查不可接受错误；
- 完成人类内容验收；
- 输出 Accepted Story Package。

## 3.2 V1 外部

以下属于 Downstream Delivery，不阻塞 Story V1：

- 图片生成；
- TTS；
- Asset Binding；
- Assemble；
- H5 / Player；
- App 发布；
- 完整 Navigation Contract；
- Runtime E2E。

既有能力继续保留，不要求为 V1 重建。

---

# 4. V1 Baseline Manifest：唯一工程起点

V1 不允许继续从 #136 或任意历史 stacked PR 直接向前“接着修”。

## 4.1 基线原则

新的 V1 工作必须：

> **从 `main` 的已知稳定生产基线出发，仅提取 V1 确认需要的能力。**

历史 #136–#162 campaign 统一作为：

> **Historical Campaign / Parts Warehouse**

不得成为新的 V1 baseline。

## 4.2 Source Commit 规则

在 V1 首个实现 Issue 开工前，由 Engineering Owner + Independent Reviewer 建立并确认唯一 `V1 Baseline Manifest`。

Manifest 必须至少记录：

| 字段 | 要求 |
|---|---|
| Source branch | `main` |
| Source commit | 精确 SHA，必须 CONFIRMED |
| Production-code baseline | 若当前 main 仅比 `0db99891` 多治理文档，需同时标明生产代码基线 |
| Course Source version | L22 / L23 / L24 对应教案版本 |
| Approved Input version | 精确文件/版本 |
| Story Profile version | 精确版本 |
| Story Beats version | 精确版本 |
| Prompt version | 精确 hash / commit / artifact |
| Story Model | A/B 结束后冻结 |
| Runtime parameters | temperature 等关键参数 |
| Validation set | 当前启用的 BLOCK / REVIEW / WARN / INFO |
| Extracted capabilities | 从旧 campaign 最小提取的能力 |
| Explicit exclusions | 明确不带入 V1 的旧能力 |
| Evidence format | run / candidate / review 的最小留证格式 |

## 4.3 Baseline Manifest 的治理规则

- M1 可以在 manifest 建立后进入调试；
- 一旦进入 M2 计数，代码 / Prompt / Policy / Gate 版本必须冻结；
- 任何修改都会产生新的 Baseline Manifest 版本；
- 不允许口头说“基本一样”；
- 不允许用“当前工作区”代替精确 commit；
- 不允许把旧 stacked branch 当事实源。

---

# 5. Accepted Story Package：最小产物合同

Accepted Story 不再允许只是“某个 Markdown、JSON 或 pages 文件看起来不错”。

V1 采用一个**最小故事产物合同**，但避免把它扩建成平台 Schema。

推荐物理形态：

```text
accepted_story/
  story.md               # 人可读故事正文
  story_manifest.json    # 最小结构化元数据与追踪信息
```

也可以用等价 JSON / pages 实现，但必须覆盖以下语义。

## 5.1 必须包含的内容

### A. Story Body

- 章节 / Scene；
- 正文 / 对话；
- 必要选择或互动语义（如果课程本身要求）。

### B. World References

至少可追踪：

- 使用到的角色 ID / 名称；
- 使用到的地点 ID / 名称；
- 必要 Canon / Context 版本。

### C. Course Evidence

故事中的课程事实必须能够回溯到：

- Course Source；
- Approved Input；
- knowledge ref / evidence ref。

**不要求正文每一句都显示引用，但必须能追踪关键教学内容。**

### D. Question Tracking

每道应保留题目必须有：

- question_id；
- source；
- 当前状态；
- 建议插入位置 / scene anchor；
- 是否已经进入 Story / downstream interaction。

重要原则：

> **题目不必生硬写进故事正文，但绝不能静默丢失。**

### E. Generation Identity

至少记录：

- source commit；
- Prompt / Policy version；
- Story Profile version；
- Story Beats version；
- Story Model；
- runtime parameters。

### F. Review Record

至少记录：

- deterministic / hard validation 结果；
- narrative review 结果；
- Reviewer；
- `ACCEPTED / REVISION_REQUIRED / REJECTED`；
- 必要人工备注。

---

# 6. Accepted Story 的质量定义

Accepted Story **不是 Gate 全绿的同义词**。

一个故事至少要通过四层判断。

## A. 硬正确性

不得出现：

- 教学核心内容错误；
- 明确违反 Canon / Approved Input 的事实；
- 题目身份丢失、篡改或不可追踪；
- 明确课程边界越界；
- 明显跨章回流或关键连续性冲突；
- 结构错误导致故事无法正常消费。

这类问题原则上允许机器直接 BLOCK。

## B. 世界观与角色成立

故事应满足：

- 银河世界锚点自然存在；
- 角色身份与关系不被破坏；
- 伙伴不是单纯教学话筒；
- 不虚构未经授权的共同经历；
- 角色行为符合已知关系与当前故事状态。

明确事实违反可以 BLOCK；模糊语义判断进入 REVIEW / WARN。

## C. 故事质量

必须判断：

- 是否好看；
- 是否自然；
- 是否有继续阅读欲望；
- 节奏是否舒服；
- 角色是否鲜活；
- 教学内容是否真正融入故事；
- 是否有“银河自己的故事感”；
- 是否存在明显“为了过 Gate 而说话”。

这一层不能由字符串 / 正则 Gate 替代。

## D. Acceptance

### M1 / M2

Golden 阶段由 Owner 逐篇批准。

### M3 及后续量产

逐步转为：

- 指定内容 Reviewer 按统一量规批准；
- Owner 做抽样；
- Owner 处理低置信度案例；
- Owner 处理规则外例外；
- Owner 处理 Canon / 产品原则变更。

V1 的长期方向不是让 Owner 成为每一篇故事的永久人工 Gate。

---

# 7. Validation / Gate：统一四级术语

V1 统一采用：

| 等级 | 含义 | 处理 |
|---|---|---|
| **BLOCK** | 可确定、不可接受的错误 | 自动拒绝 |
| **REVIEW** | 有真实风险，但需要语义或人工判断 | 必须复审 |
| **WARN** | 非阻断质量问题 | 允许继续，记录 |
| **INFO** | 观察项 / 统计项 | 不影响生产 |

> **INFO 即此前沟通中使用过的 OBSERVE。V1 后续统一只使用 INFO。**

## 7.1 BLOCK 的适用边界

只有以下类型原则上可以直接 BLOCK：

- 确定性结构错误；
- closed-set / ID / schema 明确错误；
- 明确 Canon 事实冲突；
- 明确 Course / Approved Input 冲突；
- 明确题目身份丢失或结构破坏；
- 明确不能被 downstream 消费的数据错误。

以下情况**不得仅凭关键词命中自动视为 BLOCK**：

- 疑似课程越界；
- 角色是否自然；
- 某句是否“像教学话筒”；
- 问号数量；
- 陈述句比例；
- 模糊叙事质量；
- 需要语境才能判断的历史 / 关系表达。

这些应进入 REVIEW / WARN 或由更可靠的语义判断承担。

## 7.2 Gate 的目标

Gate 的职责是：

> **阻止不可接受错误，而不是把故事训练成最会通过机器考试的文本。**

---

# 8. Story Model A/B：实验边界

模型 A/B 属于 **V1 内容生产策略实验**，不是未来平台能力。

但它必须与正式生产路径隔离，避免再次演化成 Gate 调试 campaign。

## 8.1 实验原则

- 使用独立实验入口 / branch / run mode；
- 不直接发布、不进入 downstream；
- 固定相同 Course Input；
- 固定 Story Profile / Beats；
- 固定 Prompt / Policy；
- 固定 runtime 参数；
- 不为不同模型单独改 Prompt；
- 不自动重试；
- 第一轮只比较模型，不同时修改 Gate；
- 每个候选无论 Gate 结果如何，**完整候选都必须留存供盲评**；
- Gate 只是评价维度之一，不能在盲评前淘汰候选。

## 8.2 第一轮建议

候选模型：

1. 当前 baseline：`gemini-2.5-flash`
2. 当前可用的更强 Gemini 档位
3. 一个跨厂商高质量 Story 参照模型

具体模型以网关真实可用性和价格确认后冻结，不在本方案里硬编码供应商版本。

## 8.3 最小样本

建议：

- 每个模型至少 **2 个独立候选**；
- 使用同一完整 L22 输入；
- 候选匿名化后进入盲评；
- 若前两名接近，再补第 3 个样本，而不是一开始大规模跑。

## 8.4 评价顺序

先评：

1. 故事吸引力；
2. 角色鲜活度；
3. 银河世界感；
4. 教学融合自然度；
5. 继续阅读意愿；
6. 事实正确性 / 明显越界。

再打开：

7. Gate / Validation 结果；
8. token；
9. cost；
10. latency。

模型 A/B 的目标是：

> **选出在可接受成本下，最容易稳定得到 Accepted Story 的模型。**

而不是：

> **选出最会过当前 Gate 的模型。**

---

# 9. 历史 PR / Campaign 处理原则

旧 #136–#162 栈不再视为“等待全部合并的产品”。

统一定义为：

> **Historical Campaign / Parts Warehouse**

处理规则：

1. 不整包 merge；
2. 不因为某个模块正确，就顺带接受所有历史架构决定；
3. 每个 V1 必需能力先审计真实价值；
4. 能最小提取 commit 则最小提取；
5. commit 污染严重则做 final-state file/function lift；
6. 与当前 V1 无关的能力冻结；
7. 不为了“已经投入很多”而继续完成历史 campaign。

## 9.1 当前优先保留/提取的能力类别

- Course / Question Bank 转换；
- Approved Input；
- 最小 read-side Context；
- Question tracking / 锁题；
- 关键 provenance / Canon 防错；
- rejected candidate / quarantine；
- 最小 run evidence；
- 可重复 E2E / preflight；
- 可确定结构错误的前置校验。

## 9.2 当前后移

- 完整 StateWriter 写回；
- 完整连续世界状态平台；
- 多世界观泛化；
- 完整 Runtime / Navigation 重构；
- xRunS 平台化抽象；
- 复杂 Release Governance；
- 为模糊叙事质量建立大量 hard Gate；
- 完整 Replay Corpus 平台。

---

# 10. 三个 V1 里程碑与量化验收

所有数字均为 **V1 初始工程阈值**，不是永久产品政策。  
如需调整，必须在对应里程碑开始前由 Owner + Engineering Owner 显式修改，不允许边跑边放宽。

---

## M1 — 一篇真正的 Accepted Story

### 目标

> L22 产出一份苹果与李光都认可的完整 Accepted Story Package。

### 要求

- Story Authoring 独立验收；
- 不以图片 / TTS / H5 完成为条件；
- 可以调试 Prompt / Policy / Model；
- 所有重大调整必须留记录；
- 必须建立 V1 Baseline Manifest；
- 必须保留最终 Accepted Story Package。

### 完成标志

> **L22 = Accepted Story。**

M1 达成只证明“能做出来”，**不代表 V1 完成。**

---

## M2 — 三课冻结规则下的可重复性验证

建议课程：

> **L22 / L23 / L24**

### 冻结条件

用于正式计算 M2 的三课必须使用：

- 同一 source code baseline；
- 同一 Prompt / Policy version；
- 同一 Validation 配置；
- 同一 Story Model；
- 同一 runtime parameter strategy。

允许变化：

- 课程本身的输入；
- 该课对应的 Story Profile / Beats 内容；
- 合法的 read-side Context。

### 禁止

- 为某一课单独加 Prompt 规则；
- 为某一课改 Gate 阈值；
- 为某一课新增 exception；
- 在三课计数中途改 code / Prompt / Policy 后继续沿用旧计数。

如果以上任一被冻结项发生修改：

> **修改前结果保留为调试样本；M2 三课连续验证重新计数。**

### 重生成次数

初始阈值：

> **每课最多 3 个 Story candidate（含第一次生成）。**

若第 4 次才得到 Accepted：

- 可作为调试成果保留；
- 该课不计入本轮 M2 成功；
- 需先分析稳定性原因，再启动新的 M2 计数。

### M2 完成标志

> **同一冻结生产规则下，L22 / L23 / L24 三课均在每课 ≤3 个候选内达到 Accepted Story。**

---

## M3 — 小批量“合格故事生产线”成立

### 批次定义

初始采用：

> **连续 5 课的小批次。**

若课程结构天然以单元组织，也可由 Owner 在开始前批准：

> 一个完整单元，但不得少于 5 课。

### 冻结规则

与 M2 相同：

- 不逐课改代码；
- 不逐课改 Prompt / Policy；
- 不逐课改 Gate；
- 不逐课创建特殊例外。

### 初始验收数字

| 指标 | V1 初始目标 |
|---|---|
| 最终 Accepted Story | **5/5** |
| 每课 ≤3 candidates 内 Accepted | **至少 4/5** |
| 需要工程人员修改代码/规则才能继续的课次 | **≤1/5（≤20%）** |
| 内容 Reviewer 人工 Review + 轻量修改 | **中位数 ≤45 分钟/课** |
| Owner 亲自处理 | **≤1/5，或仅处理抽样/例外** |
| Story Model / Prompt / Validation 中途变更 | **0 次** |
| 单课 token / actual cost / latency | **必须完整记录** |
| Cost 上限 | **V1 暂作为观察指标，不设硬上限；完成真实价格表后再决定** |

### M3 完成标志

> **故事生产主要成为内容生产活动，而不是工程调试活动。**

只有达到 M3，才正式宣告：

> **银河“合格故事生产线”V1 成立。**

---

# 11. Owner Acceptance 如何避免成为量产瓶颈

## Golden 阶段：M1 / M2

Owner 逐篇审。

目的：

- 校准“什么叫好故事”；
- 形成 Reviewer 可理解的量规；
- 建立正反例。

## M3

开始转交给指定内容 Reviewer：

```text
Story Candidate
      ↓
Hard Validation
      ↓
Content Reviewer
      ↓
Accepted / Revision
      ↓
Owner 抽样 / 例外处理
```

Owner 只直接处理：

- 低置信度；
- Reviewer 无法裁决；
- Canon / 课程原则冲突；
- 新型失败；
- 规则变更；
- 抽样质检。

M3 同时应沉淀一版轻量：

> **《Accepted Story Review Rubric》**

但 Rubric 的目标是帮助人稳定判断，不是重新制造几十条机器 hard Gate。

---

# 12. 苹果与李光的协作重新分工

## 苹果 / Owner

负责：

- 一级业务目标；
- 什么叫好故事；
- Canon / 课程不可妥协边界；
- Accepted Story 最终产品口径；
- Gate 严重度的产品裁决；
- 是否值得继续增加工程投入。

Owner 不再承担：

- PR 串手工转述；
- Agent 之间人工传话；
- 具体代码架构；
- 模型网关实现细节。

## 李光 / Engineering Owner

负责：

- 把 Owner 目标翻译为最小工程路径；
- 建立并维护 V1 Baseline Manifest；
- 控制技术债与抽象边界；
- 判断历史能力如何最小提取；
- 给出成本 / 风险 / 替代方案；
- 阻止 Agent 因局部 Issue 自动扩成平台工程。

## Claude Code / Coding Agent

负责：

- 在批准范围内实现；
- 测试；
- 提供证据；
- 不自行改变一级目标；
- 不自行扩大 V1；
- 不自行执行未授权付费实验。

## 苹果 CodeX / Independent Reviewer

负责：

- GitHub 独立审查；
- diff / correctness / regression；
- 检查 Issue 与实现是否一致；
- 检查 V1 Baseline 是否被偷偷改变；
- 提醒 scope expansion。

不负责：

- 替 Owner 决定产品目标；
- 自动把所有建议变成 Issue；
- 用“无 unresolved thread”代替“真正发生过 Review”。

---

# 13. GitHub 治理规则

## 13.1 所有动作必须显式标记状态

统一使用：

- **已执行**
- **待执行，可由 ChatGPT 完成**
- **待苹果 CodeX 执行**
- **待李光 / Claude Code 执行**
- **仅建议，不应执行**

避免“大家都以为别人已经写入 GitHub”。

## 13.2 Review 必须记录

至少记录：

- Codex Review 是否实际发生：YES / NO
- Reviewed HEAD SHA
- Review 结论
- Unresolved Threads 数量

“没有 unresolved thread”不能等价成“已经独立 Review”。

## 13.3 Paid Story Run

每次必须记录：

- source commit；
- Story Model；
- input version；
- Prompt / Policy version；
- Validation version；
- 授权人；
- 结果；
- failure stage / reason；
- token / cost（如可得）；
- candidate artifact。

---

# 14. #164 的新定位

#164 不再拥有 V1 产品范围定义权。

正式层级应调整为：

```text
Owner V1 目标与收口方案（本文件）
        ↓
V1 Baseline Manifest
        ↓
#164 / 后续执行 Issue
        ↓
具体 PR
```

#164 适合继续承担：

- 历史 campaign 最小提取；
- Rule Ownership；
- Validation 分层；
- 错误前移；
- 必要 Preflight；
- Evidence 最小闭环。

但以下 #164 原目标不再作为 V1 完成前置：

- 完整连续世界 StateWriter；
- L22→L23→L24 完整 Delta 写回平台；
- Navigation / Player 收口；
- Asset / TTS / Assemble 验收；
- 完整 Runtime feedback；
- 完整 Replay Corpus。

这些进入 Post-V1 / Downstream / V2。

---

# 15. 接下来建议执行顺序

## Step 0 — 已完成：冻结旧战场

已完成：

- #164 已记录 Owner V1 范围裁决；
- #137 已记录 G4 历史 paid campaign 暂停；
- #136、#138、#144–#162 未误合并/关闭；
- 未新增付费实验。

## Step 1 — Owner / 李光确认本 v0.2

确认重点：

- Accepted Story Package；
- M1/M2/M3；
- M2/M3 初始验收数字；
- #164 新定位。

## Step 2 — 建立 V1 Baseline Manifest

由李光/Claude Code整理事实，苹果 CodeX 独立复核。

在 Manifest 冻结前：

> **不恢复新一轮 Story paid run。**

## Step 3 — Gate 分级审议

逐条把当前八闸归入：

- BLOCK；
- REVIEW；
- WARN；
- INFO。

本阶段先做裁决表，不边审边改代码。

## Step 4 — Story Model A/B

按独立实验规则执行。

第一轮只筛模型：

> **不同时调 Gate，不同时调 Prompt。**

## Step 5 — 完成必要最小提取

只实现已经由：

> V1 Baseline Manifest + Gate 分级 + A/B 结果

证明需要的代码变化。

## Step 6 — M1：L22 Accepted Story

只验故事，不接下游。

## Step 7 — M2：冻结规则跑 L22/L23/L24

任何生产规则修改都会重新计数。

## Step 8 — M3：连续 5 课小批量

验证：

- 稳定性；
- Reviewer 工时；
- 工程介入率；
- 成本；
- 可转交性。

## Step 9 — 决定 Post-V1

再决定：

- 接回 Image/TTS/H5；
- 完整 State/Delta；
- xRunS Story capability；
- 扩展到全部 20 课。

---

# 16. V1 停止扩张规则

任何新需求先回答：

1. 不做它，会不会直接妨碍 Accepted Story？
2. 它解决的是已发生事故，还是未来想象？
3. 它属于 Story Authoring，还是 Downstream？
4. 它属于 V1，还是长期平台？
5. 能否用人工 Review 或轻量机制先解决？
6. 它会不会要求修改已经冻结的 M2/M3 Baseline？

如果不阻塞 Accepted Story：

> **默认后移。**

如果会破坏冻结 Baseline：

> **停止当前里程碑计数，先做显式 Owner / Engineering Decision。**

---

# 17. V1 成功条件

V1 不以“架构图完整”作为成功。

只有同时满足以下条件，才算完成：

1. 单次生产边界明确输出 Accepted Story Package；
2. Course / Question / Evidence 可追踪；
3. 明确硬错误不会静默进入 Accepted；
4. 模糊叙事质量不再主要由 brittle hard Gate 决定；
5. Story Model 已经过受控比较而非历史惯性选择；
6. M2 三课在同一冻结生产规则下完成；
7. M3 连续 5 课达到约定稳定性；
8. Reviewer 可以承担主要验收；
9. Owner 不成为日常量产瓶颈；
10. 工程介入率、人工工时和真实成本可以测量；
11. 历史 campaign 不再自动驱动新工程；
12. 系统能稳定输出可交付给 downstream 的 Accepted Story Package。

达到这些条件，即认为：

> **银河“合格故事生产线”V1 成立。**

---

# 18. V1 之后再讨论什么

V1 被证明成立后，再进入：

- 完整连续世界 State / Delta；
- Story Memory；
- Navigation / Runtime Contract；
- Image/TTS/H5 自动接回；
- 完整 Replay Corpus；
- 多模型调度；
- 多世界观；
- 多用户 / SaaS；
- xRunS Story Production capability。

这些能力可能长期有价值，但不得倒过来阻塞当前好故事生产。

---

## 最终原则

> **先证明能生产一篇好故事；再证明同一规则能重复生产好故事；最后证明它已经成为一条生产线。**

然后才做：

> **下游自动化、连续世界深化与平台化。**
