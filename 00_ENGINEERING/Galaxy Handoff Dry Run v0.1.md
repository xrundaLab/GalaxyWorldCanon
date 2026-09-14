# Galaxy Handoff Dry Run v0.1
## 用一次真实 Schema 变更模拟完整交接与迁移

**Dry Run ID：DRYRUN-001**  
**日期：2026-09-11**  
**目标：验证内容更新 → Impact Analysis → CHANGESET → GitHub Issues → Migration → Regression → Release 的整条链路**

---

# 0. 为什么选择这个变更

本次 Dry Run 不虚构一个新需求。

直接使用项目中已经真实发生过的变更：

旧设计：

```text
Route Canon
canon_scope
canon_writeback
```

新设计：

```text
Shared Canon
Route Continuity
Personal Relationship State
Personal Discovery State
Personal Artifact State
persistence_policy
shared_canon_proposal
```

这个变更具有代表性，因为它同时影响：

- Story Schema；
- Episode Delta；
- 用户状态；
- Prompt；
- Validator；
- Golden Sample；
- 已有Pilot文本术语。

因此适合测试：

> SCHEMA_CHANGE + STATE_MODEL_CHANGE。

---

# 1. Change Proposal

```yaml
change_proposal:
  change_id: GAL-CHG-0001

  title:
    Replace Route Canon with Route Continuity / Personal State

  author:
    Content / Canon Team

  reason:
    Route Canon容易暗示每位用户拥有独立正史，
    与One Canon原则冲突。
    用户个性化应属于连续性与个人状态，而不是新的Canon。

  proposed_change_class:
    SCHEMA_CHANGE

  affected_entities:
    - Story Schema
    - Episode Delta
    - ContextAssembler
    - State Store
    - EP001 Golden Sample
    - Validators
    - Prompts

  urgency:
    P0
```

---

# 2. Change Classification

最终：

```text
Primary: SCHEMA_CHANGE
Secondary: STATE_MODEL_CHANGE
Breaking: YES
Canon Breaking: NO
```

为什么：

> Shared Canon本身没有改变。

改变的是：

> 个性化状态的建模方式。

---

# 3. Impact Analysis

## Content

影响：

- Story Schema v0.2；
- Canonical Pilot文档；
- EP001早期Profile；
- 部分策划术语。

不影响：

- World Canon世界事实；
- Character人格；
- Prehistory；
- Map Canon。

Impact：

> MEDIUM

---

# 4. Schema Impact

旧：

```yaml
canon_scope:
  ROUTE
```

或：

```yaml
canon_writeback: false
```

新：

```yaml
persistence_policy:
  shared_canon_write:
    allowed: false
  route_continuity_write:
    allowed: true
  personal_state_write:
    allowed: true
  shared_canon_proposal:
    allowed: false
```

Impact：

> HIGH

Backward Compatible：

> NO without adapter

---

# 5. State Impact

旧系统如果已有：

```text
route_canon_events
```

需要拆分：

```text
route_continuity
relationship_state
discovery_state
artifact_state
```

如果当前尚未大量上线：

> 可以进行一次早期结构迁移。

Impact：

> HIGH

---

# 6. Engineering Impact

受影响模块：

```text
StoryProfile parser
ContextAssembler
StateWriter
DeltaExtractor
Validator
Prompt templates
Episode fixtures
Golden tests
CI deprecated field check
```

Impact：

> HIGH

---

# 7. Episode Impact

可能受影响：

```text
EP001
EP002
EP003
Pilot samples
```

但：

> 不一定需要重写Reader正文。

主要需要更新：

- machine-readable profile；
- delta；
-内部术语。

Regeneration：

> NO

Manual Review：

> YES

---

# 8. QA Impact

必须新增：

```text
Deprecated Field Test
Personal State Separation Test
Shared Canon Write Protection Test
EP001 Regression
```

---

# 9. CHANGESET

```yaml
changeset:
  change_id: GAL-CHG-0001
  release_id: GALAXY-CONTENT-2026.09-R1

  change_class:
    SCHEMA_CHANGE

  deprecated:
    - Route Canon
    - canon_scope
    - canon_writeback

  added:
    - persistence_policy
    - route_continuity
    - relationship_state
    - discovery_state
    - artifact_state
    - shared_canon_proposal

  impact:
    content: MEDIUM
    schema: HIGH
    engineering: HIGH
    state: HIGH
    episodes: MEDIUM
    qa: HIGH

  migration:
    required: true
    migration_type:
      - SCHEMA_MIGRATION
      - STATE_MIGRATION

  regeneration:
    required: false

  affected_episode_ids:
    - GALAXY-001
    - GALAXY-002
    - GALAXY-003

  approval_required:
    canon_owner: true
    engineering_owner: true
```

