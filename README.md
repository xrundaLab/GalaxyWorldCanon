# GalaxyWorldCanon

> 本仓库是**世界观约束型仓库**，不是工程代码项目。它的唯一职责是为「银河」这个 IP 的所有下游生产（剧集写作、AI 课程内容、工程系统数据结构）提供**唯一、权威、可追溯**的世界观与数据契约来源。

**面向对象**：任何被派去做「银河」相关内容生产、代码开发、或规范审查的 agent，都应该先读完这份 README，再决定去读哪些原文件——不要在没搞清楚权威层级之前，把 `03_REFERENCE_HISTORY` 或根目录治理文档当成当前规范来用。

---

## 30 秒判断：我该读哪份文档？

| 我要做的事 | 去读 |
|---|---|
| 判断某个世界观设定是否成立（角色、地点、历史、能不能出现某种真实世界知识） | [01_SOURCE_OF_TRUTH](01_SOURCE_OF_TRUTH/) 全部 5 份 |
| 给工程系统写/改数据结构、字段 | [00_ENGINEERING/银河 Story Schema v0.3](00_ENGINEERING/银河%20Story%20Schema%20v0.3｜Engineering%20Data%20Contract%20Candidate.md) |
| 写一集正式剧本 / 判断某集是否合格 | [00_ENGINEERING/银河 Narrative Production Spec v0.2](00_ENGINEERING/银河%20Narrative%20Production%20Spec%20v0.2｜单集叙事生产呈现与验收规范.md) |
| 要修改 Canon、发新版本、走审批流程 | [00_ENGINEERING/Galaxy Update Release Migration Protocol v0.1](00_ENGINEERING/Galaxy%20Update%20Release%20Migration%20Protocol%20v0.1.md) + 两份模板 |
| 想知道哪些共享资源已经正式上架、由谁消费、应固定哪个版本 | `00_ENGINEERING/resources/GALAXY_RESOURCE_CATALOG.json` |
| 想理解整体架构、模块划分、团队分工 | [00_ENGINEERING/Galaxy Engineering Handoff Pack v0.1](00_ENGINEERING/Galaxy%20Engineering%20Handoff%20Pack%20v0.1.md) |
| 想看一个"合格产出"长什么样 | [02_GOLDEN_SAMPLE](02_GOLDEN_SAMPLE/)（**但注意**，见下方特别说明） |
| 想搞清楚某个旧术语/旧设定为什么消失了 | [03_REFERENCE_HISTORY](03_REFERENCE_HISTORY/)（仅供追溯，**不可**当规范用） |
| 想了解外部代码仓库 `vibe-coding-ppt` 的改造计划 | 根目录 6 份 `GALAXY_*` / `LEGACY_*` / `Liguang_*` 文档 |

**默认原则：只信任本表左列指向的文档；其他文档在被指向之前，都视为背景资料而非当前规则。**

---

## 仓库结构与权威分层

```
01_SOURCE_OF_TRUTH/   ← 世界观最高权威（回答"银河世界是什么"）
00_ENGINEERING/       ← 工程规范最高权威（回答"怎么生产、怎么变更、数据长什么样"）
02_GOLDEN_SAMPLE/     ← 生产参考样本（不是自动等于"已批准"，逐份核对状态）
03_REFERENCE_HISTORY/ ← 历史存档，全部已被 00/01 取代，只用于追溯"为什么现在是这样"
04_REPOSITORY_AUDIT/  ← 预留空目录（历史遗留，实际审计成果放在了根目录，见下）
根目录 GALAXY_*.md    ← 对外部代码仓库 vibe-coding-ppt 的工程审计/迁移提案，尚未执行
```

### 1. `01_SOURCE_OF_TRUTH/`——世界观最高权威

对下游任何 agent 具有强约束力，判断"世界观对不对"以此为准：

