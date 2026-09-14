# GALAXY Repository Audit v0.2

> 状态：`FINAL DRAFT`  
> 审计对象：`xrundaLab/vibe-coding-ppt`，本地 `main`（审计时 HEAD `0dddb65`）  
> 更新依据：`GALAXY_M0_DECISIONS.md`。本文件替代 v0.1 审计结论，不启动代码改造。

## 1. Executive Conclusion

当前 Factory 已有可复用的课程解析、AI 编排、逐章页面生成、页面质检、图片/TTS、静态 Deck 组装与发布能力。它可承接新版 Story Profile 与 Golden regression，**无需重写页面生产线**。

当前最大工程缺口不是“内容生成能力”，而是可追溯的叙事状态合同：世界/角色规则仍集中于硬编码和长 Prompt，浏览器分支不跨集持久化，生产工作区未进入 Git，且未见 Release Manifest、ContextAssembler、Approved Delta 写入和真实用户状态 adapter。

P0 的最小目标已收敛为：以 `GALAXY-001` 验证版本、Context、Detected/Approved Delta 和 file-backed 状态往返；真实 APP/统一身份接入明确留在 P1。

## 2. Evidence Boundary

已按优先级审阅 Handoff Pack 的 `00_ENGINEERING`、`01_SOURCE_OF_TRUTH`、`02_GOLDEN_SAMPLE`，并审阅本地 Factory、静态 Runtime、测试和部署配置。

仍不可从本 checkout 验证：

- `factory/courses/` 被 `.gitignore` 排除，真实在产课程工作区未随仓库提交；
- APP/统一身份是否已有外部学习状态 API；
- 线上模型、Prompt、素材的真实发布版本；
- 已上线账户是否在其他系统持有连续性状态。

因此本报告只判断“当前仓库是否具备可审计实现”，不否认仓库外可能存在的能力。

## 3. Current Architecture

```text
outline.md
  -> 结构化 outline / 课程审计
  -> llm_understand + world_bible.resolve_context/prompt_block
  -> understanding.json
  -> pages.json 的逐章生成、审计与人工反馈
  -> 图像 Prompt 扩写、TTS、assemble
  -> public/ 静态 HTML Deck -> Cloudflare Pages

浏览器 Deck
  -> 单会话选择 / 重试 / 跳页
  -> CreatorReviewAppSDK.complete()
```

| 领域 | 当前实现 | v0.2 判断 |
|---|---|---|
| 课程数据 | `outline.md`、structured outline、slug | 可复用，但须补 `course_id=COURSE-001` 与版本。 |
| 世界/角色 | `factory/world_bible.py` | 必须 Extract/Wrap；不可继续作为新版事实源和默认策略。 |
| Prompt | `llm_understand()` 等生产调用 | 调用框架保留；Context 注入方式替换为按需装配。 |
| 页面/交互 | `pages.json`、`deck_template.html` | 保留为 renderer；P0 不重写。 |
| 视觉/TTS | `generate_images.py`、TTS、官方 IP 图 | 可复用；P1 再补 reference provenance。 |
| 用户剧情状态 | 浏览器内存 + complete 回调 | P0 用 file-backed adapter 仅验证合同；真实写入 P1。 |
| 发布 | `assemble()`、`public/`、`wrangler.toml` | 需补 Release Manifest。 |

## 4. Existing Production and State Flow

1. Factory 从忽略的本地课程工作区读取 outline，产出中间 JSON 和页面资产。
2. `world_bible.resolve_context()` 依据课号/关键词推断阶段和叙事包装；`prompt_block()` 注入默认航行任务与拓奇设定。
3. `normalize_understanding()` 继续把故事种子、奖励和航行日志归一化为旧模板。
4. 页面、图片、TTS 和静态 HTML 均能量产，但选项结果没有可审计的跨集 StateWriter。

生产 `meta.json` 是任务进度/反馈日志，不得被误用为学习者关系、发现或路线档案。

## 5. v0.2 P0 Target Architecture

