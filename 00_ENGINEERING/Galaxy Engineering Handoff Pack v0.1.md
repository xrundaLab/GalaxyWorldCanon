# Galaxy Engineering Handoff Pack v0.1
## 银河剧情世界系统工程交接包

**版本：v0.1**  
**日期：2026-09-11**  
**状态：ENGINEERING HANDOFF CANDIDATE**  
**目标读者：Product / Canon Owner、CodeX、李光、李光 CodeX、生产系统研发与QA人员**

---

# 0. 交接目的

本交接包不是一份“需求说明书合集”。

它的目标是把银河当前已经完成的：

- World Canon；
- Character Canon；
- Prehistory；
- Map Canon；
- Controlled Reference Library；
- Story Schema；
- Narrative Production Spec；
- Continuity / Ledger；
- Canonical Pilot；

转换成一套可以被研发系统持续消费的内容工程机制。

最终希望实现：

> **内容团队负责世界、角色、课程、知识与故事规则；工程系统负责稳定读取、组装、校验、生成、记录与迁移。**

系统必须支持：

```text
内容更新
↓
Release Manifest
↓
Context Assembly
↓
Story Production
↓
Validation
↓
Episode Delta
↓
State Writeback
↓
Regression
```

而不是继续依赖：

> 一个越来越长的超级 Prompt + 人工记忆。

---

# 1. 核心工程原则

## 1.1 One Canon

银河只有一套：

> **Shared Canon**

任何用户个性化经历：

> 不创建第二套银河正史。

---

## 1.2 Reference, Don't Duplicate

Story Profile 不复制：

- 世界规则；
- 角色设定；
- 地点历史；
- 课程内容。

统一通过 ID 引用 Source of Truth。

---

## 1.3 Read State Before Generation

生产一集以前必须读取：

- 当前课程；
- 当前世界；
- 当前角色；
- 当前地点；
- 当前用户关系；
- 当前知识权限；
- 当前未解决线程；
- 当前Runtime能力。

---

## 1.4 Write Delta After Generation

生产完成以后：

> 不重新保存一份“完整世界”。

只记录：

> **这一集改变了什么。**

---

## 1.5 Canon Is Read-only by Default

Generator Agent：

> 无 Canon 写权限。

最多提交：

> `shared_canon_proposal`

由 Canon Owner 审批。

---

## 1.6 Runtime ≠ Canon

当前产品做不到：

> 不代表银河世界不存在。

例如：

```text
max_primary_dialogue_characters = 2
```

属于 Runtime。

不能变成：

> “银河规定三个人不能同时对话。”

---

# 2. 当前正式内容资产

建议当前交接时登记以下文件。

| ID | 文件 | 角色 | 当前状态 |
|---|---|---|---|
| DOC-001 | 六月→九月 Gap Matrix | 设计迁移依据 | APPROVED BASELINE |
| DOC-002 | Character Pack v0.1 | Character Canon | APPROVED BASELINE |
| DOC-003 | Controlled Reference Library v0.1 | Knowledge Governance | WORKING BASELINE |
| DOC-004 | World Canon v1.0 | World Canon | CANDIDATE BASELINE |
| DOC-005 | World Canon 自审 | Patch / Audit | APPROVED AUDIT |
| DOC-006 | 前史与人物历史骨架 v0.1 | Prehistory | CANDIDATE BASELINE |
| DOC-007 | 空间地图与固定场所骨架 v0.1 | Map Canon | CANDIDATE BASELINE |
| DOC-008 | Story Schema v0.3 | Data Contract | ENGINEERING CANDIDATE |
| DOC-009 | Narrative Production Spec v0.2 | Production Rules | ENGINEERING CANDIDATE |
| DOC-010 | EP001 v0.2 | Canonical Example | CANDIDATE |
| DOC-011 | EP001正式首集评审 | QA Example | APPROVED REVIEW |

以上文档以后不应作为运行时全文直接塞进 Prompt。

工程系统应：

> 解析为可引用 Source + Registry + Context。

---

# 3. Source of Truth 责任表

| 内容类型 | 唯一事实源 |
|---|---|
| 课程目标 | Course Source of Truth |
| 世界规则 | World Canon |
| 世界历史 | Prehistory / Timeline Canon |
| 人物身份与人格 | Character Canon |
| 地点与航路 | Map Canon / Location Registry |
| 外部科学知识 | Controlled Reference Library |
| 用户经历 | Route Continuity |
| 用户与角色关系 | Personal Relationship State |
| 用户已知地图与事实 | Personal Discovery State |
| 用户作品 | Personal Artifact State |
| 当前公共世界状态 | World State Ledger |
| 单集生产字段 | Story Schema |
| 单集写作与验收 | Narrative Production Spec |
| 当前技术边界 | Runtime Config |

---

# 4. 推荐仓库目录

建议仓库建立：

