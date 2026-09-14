# Galaxy Update / Release / Migration Protocol v0.1
## 银河内容系统更新、版本发布与迁移协议

**版本：v0.1**  
**日期：2026-09-11**  
**状态：ENGINEERING HANDOFF CANDIDATE**  
**适用：Galaxy World Canon / Character Canon / Prehistory / Map Canon / Reference Library / Story Schema / Production Spec / Runtime Config / Episode Pipeline**

---

# 0. 协议目的

本协议解决一个长期问题：

> **当内容团队以后继续更新世界、角色、Schema、Reference 或生产规则时，研发系统怎样稳定知道“改了什么、影响哪里、要不要迁移、旧内容是否失效”。**

目标不是把内容更新变成重型流程。

目标是避免：

- 修改一份 Markdown 后无人知道影响范围；
- Prompt、Schema、代码和 Canon 各自漂移；
- 旧 Episode 继续使用废弃字段；
- 用户状态在版本升级后失效；
- 李光团队每次重新读一遍全部文档；
- 内容更新只能靠口头同步。

正式流程：

```text
Change Proposal
↓
Change Classification
↓
Impact Analysis
↓
CHANGESET
↓
Release Candidate
↓
GitHub Issues / Migration Plan
↓
Implementation
↓
Regression
↓
Canon / Product Approval
↓
Release
↓
Post-release Verification
```

---

# 1. 基本原则

## 1.1 内容更新必须可追溯

任何正式更新必须知道：

```text
谁改的
改了什么
为什么改
从哪个版本开始生效
影响哪些模块
是否需要迁移
是否需要重生成内容
是否需要回归测试
```

---

## 1.2 文档版本与内容 Release 分离

单份文档拥有自己的版本：

```text
World Canon v1.0
Character Canon v0.1
Story Schema v0.3
Production Spec v0.2
```

而一次完整内容发布使用：

```text
GALAXY-CONTENT-2026.09-R1
```

Release Manifest 记录：

> 本次发布具体由哪些文档版本组成。

---

# 2. 版本规则

建议统一：

```text
MAJOR.MINOR.PATCH
```

当前已有 `v0.1 / v0.2 / v0.3 / v1.0` 可以继续使用。

---

## 2.1 PATCH

适用：

- 错字；
- 语句澄清；
- 不改变含义的文案；
- 元数据修正。

例：

```text
v0.3.0 → v0.3.1
```

通常：

> 无工程迁移。

---

## 2.2 MINOR

适用：

- 新增兼容字段；
- 新增角色资料；
- 新增地点；
- 新增 Reference Asset；
- 新增不破坏旧逻辑的生产规则。

例：

```text
v0.3 → v0.4
```

通常：

> 需要 Impact Scan，但不一定迁移。

---

## 2.3 MAJOR

适用：

- 旧字段失效；
- 世界基础规则改变；
- 持久化模型改变；
- 角色身份重构；
- 数据结构不兼容；
- 旧 Episode 语义可能错误。

例：

```text
v0.x → v1.0
```

必须：

> Migration + Regression + Approval。

---

# 3. Release ID

推荐格式：

```text
GALAXY-CONTENT-YYYY.MM-RN
```

例如：

```text
GALAXY-CONTENT-2026.09-R1
GALAXY-CONTENT-2026.09-R2
GALAXY-CONTENT-2026.10-R1
```

Release ID 不替代：

> 单文档版本。

---

# 4. Change Class

每一个正式变更必须先分类。

统一：

```text
PATCH
CONTENT_UPDATE
SCHEMA_CHANGE
CANON_BREAKING_CHANGE
RUNTIME_CHANGE
REFERENCE_UPDATE
```

---

# 5. PATCH

例：

- 康缇一处错字；
- 地名英文拼写修正；
- 文档说明更清楚但不改变规则。

默认影响：

```yaml
migration: NONE
regeneration: NONE
engineering_issue: OPTIONAL
regression: LIGHT
```

---