---

# 10. CodeX 应生成的 EPIC

# EPIC｜Migrate Personal Narrative Persistence Model

**Goal**

将旧的：

> Route Canon

迁移为：

> One Shared Canon + Route Continuity + Personal State。

---

# 11. ISSUE 1
## Implement Story Schema v0.3 persistence_policy

### Background

Story Schema v0.2 使用：

```text
canon_scope / canon_writeback
```

新Schema改为：

```text
persistence_policy
```

### Required Behavior

支持：

```yaml
shared_canon_write
route_continuity_write
personal_state_write
simulation_state
shared_canon_proposal
```

### Acceptance Criteria

- [ ] v0.3 Profile可通过Schema校验
- [ ] 新内容出现 `canon_scope` → ERROR_ON_NEW_CONTENT
- [ ] 新内容出现 `canon_writeback` → ERROR_ON_NEW_CONTENT
- [ ] EP001 v0.3 fixture → PASS
- [ ] Shared Canon默认不可写

### Tests

```text
schema_valid_ep001
deprecated_canon_scope
deprecated_canon_writeback
shared_write_default_false
```

Priority：

> P0

---

# 12. ISSUE 2
## Split legacy route state into continuity and personal state

### Required State

```text
Route Continuity
Personal Relationship State
Personal Discovery State
Personal Artifact State
```

### Acceptance Criteria

- [ ] 旧 `route_canon_events` 可迁移到 `route_continuity`
- [ ] 角色关系数据进入Relationship State
- [ ] 地图发现进入Discovery State
- [ ] 用户作品进入Artifact State
- [ ] 不产生新的Shared Canon记录
- [ ] Migration可重复执行且幂等

Priority：

> P0

---

# 13. ISSUE 3
## Update ContextAssembler for new personal state model

### Required Behavior

输入：

```text
route_id
```

输出 Context 中分别包含：

```text
route_continuity
relationship_state
discovery_state
artifact_state（按需）
```

### Acceptance Criteria

Given：

```text
TOKI relationship = EARLY_ACQUAINTANCE
L01 discovery = VISITED
```

When：

> assemble EP002 context

Then：

```text
TOKI != NOT_MET
L01 = VISITED
Shared Canon unchanged
```

Priority：

> P0

---

# 14. ISSUE 4
## Update Episode Delta and StateWriter

旧：

```text
route_canon_delta
```

新：

```text
route_continuity_delta
relationship_delta
discovery_delta
artifact_delta
shared_canon_proposal
```

### Acceptance Criteria

EP001 output：

```text
TOKI NOT_MET → EARLY_ACQUAINTANCE
L01 UNKNOWN → VISITED
ROUTE_LOG created
```

必须写到三个不同State域。

Shared Canon：

> 0 changes

Priority：

> P0

---

# 15. ISSUE 5
## Add deprecated field CI check

CI扫描：

```text
canon_scope
canon_writeback
Route Canon
route_canon_delta
```

范围：

- Schema fixtures；
- Prompts；
- Episode Profiles；
- Config。

### Acceptance Criteria

新PR中出现旧字段：

> CI FAIL

历史Archive：

> 可排除。

Priority：

> P0

---

# 16. ISSUE 6
## Update EP001 Golden Sample

更新：

```text
story_profile.yaml
episode_delta.yaml
validation_expected.yaml
```

Reader正文：

> 无需因本次迁移重写。

### Acceptance Criteria

EP001：

- Schema PASS
- Canon PASS
- TOKI state正确
- Shared Canon无写入
- Delta正确分域

Priority：

> P0

---

# 17. ISSUE 7
## Regression: EP001 → EP002 continuity

### Test

EP001完成后：

```text
TOKI = EARLY_ACQUAINTANCE
```

启动EP002：

ContextAssembler必须读取：

```text
TOKI = EARLY_ACQUAINTANCE
```

不得：

```text
TOKI = NOT_MET
```

### Acceptance Criteria

- [ ] EP001写State
- [ ] EP002读State
- [ ] relationship continuity PASS
- [ ] no shared canon mutation

Priority：

> P0

---

# 18. ISSUE 8
## Shared Canon mutation guard

验证：

Generator输出：

> “用户与拓奇初遇”

不得写入：

> Shared Timeline。

### Acceptance Criteria

- [ ] relationship写Personal Relationship State
- [ ] user event写Route Continuity
- [ ] Shared Canon remains unchanged
- [ ] 未授权写入返回 `SHARED_CANON_WRITE_DENIED`

Priority：

> P0

---

# 19. Migration Plan

## Phase A｜Backup

保存：

```text
legacy route state
legacy profiles
legacy deltas
```

---

## Phase B｜Schema Adapter

临时允许读取旧数据：

```text
canon_scope: ROUTE
```