```text
/galaxy
  /canon
    world/
    prehistory/
    character/
    map/
    safety/

  /course
    source/
    mappings/

  /reference
    assets/
    registry/

  /schema
    story/
    episode_delta/
    registry/
    runtime/

  /state
    shared/
    route/
    relationship/
    discovery/
    artifact/
    thread/

  /production
    specs/
    prompts/
    renderers/
    validators/

  /episodes
    candidates/
    approved/
    released/

  /releases
    manifests/
    changelogs/
    migrations/

  /tests
    canon/
    continuity/
    character/
    schema/
    regression/
```

不要把：

> 世界文档、运行数据、Prompt、Episode脚本

全部塞在同一个目录。

---

# 5. Release Manifest

每次正式内容发布必须拥有：

```yaml
galaxy_release:
  release_id: GALAXY-CONTENT-2026.09-R1

  world_canon_version: "1.0"
  character_canon_version: "0.1"
  prehistory_version: "0.1"
  map_canon_version: "0.1"
  reference_library_version: "0.1"

  story_schema_version: "0.3"
  production_spec_version: "0.2"
  runtime_config_version: "0.1"

  course_source_version: "5.0.7"

  status:
    CANDIDATE

  approved_by:
    canon_owner:
    engineering_owner:

  created_at:
```

---

# 6. Release 的意义

系统生产任何一集时：

必须记录：

```text
release_id
```

以后如果 EP037 出现问题：

可以追溯：

> 它是根据哪一版世界、角色、课程和Schema生成。

禁止：

> 永远只读取“当前最新文档”。

否则旧Episode不可复现。

---

# 7. Registry 总览

第一阶段建议建立以下 Registry。

```text
CanonRegistry
CharacterRegistry
LocationRegistry
GuestCharacterRegistry
ReferenceRegistry
ForbiddenTropeRegistry
CourseRegistry
```

后续可增加：

```text
OrganizationRegistry
TechnologyRegistry
SpeciesRegistry
VisualAssetRegistry
```

---

# 8. CanonRegistry

目的：

> 把 World Canon 中真正需要机器识别的规则从长文中抽出来。

建议结构：

```yaml
canon_item:
  canon_id: WC-001
  title:
  category:
  canon_level:
    C0 | C1 | C2 | C3

  truth_domain:
    FICTIONAL_WORLD_FACT

  statement:

  status:
    ACTIVE | DEPRECATED

  source_doc:
  source_section:

  introduced_version:
  deprecated_version:

  allowed_mutation:
    OWNER_ONLY | REVIEW_REQUIRED | EPISODE_ALLOWED

  tags: []
```

---

# 9. CharacterRegistry

```yaml
character:
  character_id: TOKI

  status:
    ACTIVE

  canon_version: "0.1"

  role_type:
    MAIN_COMPANION

  allowed_locations: []

  voice_profile_ref:
  relationship_rules_ref:
  knowledge_boundary_ref:

  canon_source:
    DOC-002
```

不要在 Registry 重复完整人物圣经。

Registry 负责：

> 定位与引用。

---

# 10. Character Knowledge Boundary

必须独立存储：

```yaml
knowledge_boundary:
  character_id: KONTI

  confirmed_known: []
  likely_known: []
  private_knowledge: []
  forbidden_until: []
```

生产时：

> World Truth 不等于 Character Knowledge。

---

# 11. LocationRegistry

```yaml
location:
  location_id: L01

  canonical_name:
  working_name:

  region_id:

  location_type:

  canon_level:

  current_state:
    OPEN

  connected_routes: []

  regular_characters: []

  first_appearance:

  source_doc:
```

Location State 后续由 Ledger 更新。

---

# 12. GuestCharacterRegistry

至少支持：

```yaml
guest_character:
  registry_id:
  display_name:
  avatar_type:
  canon_tier:
  cameo_modes:
  voice_style:
  allowed_topics:
  forbidden_uses:
  consent_scope:
  likeness_permission:
```

儿童相关角色：

> 必须支持 guardian consent / pseudonym / abstract avatar 等字段。

---

# 13. ReferenceRegistry

```yaml
reference_asset:
  asset_id:

  source_org:
  source_url:

  source_tier:

  truth_domain:
    REAL_WORLD_FACT

  truth_label:
    FACT | ESTIMATE | HYPOTHESIS | OPEN_QUESTION

  core_fact:

  story_use_mode:
    RULE | EVIDENCE | TOOL | DEEP_NOTE | BACKGROUND_ONLY

  preferred_voice:

  age_min:
  age_max:

  verification_status:
    VERIFIED

  last_verified:
  freshness_policy:

  copyright_status:
  visual_license_status:
```

---

# 14. ForbiddenTropeRegistry

第一批：