# 6. CONTENT_UPDATE

例：

- 新增一段人物历史；
- 新增一个固定地点；
- 补充角色普通生活；
- 调整 Character Voice；
- 新增长期线程。

默认：

```text
Impact Analysis required
```

重点检查：

- 已发布 Episode 是否冲突；
- Character / Location State；
- ContextAssembler检索；
- Golden Sample。

---

# 7. SCHEMA_CHANGE

例：

```text
新增字段
删除字段
字段改名
状态模型改变
Persistence语义改变
```

必须：

```text
Schema Migration
Compatibility Check
Regression
Issue Generation
```

---

# 8. CANON_BREAKING_CHANGE

例如：

- 现实航线本质被重新定义；
- 四星域结构被推翻；
- 主角色身份改变；
- 已发生历史被Retcon。

必须：

```text
Canon Owner Approval
Affected Episode Scan
Data Impact Scan
Migration
Regression
Release Note
```

不能：

> 默默覆盖旧Canon。

---

# 9. RUNTIME_CHANGE

例：

```text
最大互动数 2 → 4
支持新的交互组件
开始支持长期资产
增加音频
```

重点检查：

> Production Spec 与 RuntimeAdapter。

通常不应该：

> 改 World Canon。

---

# 10. REFERENCE_UPDATE

例：

- NASA页面更新；
- 数据数字变化；
- 一个事实从 HYPOTHESIS 变成更强证据；
- URL失效。

流程：

```text
VERIFY
→ UPDATE ASSET
→ FIND DEPENDENT EPISODES
→ RECHECK CLAIMS
```

---

# 11. Change Proposal

任何重要变更应先创建：

```yaml
change_proposal:
  change_id:
  author:
  date:

  source:
  target:

  summary:
  reason:

  proposed_change_class:

  canon_level_if_applicable:

  affected_entities: []

  urgency:
```

---

# 12. Proposal 状态

统一：

```text
DRAFT
READY_FOR_REVIEW
REVIEWING
APPROVED_FOR_IMPACT
REJECTED
```

此阶段：

> 还没有进入 Release。

---

# 13. Impact Analysis

CodeX / Canon Systems Editor 负责。

必须回答六类影响：

```text
CONTENT
SCHEMA
ENGINEERING
STATE
EPISODE
QA
```

---

# 14. Content Impact

检查：

- World Canon；
- Character Canon；
- Map；
- Prehistory；
- Reference；
- Production Spec。

输出：

```text
Affected Sources
Conflicts
Required Source Updates
```

---

# 15. Schema Impact

检查：

- Story Schema；
- Episode Delta；
- Registry；
- Runtime；
- State Store。

输出：

```text
Backward compatible?
Deprecated fields?
Migration required?
```

---

# 16. Engineering Impact

检查：

- ContextAssembler；
- Validator；
- StateWriter；
- Prompt；
- RuntimeAdapter；
- QA；
- CI。

---

# 17. State Impact

检查：

- Route Continuity；
- Relationship State；
- Discovery State；
- Artifact State；
- Ledger。

特别回答：

> 已有用户数据怎么办？

---

# 18. Episode Impact

输出：

```text
Affected Released Episodes
Affected Candidate Episodes
Regeneration Required?
Manual Review Required?
```

不能默认：

> 一改Canon就重生成所有Episode。

---

# 19. QA Impact

检查是否需要：

- 新Golden Test；
- 修改旧Golden Expected；
- 增加Regression；
- 增加Validator。

---

# 20. Impact Level

推荐统一：

```text
NONE
LOW
MEDIUM
HIGH
CRITICAL
```

---

# 21. CHANGESET

Impact Analysis完成后生成：

```yaml
changeset:
  change_id:
  release_id:

  change_class:

  changed_sources: []

  added: []
  changed: []
  deprecated: []
  removed: []

  impact:
    content:
    schema:
    engineering:
    state:
    episodes:
    qa:

  migration:
    required:
    migration_type:

  regeneration:
    required:
    scope:

  affected_episode_ids: []

  required_issues: []

  approval_required:
```

