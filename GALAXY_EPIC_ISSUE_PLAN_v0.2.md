# GALAXY Epic / Issue Plan v0.2

> 状态：`FINAL DRAFT`  
> 本文仅是待确认的 Issue 草案；暂不创建 GitHub Issue。  
> 前置等待：李光确认 APP / RunS 统一身份状态的责任仓库、负责人和 adapter 边界。

## 1. 治理与仓库归属

| 工作项 | 建议归属 | 说明 |
|---|---|---|
| 总控 Epic：`[Epic] Galaxy Story System Baseline v0.3 Engineering Migration` | `xrundaLab/.github-private` | 跨 Canon、Factory、APP 宿主、测试与发布治理。 |
| P0 Factory 实施项 | `xrundaLab/vibe-coding-ppt` | 实际代码、fixture、生产适配、测试。 |
| 真实用户状态接入 | APP/统一身份真实代码仓库（待确认） | Factory 只定义 adapter contract，不能单方面实现账户持久化。 |

任何未来执行 Issue 都应关联治理 Epic；不得用自动关闭关键词跳过测试和业务验收。

## 2. Milestones

| Milestone | Exit Criteria | Not included |
|---|---|---|
| M0 | `GALAXY_M0_DECISIONS.md` 经人工确认，明确 APP 状态责任边界。 | 代码改造、Canon 修改。 |
| M1 | Release identity、legacy profile 抽取、在产课程备份/分类完成。 | 用户连续性。 |
| M2 | Story Profile、Registries、read-only ContextAssembler 可经 EP001 fixture 验证。 | 全量 RAG/多角色出场。 |
| M3 | Detected -> Approved Delta、file-backed adapter、单一测试命令、EP001 Golden regression 通过。 | 真实 APP 状态接入；GHA 不阻塞。 |
| M4 | APP/统一身份 adapter、EP002/003 回归、发布门禁。 | 开放 RPG/数值经济。 |

## 3. P0-01：Release Manifest、Stable IDs 与工作区迁移清点

- **仓库 / 类型**：`xrundaLab/vibe-coding-ppt` / Feature
- **依赖**：M0 决议。

**Background**  
`factory/courses/` 被忽略，发布无法复现输入版本；当前没有 release 级身份合同。

**Source Specs**  
Release Manifest Template、Release Migration Protocol、`GALAXY_M0_DECISIONS.md`。

**Current Behavior**  
使用 slug/标题/课号；`meta.json` 只管理本地生产步骤。

**Required Behavior**  
新增 immutable Release Manifest，记录 `release_id/course_id/episode_id/course_version/canon_version/schema_version/prompt_version/model_version/world_profile_id`、输入输出 hash 与生成时间。迁移前备份并分类所有 `factory/courses/` 为 `legacy-v2`、`baseline-v0.3-candidate` 或 `review-required`。

**Data / Schema Impact**  
新增 `release_manifest.json` 和课程分类清单；历史缺失版本标记 unknown/untracked，不伪造字段。

**Migration Impact**  
历史课不自动升级；新 release 才强制完整 Manifest。

**Acceptance Criteria**

1. Given 新 release，When 缺任一版本字段，Then manifest validator 非零失败。
2. Given `release_id` 已存在但 hash 不同，When 再发布，Then 拒绝并要求新 ID。
3. Given 任一在产工作区，When 迁移开始前检查，Then 必有备份路径与三类之一的分类。
4. Given 历史课程版本未知，Then manifest/category 不得补造 Canon/Prompt/model version。

**Tests**  
manifest schema、重复 ID/hashes、工作区分类 fixture。

**Out of Scope**  
自动回滚服务、生产站部署、历史内容重生成。

## 4. P0-02：Extract / Wrap Legacy World Profile

- **仓库 / 类型**：`xrundaLab/vibe-coding-ppt` / Refactor
- **依赖**：P0-01。

**Background**  
飞船受损、修理、星粒、拓奇装笨、课号轮换星域和单伙伴限制被分散硬编码。

**Source Specs**  
Narrative Production Spec、World/Character/Map Canon、Legacy Inventory v0.2。

**Current Behavior**  
`world_bible.py`、`UNDERSTAND_SYS`、`normalize_understanding()`、图像 Prompt 后缀共同将所有课程拉回旧循环。

**Required Behavior**  
显式保留 `legacy-v2` profile；新增 `baseline-v0.3` profile。baseline 默认不得注入旧任务/人格/课号推断。profile ID 必须写入 manifest。

**Data / Schema Impact**  
提取 versioned profile config；不修改 Canon 正文。

**Migration Impact**  
既有课绑定 legacy；未分类课冻结为 `review-required`。

**Acceptance Criteria**

1. Given `legacy-v2` fixture，Then 既有生成/渲染行为兼容。
2. Given `baseline-v0.3` EP001，Then Context/Prompt 不含默认修船、固定星粒或“拓奇呆萌憨傻”。
3. Given baseline 任意课程标题，Then 课号不能自动决定 location/region。
4. Given 官方伙伴在 profile 中 eligible，Then 不因“非拓奇”被 visual validator 拒绝；仍遵守同屏上限。