```text
TROPE-SHIP-DAMAGE-DEFAULT
TROPE-ENERGY-SHORTAGE-DEFAULT
TROPE-SYSTEM-ALARM-DEFAULT
TROPE-TOKI-PLAYS-DUMB
TROPE-KONTI-KNOWS-EVERYTHING
TROPE-PROTI-REPAIRS-EVERYTHING
TROPE-AJI-ETHICS-CALLBACK
TROPE-MANDATORY-CRISIS
TROPE-FIXED-REGION-ROTATION
TROPE-FIXED-CHARACTER-ROTATION
TROPE-EMPTY-POINT-REWARD
TROPE-KNOWLEDGE-RESKINNING
```

支持：

```yaml
trope:
  id:
  severity:
    BLOCK | WARN
  detection_hint:
  exception_requires_reason: true
```

---

# 15. CourseRegistry

课程继续来自：

> Course Source of Truth。

建议生成稳定ID：

```text
COURSE-001
COURSE-002
...
```

至少映射：

```yaml
course:
  course_id:
  package:
  unit:
  lesson_index:
  title:
  source_version:
```

不要直接依赖：

> 课程标题作为主键。

---

# 16. ContextAssembler
## 本轮工程最重要模块

建议 P0 建立：

# `ContextAssembler`

职责：

> 为一个 Episode 生成“足够但不过量”的生产 Context。

---

# 17. ContextAssembler 输入

```yaml
assemble_context:
  release_id:
  course_id:
  episode_id:
  user_id:
  route_id:
  runtime_config_version:
```

如果 Pilot：

```yaml
user_id: TEST_USER
route_id: PILOT_ROUTE
```

---

# 18. ContextAssembler 读取

至少读取：

```text
Course Contract
World Canon subset
Relevant Prehistory
Eligible Character data
Character Knowledge
Location data
Current Timeline
Route Continuity
Relationship State
Discovery State
Approved Reference Assets
Runtime Config
Safety Rules
```

---

# 19. ContextAssembler 不应该做什么

它不负责：

- 写故事；
- 改Canon；
- 推断新世界规则；
- 自动改变角色关系；
- 搜索任意互联网资料。

它只负责：

> **Retrieve + Filter + Resolve + Assemble**

---

# 20. Relevant Context Filter

不得把全部：

- 6万字前史；
- 完整 World Canon；
- 全角色Bible；

每次全部塞入生成上下文。

推荐过程：

```text
Course Tags
+
Episode Need
+
Character IDs
+
Location IDs
+
Open Threads
↓
Retrieve relevant chunks/items
↓
Resolve permissions
↓
Assemble Context Snapshot
```

---

# 21. Context 输出

输出至少符合 Story Schema v0.3：

```yaml
context_snapshot:
  timeline:
  location:
  route_continuity:
  user:
  relationship_state:
  discovery_state:
  character_state:
  world_state:
  knowledge_state:
```

---

# 22. StoryProfileValidator

职责：

> 在正式故事生产以前阻止明显错误。

输入：

```text
Story Profile v0.3
```

输出：

```yaml
validation:
  status:
    PASS | WARN | BLOCK

  issues: []
```

---

# 23. Pre-generation Gates

至少实现：

```text
Course Gate
Canon Gate
Character Gate
Timeline Gate
Location Gate
Epistemic Gate
Reference Gate
Safety Gate
Runtime Gate
```

---

# 24. Gate Severity

统一：

```text
BLOCK
WARN
INFO
```

## BLOCK 示例

- 未认识康缇却写成老朋友；
- 引用不存在课程；
- 角色知道未来信息；
- 修改C0 Canon；
- 使用未批准高风险事实。

## WARN 示例

- 新建C3临时NPC；
- 新建临时地点；
- 单集新增较多世界词汇。

---

# 25. NarrativeGenerator

输入：

```text
Approved Story Profile
+
Narrative Production Spec
+
Relevant Character Voice
```

输出：

```text
Story Beats
Script
Reader
Detected State Changes
Proposed Episode Delta
```

Generator 不允许：

> 写 Canon Registry。

---

# 26. Story Beats 阶段必须单独存在

不要：

> 从 Story Profile 直接一次性生成最终Script。

推荐：

```text
Story Profile
↓
Story Beats
↓
Beat Review
↓
Narrative Rendering
```

这样：

> 结构错误在低成本阶段修正。

---

# 27. DeltaExtractor

正式生产完成后运行。

自动抽取：

```text
new_entities
new_locations
new_facts
relationship_changes
location_changes
knowledge_changes
artifacts
new_threads
resolved_threads
```

然后比较：

```text
Expected Delta
vs
Detected Delta
```

---

# 28. Unauthorized Mutation Detector

如果 Story Profile 没有：

> Shared Canon Proposal

但生成文本出现：

> 新历史 / 新政权 / 新关键技术 / 新长期地点

必须：

```text
BLOCK RELEASE
```

错误码建议：

```text
UNAUTHORIZED_CANON_MUTATION
```

---

# 29. StateWriter

只写允许的状态。

第一阶段：

```text
Route Continuity
Personal Relationship State
Personal Discovery State
Personal Artifact State
Episode Release Metadata
```

默认：