映射：

```text
route_continuity_write = true
personal_state_write = true
shared_canon_write = false
```

仅用于：

> Migration Reader。

新内容：

> 禁止继续写旧格式。

---

# 20. Phase C｜Data Migration

伪逻辑：

```text
for each legacy_route_event:
    if type == relationship:
        → relationship_state
    elif type == discovery:
        → discovery_state
    elif type == artifact:
        → artifact_state
    else:
        → route_continuity
```

无法分类：

```text
MIGRATION_REVIEW_REQUIRED
```

不得：

> 猜。

---

# 21. Phase D｜Validate Counts

迁移前：

```text
legacy events = N
```

迁移后：

```text
route
+ relationship
+ discovery
+ artifact
+ review_required
= N
```

防止：

> 静默丢数据。

---

# 22. Phase E｜Golden Regression

运行：

```text
EP001
EP002
EP003
Prototype Pilot
```

必须全部通过：

- Schema；
- Continuity；
- Canon；
- Character；
- Delta。

---

# 23. Phase F｜Disable Legacy Write

部署以后：

```text
legacy read: temporary
legacy write: disabled
```

---

# 24. Phase G｜Remove Adapter

确认无旧数据以后：

> 下一Minor/Major版本删除旧Reader。

---

# 25. Rollback Plan

如果新State Writer错误：

1. 停止新写入；
2. 恢复旧State snapshot；
3. 恢复旧Runtime Release；
4. 保留失败迁移日志；
5. 修复后重新Dry Run。

不能：

> 手工覆盖线上数据。

---

# 26. Regression Matrix

| Test | Expected |
|---|---|
| EP001首次见拓奇 | Relationship State写入 |
| L01首次访问 | Discovery State写入 |
| Route Log | Artifact State写入 |
| EP002读取拓奇关系 | EARLY_ACQUAINTANCE |
| 用户初遇事件 | Route Continuity |
| Shared Canon | 无变化 |
| `canon_scope`新内容 | FAIL |
| 角色知识泄漏 | BLOCK |
| 生成新银河大战历史 | BLOCK |

---

# 27. Release Candidate

Dry Run成功后候选：

```text
GALAXY-CONTENT-2026.09-R1-RC1
```

Manifest：

```yaml
story_schema_version: "0.3"
production_spec_version: "0.2"
```

---

# 28. Dry Run 判定标准

## PASS 条件

1. CHANGESET完整；
2. Issues可直接开发；
3. Acceptance Criteria可测试；
4. Migration有rollback；
5. EP001 Golden通过；
6. EP002能继承EP001状态；
7. Shared Canon没有用户级污染；
8. Deprecated field可被CI阻止。

---

# 29. 本次 Dry Run 结果

从架构设计角度：

> **PASS CANDIDATE**

原因：

整个变化可以被清楚拆成：

```text
Schema
State
Context
Writer
CI
Regression
```

没有需要开发人员重新阅读完整World Canon才能理解的模糊任务。

---

# 30. 这证明了什么

如果以后内容团队说：

> “我们更新了爱今的角色历史。”

流程不会是：

> 把新Character Pack扔到群里。

而是：

```text
Character Canon Diff
↓
CONTENT_UPDATE
↓
Impact Analysis
↓
Affected Episodes / Context Rules
↓
CHANGESET
↓
Issue / No Issue
↓
Regression
↓
Release
```

如果以后说：

> “Story Schema新增一种长期线程状态。”

同样会进入：

> SCHEMA_CHANGE。

---

# 31. 给 CodeX 的实际指令模板

未来可以直接对 CodeX 说：

> 读取当前 Galaxy Baseline Release 和新的内容更新文件。  
> 按《Galaxy Update / Release / Migration Protocol》执行：
> 1. 生成diff；
> 2. 对每项变更分类；
> 3. 做Impact Analysis；
> 4. 生成CHANGESET；
> 5. 生成GitHub EPIC / ISSUE草案；
> 6. 写Acceptance Criteria与Tests；
> 7. 标记Migration与Regression要求；
> 8. 不直接修改C0/C1 Canon，不直接大规模重构代码，等待人工批准。

这应该成为：

> **未来长期更新的标准入口。**

---

# 32. Dry Run 最终结论

本次真实Schema迁移证明：

> 当前 Handoff Pack 已经不仅能描述“要建设什么”，也开始能够描述“以后变化时怎么继续建设”。

因此交接体系已具备：

- Baseline；
- Version；
- Diff；
- Impact；
- Issue；
- Migration；
- Regression；
- Release；

完整闭环。

剩余真正需要发生的：

> 不再是继续设计协议。

而是：

> **让 CodeX 对真实 GitHub 仓库执行第一次 Repository Audit，并据此生成真正的工程 Issues。**