- **银河 World Canon - 世界圣经 v1.0** — 世界观总纲，含 Canon 分级体系（C0/C1/C2/MYSTERY/CREATOR-ONLY/UNDECIDED）、16 条最小不可变 Canon、15 条硬性禁止事项。⚠️ 已知有一份未应用的自审补丁（见 `03_REFERENCE_HISTORY` 中的自审文档），正文仍留有几处待修问题，改动前建议先核对该补丁清单。
- **银河前史与人物历史骨架 v0.1** — 历史/时间线权威，五大历史纪元、角色相识顺序。
- **银河空间地图与固定场所骨架 v0.1** — 空间权威，8 个核心地点、4 类航路、12 条冻结原则。
- **银河角色生态与 Character Pack v0.1** — 四位主角色（拓奇/康缇/普罗/爱今）设定与行为盲点。状态标注为"9月工作稿"，比同目录其他文档略"软"，可能优先被修订。
- **银河 Controlled Reference Library v0.1** — 现实知识/科幻母题/儿童安全红线的受控资料库。⚠️ 文档自述"原稿遗失，本版为重建版"，旧的 6 层内部标签体系未恢复，如原稿出现需先 diff 再决定是否替换。
- `银河AI学课程信息表_v5.0.7-候选.xlsx` — 课程内容唯一事实源（Course Source of Truth），供 AI 学课程生产引用。

### 2. `00_ENGINEERING/`——工程规范最高权威

对下游工程/生产 agent 具有强约束力：

- **Galaxy Engineering Handoff Pack v0.1** — 架构总纲：One Canon 原则、Reference Don't Duplicate、Canon 默认只读、Runtime≠Canon、P0/P1/P2 路线图与团队分工。**建议所有 agent 最先读这一份**，它是理解其余工程文档为什么这样设计的钥匙。
- **银河 Story Schema v0.3｜Engineering Data Contract Candidate** — 数据契约唯一版本，取代已废弃的 `03_REFERENCE_HISTORY` 中的 v0.2。字段级强约束（如 `persistence_policy` 取代已废弃的 `canon_scope`）。
- **银河 Narrative Production Spec v0.2** — 单集生产/写作/QA 规范，只管"怎么生产一集"，不涉及世界观本身。含 14 条禁用套路清单、角色行为红线、发布检查清单。取代 `03_REFERENCE_HISTORY` 中的 v0.1 剧情圣经。
- **Galaxy Update Release Migration Protocol v0.1** — 变更治理总协议，定义变更全生命周期、6 类变更分类、8 类迁移类型，明确规定 CodeX 等自动化系统的权限边界（例如不能批准 C0/C1 Canon 变更、不能接受 Retcon）。**任何要修改 Canon 或 Schema 的 agent，必须先看这份，走对应审批流程。**
- **Galaxy CHANGESET Template v0.1 / Galaxy Release Manifest Template v0.1** — 变更记录与发布清单的标准模板，正式变更必须据此生成实例。
- **Galaxy Handoff Dry Run v0.1** — 用真实案例验证治理协议的演练报告，附带"给 CodeX 的标准指令模板"，可作为写变更请求时的参考范例。
- **resources/GALAXY_RESOURCE_CATALOG.json** — 共享资源路由索引。它只记录资源 ID、权威/成熟度、来源与检索方式，不复制 Canon 正文。Map / Prehistory 等结构化投影通过此处声明是否已正式上架；下游 run 仍必须从外部固定 Catalog 的 exact commit/blob/SHA，禁止静默跟随 moving `main`。

### 3. `02_GOLDEN_SAMPLE/`——生产参考样本（需逐份核对状态，不要笼统当"已批准范本"）

- **银河三集 Pilot v0.1** — Schema 验证记录，历史使命已完成（发现已并入 Story Schema v0.3），仅供追溯 Schema 演化过程。
- **银河 EP001 v0.2《谁在和我说话？》** — 目前仓库里唯一完整的正式剧本候选。
- **《银河 EP001 正式首集评审 v1.0》** — 对上述剧本的质量评审。**⚠️ 结论是 7.8/10、REVISION REQUIRED，尚未 APPROVED**，列有 6 项 P0 必改项。引用 EP001 作为范本前，请先确认该评审中的必改项是否已落实，不要默认它是"金标准"。