---

# 22. Migration Type

统一枚举：

```text
NONE
FORWARD_COMPAT
DATA_BACKFILL
STATE_MIGRATION
SCHEMA_MIGRATION
CONTENT_REGENERATION
CANON_RECONCILIATION
FULL_BREAKING_MIGRATION
```

---

# 23. FORWARD_COMPAT

新增可选字段。

旧数据仍合法。

例如：

```text
character_state 新增 optional note
```

只需：

> 新代码支持。

---

# 24. DATA_BACKFILL

旧记录缺少新必需值，但可以推导。

例如：

> 旧 Episode 缺 `release_id`

可以根据发布时间补回。

---

# 25. STATE_MIGRATION

用户已有状态结构改变。

必须：

- migration script；
- backup；
- dry run；
- validation；
- rollback plan。

---

# 26. SCHEMA_MIGRATION

例如：

```text
canon_scope
↓
persistence_policy
```

需要：

- parser更新；
- validator更新；
- fixture更新；
- prompt更新；
- deprecated detection。

---

# 27. CONTENT_REGENERATION

只有当旧成品本身：

> 已违反新规则或内容错误

才要求重生成。

不能因为：

> 文档升级

就自动重生成全部117课。

---

# 28. CANON_RECONCILIATION

用于：

> 正史发生Retcon，但旧Episode已公开。

需要决定：

- 保留历史版本；
- 修改Episode；
- 用剧情解释；
- 标记Deprecated；
- 迁移State。

必须人工参与。

---

# 29. Compatibility Policy

建议每个 Schema 声明：

```yaml
compatibility:
  accepts_previous_minor:
    true

  deprecated_fields: []

  hard_removed_fields: []
```

---

# 30. Deprecated 生命周期

字段废弃建议：

```text
ACTIVE
→ DEPRECATED
→ ERROR_ON_NEW_CONTENT
→ REMOVED
```

不要：

> 一宣布废弃，旧数据当场全部失效。

但重大早期系统改造可：

> 明确一次性迁移。

---

# 31. Release Candidate

CHANGESET批准以后建立：

```text
GALAXY-CONTENT-xxxx-Rx-RC1
```

RC包含：

- Manifest；
- Sources；
- Schemas；
- Changelog；
- Migrations；
- Test expectations。

---

# 32. GitHub Issue 生成规则

一个 CHANGESET 不等于一个 Issue。

CodeX应按：

> **可独立开发、可独立验收、可独立测试**

拆 Issue。

---

# 33. Issue 分类标签

建议：

```text
galaxy/canon
galaxy/schema
galaxy/context
galaxy/state
galaxy/migration
galaxy/qa
galaxy/runtime
galaxy/content
```

优先级：

```text
P0
P1
P2
```

变更类型：

```text
breaking-change
migration
regression
deprecation
```

---

# 34. Issue Template

统一：

```markdown
# Background

# Source Specs

# Change

# Current Behavior

# Required Behavior

# Data / Schema Impact

# Migration Plan

# Acceptance Criteria

# Tests

# Dependencies

# Rollback

# Out of Scope
```

---

# 35. Acceptance Criteria

必须：

> 可测试。

禁止：

> “优化连续性”。

正确：

```text
Given EP002 starts after EP001
When context is assembled
Then TOKI relationship state equals EARLY_ACQUAINTANCE
```

---

# 36. PR Policy

任何涉及：

```text
Schema
Migration
Canon Registry
State Model
```

的 PR：

必须引用：

```text
change_id
release_id
issue_id
```

---

# 37. CI Required Checks

建议：

```text
Schema Validation
Registry ID Validation
Broken Reference
Deprecated Field
Migration Test
Golden Episode Regression
Canon Mutation Test
Epistemic Test
```

---

# 38. Release Approval

至少两个维度：

## Content / Canon Approval

确认：