> 不写 Shared Canon。

---

# 30. Shared Canon Proposal Queue

P1 可以实现。

结构：

```yaml
canon_proposal:
  proposal_id:
  source_episode:
  proposed_change:
  canon_level:
  reason:
  affected_entities:
  migration_required:
  status:
    PROPOSED | REVIEWING | APPROVED | REJECTED
```

只有 Canon Owner：

> APPROVE。

---

# 31. 推荐模块架构

```text
CourseRegistry
CanonRegistry
CharacterRegistry
LocationRegistry
ReferenceRegistry
RuntimeRegistry

        ↓

ContextAssembler

        ↓

StoryProfileBuilder
        ↓
StoryProfileValidator

        ↓

StoryPlanner
        ↓
StoryBeatReviewer

        ↓

NarrativeGenerator
InteractionRenderer

        ↓

RuntimeAdapter

        ↓

QA Pipeline

        ↓

DeltaExtractor

        ↓

StateWriter
CanonProposalQueue
```

---

# 32. Shared / Route / Personal 数据边界

## Shared

```text
World Canon
Character Canon
Prehistory
Map Canon
Public World State
```

## Route

```text
重要个人经历
个人选择
未解决个人线程
```

## Relationship

```text
用户与角色关系状态
共同记忆
角色对用户印象
```

## Discovery

```text
用户知道的地点
听说过的地点
知道的历史
知道的世界事实
```

## Artifact

```text
用户作品
航行日志
个人银河舱资产
```

---

# 33. 不做平行宇宙

多用户第一阶段采用：

> 非同步个人航线连续性。

不尝试解释：

> 一个NPC如何实时同时与十万用户交互。

当前不建设：

- MMO Shared Timeline；
- 用户间实时一致性；
- 全局NPC单一时间锁。

未来需要时另建：

> Community Timeline。

---

# 34. Runtime Config

建议独立文件：

```yaml
runtime_config:
  version: "0.1"

  episode_duration:
    min: 5
    max: 8

  max_primary_dialogue_characters: 2

  max_core_interactions: 2

  supported_interactions:
    - choice
    - sort
    - select

  complex_branching:
    enabled: false

  persistent_inventory:
    enabled: false

  audio:
    enabled: TBD

  visual_generation:
    enabled: true
```

这些不能散落在 Prompt。

---

# 35. RuntimeAdapter

负责把：

> Approved Script

转成：

> 当前产品真正能跑的形式。

它可以改变：

- 页数；
- UI；
- 转场；
- 动画表现；
- 互动形式。

它不能改变：

- 学习目标；
- Canon；
- 角色身份；
- 关键选择意义。

---

# 36. QA Pipeline

建议拆成：

```text
Schema QA
Course QA
Canon QA
Continuity QA
Epistemic QA
Character QA
Reference QA
Safety QA
Readability QA
Agency QA
Runtime QA
Delta QA
```

---

# 37. 自动QA 与人工QA边界

适合自动：

- ID是否存在；
- 角色是否已认识；
- 地点是否可达；
- 角色是否知道信息；
- 禁用套路；
- Runtime上限；
- Schema格式；
- Delta差异。

适合人工：

- 好不好看；
- 人物是否自然；
- 文笔；
- 幽默；
- 情绪；
- 是否低幼；
- 是否值得继续。

---

# 38. EP001 作为 Golden Sample

EP001 v0.2 应进入：

```text
/tests/golden/EP001
```

包含：

```text
story_profile.yaml
context_snapshot.yaml
story_beats.md
script.md
episode_delta.yaml
validation_expected.yaml
```

以后任何生产系统重大修改：

> 都重新跑 EP001。

---

# 39. Golden Sample 用途

如果更换：

- 模型；
- Prompt；
- ContextAssembler；
- Story Schema；
- Character Data；

必须检查：

> EP001 是否仍满足预期。

这就是：

> Narrative Regression Test。

---

# 40. 建议第一批 Golden Tests

至少：

```text
EP001：概念与初遇
EP002：事实核查
EP003：问题定义
Pilot-Prototype：原型测试
```

四类足够覆盖：

- AI概念；
- Evidence；
- Problem Definition；
- Prototype。

---

# 41. 旧系统迁移目标

第一阶段不要推翻现有系统。

采用：

> **Extract → Wrap → Dual-run → Replace**

---

# 42. Migration Step 1｜Inventory

CodeX 先扫描当前仓库：

- Prompt；
- 配置；
-硬编码角色；
-课程解析；
-星域逻辑；
-奖励；
-分支；
-素材调用。

形成：

```text
LEGACY_BEHAVIOR_INVENTORY.md
```

---

# 43. 必查硬编码

重点搜索：

```text
default_ship_damage
default_repair_mission
default_star_particle_collection
toki_silly_behavior
single_companion_only
mandatory_crisis_opening
mandatory_reward
fixed_character_rotation
fixed_star_region_rotation
```

以及语义近似实现。