**Tests**  
legacy snapshot、baseline negative assertions、image prompt fixtures。

**Out of Scope**  
角色文学重写、全量素材替换、历史课程改版。

## 5. P0-03：Story Profile v0.3、最小 Registries 与 Validator

- **仓库 / 类型**：`xrundaLab/vibe-coding-ppt` / Feature
- **依赖**：P0-01、P0-02；M0 Golden IDs。

**Background**  
`understanding.json` 是生成中间件，不能表达 v0.3 的权限、持续性和 Delta 合同。

**Source Specs**  
Story Schema v0.3、Engineering Handoff、M0 Decisions。

**Current Behavior**  
无 schema version、Registry 或废弃字段阻断；其他主角色未进入工程。

**Required Behavior**  
新增 `story_profile.json` adapter 和 validator；最小实现 Course/Canon/Character/Location/ForbiddenTrope registries。所有主角色有 stable ID；P0 仅 TOKI 完整运行资料，其他角色允许 stub。严格采用 `persistence_policy.shared_canon_write.allowed=false`，拒绝简写和废弃字段。

**Data / Schema Impact**  
新增 registry snapshots、profile schema。`TOKI` 映射 Eligibility、epistemic permissions、视觉/TTS 引用；其余角色记录 ID 与未引入状态。

**Migration Impact**  
legacy 可转兼容 profile，但不得标记为 v0.3 release。

**Acceptance Criteria**

1. Given EP001 profile，Then 缺 `episode_id/location_id/primary_character_id/persistence_policy/character_eligibility` 任一字段时验证失败。
2. Given `persistence_policy.shared_canon_write.allowed=false`，Then 任意 shared Canon write request 都被 validator 拒绝。
3. Given profile 包含 `canon_scope`、`Route Canon`、`canon_writeback` 或自定义简写 shared 字段，Then validator 输出字段路径并失败。
4. Given registry，Then TOKI 有完整 EP001 运行资料；其他主角色均有 stable ID，且不要求出场/完整素材。

**Tests**  
schema positive/negative fixtures、TOKI registry fixture、stub registry fixture。

**Out of Scope**  
自动 Markdown 解析、Reference RAG、全角色完整动画/TTS。

## 6. P0-04：Read-only ContextAssembler Lite

- **仓库 / 类型**：`xrundaLab/vibe-coding-ppt` / Feature
- **依赖**：P0-03。

**Background**  
当前 `resolve_context()` 由课号制造世界上下文，无法表达用户已知/未见事实。

**Source Specs**  
Engineering Handoff、Story Schema v0.3、M0 Decisions。

**Current Behavior**  
默认 TOKI 单伙伴，角色知识越权，整段 world prompt 注入。

**Required Behavior**  
实现 read-only `ContextAssemblerLite`，按 ID 组装 Course、相关 Canon、Location、Character Eligibility、当前 Relationship/Discovery/Route State、受控 references 和 runtime budget。禁止全量 Canon Prompt 注入。

**Data / Schema Impact**  
输出 Context Snapshot 与 source IDs/hash；不输出/写入 Delta。

**Migration Impact**  
legacy profile 继续走旧 resolver；baseline 只走 assembler。

**Acceptance Criteria**

1. Given EP001 initial state `TOKI=NOT_MET`、`L01=UNKNOWN`，When assembler runs，Then 输出读取这两个值且 TOKI 为本集 eligible 候选。
2. Given 同一输入，Then assembler 不得输出 `TOKI=EARLY_ACQUAINTANCE`、`L01=VISITED` 或任何状态变更。
3. Given KONTI 未引入，Then context 不得把 KONTI 标为熟人、对话者或已知事实来源。
4. Given 无关前史，Then 不在 context snapshot，除非 profile/reference 显式选择。

**Tests**  
EP001 read-only fixture、未见 KONTI negative fixture、context source/budget assertions。

**Out of Scope**  
关系变化写入、自动检索、全量世界账本。

## 7. P0-05：Detected / Approved Delta 与 file-backed State Adapter

- **仓库 / 类型**：`xrundaLab/vibe-coding-ppt` / Feature
- **依赖**：P0-03、P0-04。

**Background**  
当前模型/页面没有受控跨集状态；将模型自由推断直接写入用户状态存在高风险。

**Source Specs**  
Story Schema v0.3、Handoff Dry Run、M0 Decisions。

**Current Behavior**  
选择仅在浏览器会话中存在；`meta.json` 是生产数据。

**Required Behavior**  
实现 `DeltaExtractor -> DeltaValidator -> StateWriter`：Extractor 仅产生 Detected Delta；Validator 对照 Expected/Allowed Delta Scope；Writer 只持久化 Approved Delta。实现 file-backed State Adapter，并定义未来 APP adapter interface。EP001 `first_action` 只作为 route continuity 的轻量 choice state；不建设通用 Branch Memory。