> 内容含义正确。

## Engineering Approval

确认：

> 系统迁移、安全和测试正确。

重大 C0/C1 变更：

> 必须 Canon Owner批准。

---

# 39. Release

正式Release内容：

```text
Release Manifest
CHANGESET
Migration Notes
Compatibility Notes
Known Issues
Updated Sources
```

---

# 40. Post-release Verification

发布后检查：

```text
ContextAssembler
Golden Samples
State Migration
New Episode Production
Old Episode Load
```

发现异常：

> 允许Rollback。

---

# 41. Rollback

每个 Migration Issue 应回答：

> 如果失败如何退回？

至少：

```text
Previous Release ID
Previous Schema
Backup State
Rollback Script / Procedure
```

---

# 42. Canon Retcon 独立处理

Retcon 不能只是 CHANGESET 一行。

需要：

```yaml
retcon_request:
  canon_id:
  old_value:
  new_value:
  reason:
  affected_episodes:
  affected_states:
  migration_required:
  approved_by:
```

---

# 43. Reference 过期检查

Reference Asset 应支持：

```text
freshness_policy
last_verified
review_due
```

达到 review_due：

> 不是自动判错。

而是：

```text
REVIEW_REQUIRED
```

---

# 44. Release Diff

CodeX 应能够输出：

```text
R1 → R2

Changed Canon: 3
Changed Characters: 1
Changed Schema: 2
Deprecated Fields: 1
Affected Episodes: 4
State Migration: YES
Regeneration: NO
```

---

# 45. 以后内容团队交付给研发的最小包

每次不再发十几份全量文档。

正常增量发布只需要：

```text
Release Manifest
CHANGESET
Changed Source Files
Migration Notes（如有）
Updated Golden Expected（如有）
```

---

# 46. 全量 Baseline Release

只有：

- 第一次交付；
- 大版本；
- Major Breaking Change；

才重新给：

> 全量 Baseline Archive。

---

# 47. CodeX 自动化目标

理想情况下，用户只需要说：

> “Character Canon v0.2更新完成，请准备Galaxy Release。”

CodeX自动：

1. diff；
2. classify；
3. impact；
4. generate CHANGESET draft；
5. generate Issues；
6. identify tests；
7. wait approval。

---

# 48. 不允许 CodeX 自动做的事

CodeX不能自动：

- 批准C0/C1 Canon；
- 决定角色重大人格变化；
- 接受Retcon；
- 重生成所有Episode；
- 修改课程目标。

它负责：

> 分析与执行。

不是：

> Canon Owner。

---

# 49. 第一次交接后的推荐 Release

建议当前第一次正式交付定义为：

```text
GALAXY-CONTENT-2026.09-R1
```

状态：

> BASELINE CANDIDATE

待李光 / CodeX完成 Repository Audit后：

> 再决定正式APPROVED。

---

# 50. 当前 Baseline 包建议组成

```text
World Canon v1.0
Character Pack v0.1
Prehistory v0.1
Map Canon v0.1
Reference Library v0.1
Story Schema v0.3
Narrative Production Spec v0.2
Engineering Handoff Pack v0.1
EP001 Golden Sample
Course Source v5.0.7
Update / Release / Migration Protocol v0.1
```

---

# 51. Protocol Definition of Done

一次 Update 流程完成必须满足：

```text
Change identified
+
Classified
+
Impact analyzed
+
CHANGESET generated
+
Issues created
+
Migration executed if needed
+
Tests pass
+
Approvals complete
+
Release manifest updated
+
Post-release verification pass
```

---

# 52. 最终原则

以后银河内容系统不能再依赖：

> “大家记得我们上次改过那个设定。”

它必须能够回答：

> **哪一版？谁批准？影响哪些故事？旧数据怎么办？哪个Issue实现？哪个测试证明它没有弄坏以前的世界？**

只有做到这一点：

> 银河才真正从一次性的AI内容项目，进入可以多年持续演进的内容工程系统。