---

# 44. Migration Step 2｜Classify

每个旧行为标记：

```text
KEEP
CONFIGURE
DEPRECATE
REMOVE
UNKNOWN
```

---

# 45. Migration Step 3｜Extract

把仍有效内容：

> 从 Prompt / 代码中抽出

进入：

- Runtime Config；
- Registry；
- Production Spec；
- Schema。

---

# 46. Migration Step 4｜Dual-run

旧系统：

```text
Legacy Generator
```

新系统：

```text
ContextAssembler + StoryProfile + New Generator
```

对同一课程：

> 双跑。

比较：

- 教育目标；
-剧情质量；
-角色；
-重复套路；
-连续性；
-运行成本。

---

# 47. Migration Step 5｜Replace

只有当：

> Golden Samples + Regression PASS

以后：

才把新管线设为默认。

---

# 48. Migration 非目标

第一阶段不要求：

- 全部117课重生成；
- 完整历史数据库；
- 开放世界；
- 全自动Canon更新；
- 多人世界；
- 复杂剧情分支。

目标：

> **先解决失忆、乱写、硬编码和模板化。**

---

# 49. P0 / P1 / P2

## P0｜必须完成才能进入新生产

1. Release Manifest
2. Stable IDs
3. Core Registries
4. ContextAssembler
5. Story Schema v0.3 implementation
6. StoryProfileValidator
7. Character Eligibility
8. Epistemic Permission
9. Route Continuity
10. Personal Relationship State
11. Episode Delta
12. DeltaExtractor
13. Forbidden Trope Registry
14. Legacy Hardcode Inventory
15. EP001 Golden Sample

---

## P1｜第一阶段稳定后

1. Location Ledger
2. Knowledge Ledger
3. Thread Ledger
4. Shared Canon Proposal Queue
5. Reference Auto-retrieval
6. Location State Validation
7. Automated Narrative Regression
8. Release Diff
9. Migration Assistant

---

## P2｜以后再考虑

1. Community Timeline
2. 多用户公共事件
3. 复杂Branch
4. Multiplayer consistency
5. 自动Canon Promotion
6. 开放地图动态状态
7. 高级NPC全局状态

---

# 50. P0 Definition of Done

P0完成必须满足：

> 使用 `course_id + route_id + release_id`

可以：

1. 自动组装 Context；
2. 生成合法 Story Profile；
3. 校验人物/地点/知识权限；
4. 生产 Episode；
5. 抽取 Episode Delta；
6. 写回个人状态；
7. 阻止未授权Canon修改；
8. 对 EP001 Golden Test PASS。

---

# 51. GitHub EPIC 建议

建议最少拆六个 EPIC。

---

# EPIC 01｜Galaxy Content Foundation

目标：

> 建立 Source-of-Truth 和 Registry。

Issues：

### ISSUE 01.1
建立 Release Manifest

### ISSUE 01.2
建立 CanonRegistry

### ISSUE 01.3
建立 CharacterRegistry

### ISSUE 01.4
建立 LocationRegistry

### ISSUE 01.5
建立 ReferenceRegistry

### ISSUE 01.6
建立 ForbiddenTropeRegistry

---

# EPIC 02｜Context Assembly

### ISSUE 02.1
ContextAssembler 基础接口

### ISSUE 02.2
Course Context Resolver

### ISSUE 02.3
Character Eligibility Resolver

### ISSUE 02.4
Epistemic Permission Resolver

### ISSUE 02.5
Location Context Resolver

### ISSUE 02.6
Route / Relationship / Discovery Context Resolver

---

# EPIC 03｜Story Schema Runtime

### ISSUE 03.1
实现 Story Schema v0.3 JSON Schema

### ISSUE 03.2
StoryProfileValidator

### ISSUE 03.3
Persistence Policy

### ISSUE 03.4
Episode Delta Schema

### ISSUE 03.5
DeltaExtractor

---

# EPIC 04｜Continuity & State

### ISSUE 04.1
Route Continuity Store

### ISSUE 04.2
Personal Relationship State

### ISSUE 04.3
Personal Discovery State

### ISSUE 04.4
Personal Artifact State

### ISSUE 04.5
StateWriter

---

# EPIC 05｜QA & Regression

### ISSUE 05.1
Canon Validator

### ISSUE 05.2
Character Validator

### ISSUE 05.3
Epistemic Validator

### ISSUE 05.4
Forbidden Trope Validator

### ISSUE 05.5
Runtime Validator

### ISSUE 05.6
EP001 Golden Test

### ISSUE 05.7
Regression Runner

---

# EPIC 06｜Legacy Migration

### ISSUE 06.1
Legacy Prompt / Hardcode Inventory

### ISSUE 06.2
Hardcode Classification

### ISSUE 06.3
Extract Runtime Config

### ISSUE 06.4
Remove deprecated defaults

### ISSUE 06.5
Dual-run pipeline

### ISSUE 06.6
New pipeline default switch