**Data / Schema Impact**  
新增 `detected_delta`、`approved_delta`、state schema 和 adapter interface。EP001 expected scope：TOKI 关系、L01 发现、`USER_FIRST_ARRIVAL`、`ROUTE_LOG`；shared Canon none。

**Migration Impact**  
不迁移真实用户数据；APP/统一身份 writer 保持 P1。

**Acceptance Criteria**

1. Given EP001 outcome，When Extractor runs，Then 输出 Detected Delta，且不直接写 state。
2. Given Detected Delta 符合 Expected/Allowed Scope，When Validator runs，Then Approved Delta 仅含 TOKI `NOT_MET -> EARLY_ACQUAINTANCE`、L01 `UNKNOWN -> VISITED`、`USER_FIRST_ARRIVAL`、`ROUTE_LOG`。
3. Given Detected Delta 试图写 shared Canon、未允许角色关系或额外发现，Then Validator 拒绝且 StateWriter 不写入。
4. Given Approved Delta 写入 file-backed adapter，When 后续 ContextAssembler 读取，Then TOKI 不得回到 `NOT_MET`，L01 不得回到 `UNKNOWN`。
5. Given `first_action`，Then 它只出现在 route continuity choice state，不产生通用 branch memory schema。

**Tests**  
detected-vs-approved negative fixtures、EP001 state round-trip、shared Canon rejection、light choice fixture。

**Out of Scope**  
真实账户 API、跨设备同步、开放分支、道具/积分/兑换系统。

## 8. P0-06：单一可重复测试命令与 EP001 Golden Regression

- **仓库 / 类型**：`xrundaLab/vibe-coding-ppt` / Test
- **依赖**：P0-01 至 P0-05。

**Background**  
现有 unittest 因依赖和工作目录假设不能稳定作为团队门禁；EP001 仍是文档样本。

**Source Specs**  
EP001 Golden Sample、Handoff Dry Run、M0 Decisions。

**Current Behavior**  
有测试基础但未见统一命令/CI；本次审计环境缺部分依赖，且部分测试假设 `factory/` 为 cwd。

**Required Behavior**  
提供一条文档化、干净环境可重复执行的命令，执行 Factory 基础测试、legacy replay 最低验证和 EP001 Golden regression。GitHub Actions 可低成本添加，但不作为 M3 阻塞。

**Data / Schema Impact**  
新增 fixtures/test runner documentation；不改 Canon。

**Migration Impact**  
先固化依赖和 cwd，再逐步整理旧测试。

**Acceptance Criteria**

1. Given 干净环境按文档安装依赖，When 执行单一命令，Then Factory tests、legacy replay 与 EP001 Golden 可重复运行。
2. Given EP001 fixture，Then 验证 M0 的输入状态、read-only context、approved delta、shared Canon none 和 deprecated fields absence。
3. Given 任一 manifest/profile/context/delta fixture 非法，Then 命令非零失败并指明错误。
4. GitHub Actions 若加入，Then 复用同一命令；若未加入，M3 仍可依上述三项测试通过。

**Tests**  
本 Issue 的交付即为测试入口与回归 fixtures。

**Out of Scope**  
全 117 课回归、生产站自动部署、移动端兼容矩阵。

## 9. P1 / P2 后续项

| Priority | Suggested issue | Dependency | Rationale |
|---|---|---|---|
| P1 | APP / RunS unified identity State Adapter | 李光确认真实仓库与 owner | Factory P0 只定义 contract，不承担账户状态。 |
| P1 | EP002 / EP003 continuity regression | P0-05 + adapter/fixture | 验证“认识拓奇后不失忆、未见康缇不自动熟识”。 |
| P1 | Reference / visual asset provenance | P0-01 / P0-03 | 记录受控素材、用途、版本与 Prompt 引用。 |
| P1 | Release preflight / optional GitHub Actions | P0-06 | 低成本自动化，不阻塞 M3。 |
| P1 | Multi-character renderer slots | P0-03 | 在 runtime budget 内支持其他伙伴，不扩大首轮角色戏份。 |
| P2 | Generic Branch Memory / Canon proposal queue / world ledger | P1 stability | 避免在状态与审核未验证前过度工程。 |
| P2 | 道具、积分、兑换、开放剧情 | 产品数值与埋点成熟后 | 不能由剧情生产线单独假设其已可用。 |

## 10. Suggested Project Status After Human Confirmation

| Item | Proposed status |
|---|---|
| Governance Epic | `Backlog`，待李光确认 APP 状态责任边界后创建。 |
| P0-01 / P0-02 | `Ready`，确认在产工作区备份责任后可启动。 |
| P0-03 / P0-04 | `Backlog`，待 M0 IDs 与 Source snapshot 落定。 |
| P0-05 | `Backlog`，待 adapter contract 的跨仓责任确认。 |
| P0-06 | `Backlog`，依赖 P0 contracts，但可先整理测试运行说明。 |
| 全部 P1 / P2 | `Backlog`。 |