```text
Release Manifest + Registry Snapshots + Story Profile v0.3
  -> ContextAssembler (read only)
  -> existing narrative/pages production flow
  -> DeltaExtractor (Detected Delta)
  -> DeltaValidator (Expected/Allowed Delta Scope)
  -> StateWriter (Approved Delta only)
  -> file-backed State Adapter for Golden / Factory dry run
```

EP001 输入与批准结果严格采用 M0 决议：

- 输入：`TOKI=NOT_MET`、`L01=UNKNOWN`；
- 本集允许/期待：TOKI 到 `EARLY_ACQUAINTANCE`、L01 到 `VISITED`、`USER_FIRST_ARRIVAL`、`ROUTE_LOG`；
- Shared Canon 写入：`persistence_policy.shared_canon_write.allowed=false`，Expected Delta 为 `none`。

ContextAssembler 只读取起始 Relationship State 和 Character Eligibility，**不制造关系变化**。

## 6. Reusable Components

| Component | Reuse decision | Boundary |
|---|---|---|
| Outline 解析、章节锁定、练习降难 | KEEP | 继续服务课程与页面生产，不承担 Canon 判定。 |
| 逐章审计、术语/叙述/角色检查 | KEEP + WRAP | 增加 Story Profile、Eligibility、Forbidden Trope 验证。 |
| 图像/TTS/静态 Deck | KEEP | 以角色/地点 ID 接入，P0 不重做渲染器。 |
| `world_bible.py` | EXTRACT + WRAP | 遗留逻辑迁入 `legacy-v2` profile；不可作为 baseline 默认。 |
| 选择与重试 | KEEP | EP001 `first_action` 仅作为轻量 route choice state。 |
| `meta.json` | KEEP AS PRODUCTION META | 不得承担用户状态；发布时导出 manifest。 |

## 7. Core Conflicts and Resolutions

| Conflict | Current behavior | v0.2 resolution |
|---|---|---|
| 默认飞船受损/修理/星粒 | 多处 Prompt 和归一化强制 | 显式 legacy profile；baseline 默认禁用。 |
| TOKI 被写成反复装笨 | Persona/笑料规则硬编码 | 新 profile 不继承；TOKI 的正式内容以 Source of Truth 为准。 |
| 课号决定星域 | `resolve_context()` 推断 | 改为 Course/Story Profile 显式 `location_id/route_id`。 |
| 单伙伴被当作世界事实 | 角色/图像/TTS 只容纳 TOKI | P0 仅 TOKI 完整；所有主角色先有 stable IDs 和 stub。 |
| 每课失忆 | 静态页面、无 StateWriter | P0 file-backed adapter 验证；真实 APP 接入 P1。 |
| 模型自由写状态 | 当前无 Delta gate | 先 Detected，再按 Expected/Allowed scope 批准。 |
| 共享 Canon 污染 | 当前无状态层 | Schema 采用正式字段 `persistence_policy.shared_canon_write.allowed=false`。 |

## 8. Migration Discipline

在任何迁移或代码改动前，`factory/courses/` 必须完成备份和分类：`legacy-v2`、`baseline-v0.3-candidate`、`review-required`。历史缺失版本只能记录为 unknown/untracked，不得按标题、课号或猜测补造。

迁移路径固定为：

```text
Extract -> Wrap -> Dual-run -> Replace
```

新旧 profile 可以并存；历史发布物不回写；P0 只以 EP001 打通新链路。

## 9. Risks and Non-Goals

- 真实连续性依赖 APP/统一身份的状态归属，当前责任边界未确认，不能由 Factory 单方面承诺。
- 必须防止 `normalize_understanding()` 等隐性覆写在 baseline 流程中重新注入旧套路。
- 可重复测试命令与 Golden regression 是 M3 门槛；GitHub Actions 是低成本增强而非阻塞条件。
- P0 不建设通用 Branch Memory、开放 RPG、道具经济、兑换系统、自动 Canon Proposal 或全量历史迁移。

## 10. Audit Stop Condition

下一步仅等待李光确认 APP/统一身份状态责任边界、在产 `factory/courses/` 的备份/分类责任与 EP001 fixture 来源。确认前不创建正式 Issue、不改代码。