---

# 52. Issue Template

建议 CodeX 自动生成 Issue 时统一：

```markdown
# Background

# Source Specs
- Release:
- Story Schema:
- Production Spec:
- Canon refs:

# Current Behavior

# Required Behavior

# Data / Schema Impact

# Migration Impact

# Acceptance Criteria

# Tests

# Dependencies

# Out of Scope
```

---

# 53. Acceptance Criteria 写法

不要：

> “支持 Canon。”

应写成可测条件。

例如 ContextAssembler：

```text
Given:
- COURSE-001
- route state where TOKI = NOT_MET
- release GALAXY-CONTENT-2026.09-R1

When:
ContextAssembler assembles context

Then:
- TOKI eligibility is available=true / introduced=false
- KONTI appearance_allowed=false
- only relevant L01 location context is included
- unrelated Aji private history is absent
```

---

# 54. ISSUE 02.1 Acceptance Criteria 示例

ContextAssembler：

- [ ] 输入 `release_id/course_id/route_id`
- [ ] 读取正确版本 Source
- [ ] 输出合法 `context_snapshot`
- [ ] 不包含无关角色私人信息
- [ ] 角色知识权限正确
- [ ] 地点状态正确
- [ ] 结果可复现
- [ ] 同一Release同一State输出稳定

---

# 55. ISSUE 03.2 Acceptance Criteria 示例

StoryProfileValidator：

- [ ] 缺 `course_id` → BLOCK
- [ ] 不存在角色ID → BLOCK
- [ ] 未认识角色被配置为老朋友 → BLOCK
- [ ] Runtime超出互动上限 → WARN/BLOCK按配置
- [ ] 未批准Reference → BLOCK
- [ ] C3临时对象 → WARN
- [ ] 合法EP001 Profile → PASS

---

# 56. ISSUE 05.3 Acceptance Criteria 示例

Epistemic Validator：

- [ ] World Canon存在某事实但角色 `known_information` 不包含 → 角色不得使用
- [ ] 角色通过本集获得事实 → 可写入 `character_learned`
- [ ] 用户不知道的世界事实不得自动进入 Reader
- [ ] 违反时返回明确错误码

---

# 57. ISSUE 06.1 Acceptance Criteria 示例

Legacy Inventory：

必须输出：

```text
File
Location
Behavior
Current Purpose
Canon/Product/Runtime classification
Recommendation
Risk
```

覆盖：

> 代码 + Prompt + 配置。

---

# 58. 推荐 Milestone

## M0｜Foundation

Release / Registry / Schema

## M1｜Context & Continuity

ContextAssembler + State

## M2｜Production Integration

StoryProfile + Generator + Delta

## M3｜Regression & Migration

QA + Golden Tests + Dual-run

## M4｜Pilot Release

用 EP001–003 真正跑新链路

---

# 59. M0 Exit Criteria

- 所有核心ID稳定；
- Manifest可读取；
- Registry能查询；
- Story Schema已实现；
- 文档Source可追溯。

---

# 60. M1 Exit Criteria

使用 EP001：

> 能正确知道用户未见过Toki。

EP002：

> 能读取EP001关系变化。

EP003：

> 能读取前两集用户经历。

做到：

> 不失忆。

---

# 61. M2 Exit Criteria

能够：

```text
course
→ context
→ profile
→ script
→ delta
```

完整跑一次。

不要求：

> 自动写出最终最好故事。

要求：

> 数据链成立。

---

# 62. M3 Exit Criteria

- EP001 Golden PASS
- 至少4种课程类型Regression PASS
- 未授权Canon变化可阻止
- Legacy默认套路不再自动发生
- Dual-run结果可比较

---

# 63. M4 Exit Criteria

至少：

> EP001–003

由新Pipeline完整生产。

人工评审通过：

- Course
- Character
- Continuity
- Readability
- Runtime

---

# 64. 建议第一阶段不做UI大改

生产系统升级：

> 可以先在后台完成。

不应同时强绑定：

- 全新地图UI；
- 全新银河舱；
- 全新成就系统；
- 多人功能。

否则范围失控。

---

# 65. 内容更新机制
## 工程必须从第一天预留

任何内容更新以后：

> 不要求李光重新读完整文件。

必须拥有：

```text
CHANGESET
```

---

# 66. CHANGESET 最小格式

```yaml
changeset:
  release_id:

  changed_sources: []

  added: []

  changed: []

  deprecated: []

  removed: []

  engineering_impact:
    schema:
    prompts:
    registries:
    state:
    migration:
    regeneration:

  affected_episodes: []
```

---

# 67. Update Class

建议：

```text
PATCH
CONTENT_UPDATE
SCHEMA_CHANGE
CANON_BREAKING_CHANGE
```

---

# 68. PATCH

例：

- 错字；
- 文案；
- 非结构化说明。

通常：

> 无迁移。

---

# 69. CONTENT_UPDATE

例：