### 4. `03_REFERENCE_HISTORY/`——历史存档，全部已被取代

以下 5 份文档中的术语和设定（如 `canon_scope`、`canon_writeback`、`Route Canon`）**均已废止**，新内容生产不得再引用：

- 银河 Story Schema v0.2（已被 00_ENGINEERING 的 v0.3 取代）
- 银河 World Canon v1.0 自审与 A-B-C-D 冲突检查（记录了 World Canon 尚未应用的补丁队列，追溯价值高）
- 银河APP 9月版叙事引擎中间规范 v0.1（Narrative Production Spec 的过渡稿）
- 银河剧情游戏世界观与单集生产圣经-v0.1（六月时代原始文档，角色名仍是占位）
- 银河故事世界系统｜六月→九月差异审查与 Gap Matrix v1.0（解释"九月重构"动机的关键转折文档，想理解"为什么现在这么设计"从这份读起）

### 5. `04_REPOSITORY_AUDIT:`——预留空目录

命名带有历史遗留的尾随冒号，与其他目录命名风格不一致；目录本身为空，实际审计成果被放在了根目录（见下），未来如需清理仓库结构可考虑移除或重新利用此目录。

### 6. 根目录 `GALAXY_*.md` / `LEGACY_*.md` / `Liguang_*.md`——外部仓库工程审计（尚未执行）

**注意：这 6 份文档审计的对象是另一个代码仓库 `xrundaLab/vibe-coding-ppt`（真实的课程生产代码），不是本仓库自身。** 本仓库是纯文档库，没有代码可审计。它们是一次仓库梳理工作的产出链，按以下顺序阅读：

1. **GALAXY_REPOSITORY_AUDIT_v0.2** — 对 vibe-coding-ppt 现状的整体审计结论
2. **GALAXY_GAP_MATRIX_ENGINEERING_v0.2** — 逐项能力差距清单（EXISTING/PARTIAL/MISSING/CONFLICT）
3. **LEGACY_BEHAVIOR_INVENTORY_v0.2** — 逐条代码行为清单（KEEP/CONFIGURE/DEPRECATE/REMOVE）
4. **GALAXY_M0_DECISIONS** — 基于以上做出的 P0 范围决策
5. **GALAXY_EPIC_ISSUE_PLAN_v0.2** — 拆解为具体 Epic/Issue 草案
6. **Liguang_USER_STATE_AUDIT.md** — 针对遗留疑问的代码级实证调研

全部标记 **FINAL DRAFT**，且明确声明"未创建 GitHub Issue、未改代码，等待李光确认"——**这是尚未落地的提案，不是已完成的实施记录**，不要误以为其中的 Epic/Issue 已在执行。

---

## 给 Agent 的操作原则

1. **只信任 `00_ENGINEERING` 和 `01_SOURCE_OF_TRUTH`** 作为当前规则；`03_REFERENCE_HISTORY` 只用来查历史，绝不作为生产依据。
2. **改动 Canon 或 Schema 前，先看 Migration Protocol**，判断变更类别和所需审批级别；不要绕过 CHANGESET 流程直接改正文。
3. **引用 `02_GOLDEN_SAMPLE` 前先看配套评审文档的结论**，不要假设"在 GOLDEN_SAMPLE 目录里 = 已批准"。
4. **根目录治理文档不代表本仓库的待办事项**，它们是对外部仓库的提案且尚未执行，不要主动据此去改外部仓库代码，除非用户明确要求推进该提案。
5. 遇到文档间冲突（例如 World Canon 正文与其自审补丁不一致），以**最新层级的权威文档为准**，并在输出中提示用户该处存在已知未修的差异，而不是自行选边。