- 爱今新增过去；
- L03新增历史；
- Character Voice调整。

需要：

> Impact Scan。

不一定要迁移已有用户状态。

---

# 70. SCHEMA_CHANGE

例：

新增：

```text
route_continuity.relationship_memory
```

必须：

- schema migration；
- test；
- compatibility check。

---

# 71. CANON_BREAKING_CHANGE

例：

改变现实航线根本规则。

必须：

- Canon Owner批准；
- Impact Analysis；
- affected episode scan；
- migration；
- regression；
- release note。

---

# 72. 内容更新自动生成 Issue

理想流程：

```text
New Content Release
↓
CHANGESET
↓
CodeX Impact Analysis
↓
GitHub Issue Draft
↓
Human Review
↓
Milestone
↓
Implementation
↓
PR
↓
CI
↓
Release
```

---

# 73. Repository CI 建议

每次 Canon / Schema PR：

自动跑：

```text
Schema validation
Registry ID validation
Broken reference check
Deprecated field check
Golden episode regression
Forbidden trope regression
Migration required check
```

---

# 74. Broken Reference

例如删除：

```text
location_id: L03
```

但Episode仍引用：

> CI FAIL。

---

# 75. Deprecated Field

例如新文件继续写：

```text
canon_writeback
```

v0.3 已废弃。

CI 应：

> FAIL / WARN。

替代：

```text
persistence_policy
```

---

# 76. Version Compatibility

建议明确：

```text
Story Schema 0.3
compatible_with:
  Production Spec >=0.2
```

不要让：

> 新Schema + 旧Prompt

无声组合。

---

# 77. Handoff 给 CodeX 的第一任务

不要让 CodeX 一上来：

> 改代码。

第一任务应是：

# Repository Audit

让 CodeX：

1. 读取当前仓库；
2. 对照本 Handoff Pack；
3. 找现有对应模块；
4. 列 Gap；
5. 生成 EPIC / ISSUE Draft；
6. 不直接大规模重构。

---

# 78. Repository Audit 输出

至少：

```text
Current Architecture
Existing Assets
Existing Prompt Flow
Hardcoded Behaviors
Reusable Modules
Missing Modules
Migration Risks
Recommended EPICs
Issue Drafts
```

---

# 79. 李光的第一任务

李光不需要：

> 先读完全部世界观文档。

他优先读：

1. Engineering Handoff Pack
2. Story Schema v0.3
3. Narrative Production Spec v0.2
4. EP001 Golden Sample

World Canon / Character Canon：

> 作为需要时查阅的上游事实源。

---

# 80. Canon Owner 的职责

用户 / Product Owner：

- 批准世界重大变化；
- 批准角色重大变化；
- 批准Release；
- 决定C0/C1；
- 处理重大冲突。

不应该：

> 每个工程字段都亲自维护。

---

# 81. ChatGPT 内容侧职责

建议继续：

- World Canon；
- Character Canon；
- Story System；
- Reference Library；
- Narrative QA；
- Content Release；
- CHANGESET。

不负责：

> 替代李光做最终生产系统架构决策。

---

# 82. 用户 CodeX 职责

建议：

> Canon Systems Editor。

负责：

- 文档结构化；
- 版本检查；
- Schema一致性；
- Impact Analysis；
- GitHub Issue设计；
- PR Review；
- Migration验证。

---

# 83. 李光 CodeX 职责

建议：

> Implementation / Pipeline Engineer。

负责：

- 工程实现；
- Registry；
- Schema；
- ContextAssembler；
- Validator；
- State；
- Tests；
- Migration。

---

# 84. 风险一｜过度工程化

不要第一期就做：

> 完整知识图谱。

当前只需要：

> 能稳定生产117课。

如果一项架构：

> 对首批课程生产没有明显价值，

优先P1/P2。

---

# 85. 风险二｜把Markdown直接当数据库

Markdown：

> 适合人审阅。

系统运行：

> 应使用结构化 Registry / Schema。

不要写：

> 用模型每次重新从8万字文档中猜设定。

---

# 86. 风险三｜内容团队重复定义

如果 Character Canon更新：

> Story Prompt 不应保留旧版人物全文。

否则：

> Source Drift。

---

# 87. 风险四｜自动生成覆盖人工判断

机器可以检查：

> 是否违规。

但不能可靠判断：

> 是否真的好看。

人工 Narrative Review：

> 必须存在。

---

# 88. 风险五｜过早批量117课

新Pipeline上线前：

先跑：

> EP001–003 + Prototype Golden。

通过：

> 再规模化。

---

# 89. 第一批工程成功标准

不是：

> “所有Canon都录入数据库。”

而是：

> EP001到EP003能够连续生产，而且系统知道上一集发生过什么。

如果做到：

```text
EP001认识拓奇
↓
EP002记得已经认识
↓
EP003关系继续增长
```

并同时：

> 不让康缇提前出现；

说明核心架构已经成立。

---

# 90. 第一批回归测试

### Test 01
EP001首次进入

Expected：

```text
TOKI = NOT_MET → EARLY_ACQUAINTANCE
```

### Test 02
EP002启动

Expected：

```text
TOKI != NOT_MET
```

### Test 03
未见康缇

Expected：

```text
KONTI.appearance_allowed = false
```

### Test 04
角色知识泄漏

Expected：

> BLOCK

### Test 05
新增银河战争历史

Expected：

> UNAUTHORIZED_CANON_MUTATION

### Test 06
默认飞船坏

Expected：

> TROPE WARNING / BLOCK

---

# 91. 最小API / Function Contract 建议

示意：

```text
assemble_context(release_id, course_id, route_id)
validate_story_profile(profile)
generate_story_beats(profile, context)
render_narrative(beats, rendering_config)
extract_delta(output, expected_delta)
validate_output(output, context)
write_state(route_id, approved_delta)
```

具体语言和框架：

> 由李光工程团队决定。

---

# 92. 可观测性

生产系统至少记录：

```text
episode_id
course_id
release_id
route_id
model_version
prompt_version
schema_version
validation_result
generation_time
token_usage
qa_result
delta_result
```

以后才能：

> Debug故事为什么变差。

---

# 93. Prompt Version

Prompt也必须版本化：

```text
story_planner_prompt_version
narrative_writer_prompt_version
qa_prompt_version
```

否则换Prompt以后：

> 无法回溯质量变化。

---

# 94. Model Version

同理记录：

> 生成模型。

因为模型变化：

> 可能直接改变人物与文风。

---

# 95. 人工审核状态

Episode建议：

```text
DRAFT
AI_QA_PASS
HUMAN_REVIEW
CANON_CANDIDATE
APPROVED
RELEASED
DEPRECATED
```

不要：

> AI生成后直接上线。

---

# 96. Episode Release Record

```yaml
episode_release:
  episode_id:
  course_id:
  release_id:

  story_profile_version:
  script_version:

  prompt_versions:
  model_versions:

  qa_result:

  episode_delta:

  status:

  approved_by:
```

---

# 97. CHANGESET 与 Episode Release 分离

CHANGESET：

> 描述“基础系统改了什么”。

Episode Release：

> 描述“这一集根据什么生成”。

两者不要混。

---

# 98. 第一阶段交付物清单

交给李光前应至少包含：

```text
01 Galaxy Engineering Handoff Pack v0.1
02 Story Schema v0.3
03 Narrative Production Spec v0.2
04 World Canon v1.0
05 Character Pack v0.1
06 Prehistory v0.1
07 Map Canon v0.1
08 Reference Library v0.1
09 Course Source v5.0.7
10 EP001 v0.2 Golden Sample
11 EP001 Review
12 Release Manifest v0.1
13 CHANGESET template
```

---

# 99. 李光不需要第一天读完的文件

不要求他：

> 从头精读全部World Canon和前史。

Engineering Handoff Pack 应告诉他：

> 哪些是机器应该读取的上游源。

内容细节：

> 由ContextAssembler消费。

---

# 100. 交接会议建议顺序

1. 为什么旧模式会失忆 / 模板化
2. One Canon
3. Source of Truth
4. ContextAssembler
5. Story Profile
6. Episode Delta
7. P0
8. Legacy Migration
9. Golden Sample
10. Update Mechanism

不要：

> 从银河历史讲起。

---

# 101. 当前研发目标一句话

> **把“每课独立Prompt生成故事”升级成“读取持续世界状态后生产故事，并把变化写回状态”的内容生产系统。**

---

# 102. P0 研发成功一句话

> **下一集知道上一集发生过什么，同时不会因为知道完整Canon而让角色提前知道不该知道的事。**

---

# 103. 长期目标一句话

> **让银河在数百次AI生成之后，仍然像同一个世界、同一群人、同一段正在继续的人生。**

---

# 104. 本 Handoff Pack 的冻结结论

当前工程方向正式确定：

```text
Source of Truth
↓
Versioned Release
↓
Registry
↓
ContextAssembler
↓
Story Profile
↓
Validation
↓
Story Beats
↓
Narrative Rendering
↓
Runtime Adapter
↓
QA
↓
Episode Delta
↓
State Writeback
↓
Regression
```

不再采用：

```text
完整教案
+
巨大Prompt
+
一次性生成
+
下一集重新开始
```

这就是本轮系统升级的工程本质。

---

# 105. 下一步

Engineering Handoff Pack v0.1 完成以后：

不应立刻开发。

下一步应完成：

# Update / Release / Migration Protocol v0.1

并做一次：

# Handoff Dry Run

用一个真实的小变更完整测试：

```text
Character Canon更新
↓
CHANGESET
↓
Impact Analysis
↓
CodeX生成Issue
↓
Migration判断
↓
Regression
↓
Release
```

Dry Run成功以后：

> 才认为整个交接机制完成。
