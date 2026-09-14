# GALAXY M0 Decisions

> 状态：`FINAL DRAFT`  
> 决策日期：2026-09-11  
> 适用范围：Galaxy Story System Baseline v0.3 的 Repository Audit 后 P0 规划。  
> 非适用范围：不修改 Canon、课程目标、角色文案、历史课程或生产代码。

## 1. EP001 Golden 的正式合同

| 字段 | 正式值 |
|---|---|
| `episode_id` | `GALAXY-001` |
| `course_id` | `COURSE-001` |
| `location_id` | `L01` |
| `primary_character_id` | `TOKI` |
| 初始 Relationship State | `TOKI=NOT_MET` |
| 初始 Discovery State | `L01=UNKNOWN` |
| Expected Relationship Delta | `TOKI: NOT_MET -> EARLY_ACQUAINTANCE` |
| Expected Discovery Delta | `L01: UNKNOWN -> VISITED` |
| Expected Route Event | `USER_FIRST_ARRIVAL` |
| Expected Personal Artifact | `ROUTE_LOG` |
| Expected Shared Canon Delta | `none` |

EP001 是第一份 Golden Sample。它用于验证工程合同，不代表对世界观正文、课程内容或角色文学表现作自动修改。

## 2. 职责边界

### ContextAssembler

只读取和选择当前有效的 Course、相关 Canon、Location、Character Eligibility、Relationship State、Discovery State、Route Continuity、受控参考和 Runtime Budget。

它**不产生** `TOKI: NOT_MET -> EARLY_ACQUAINTANCE`。在 EP001 开始时，TOKI 的输入状态仍是 `NOT_MET`；关系变化只属于本集产生并经批准后的 Delta。

### DeltaExtractor

只输出 `Detected Delta`：从已通过内容/交互结果中提取候选变化。它没有持久化权限，也不能让模型自由推断的变化直接进入用户状态。

### DeltaValidator / Approval Gate

将 `Detected Delta` 与 EP001 的 `Expected Delta Scope` 和 `Allowed Delta Scope` 对照。只有字段、对象、前后状态和事件均获允许时，才形成 `Approved Delta`。

### StateWriter

只持久化 `Approved Delta`。P0 使用 file-backed State Adapter 支撑 Golden / Factory dry run；真实 APP/RunS 统一身份状态接入保持 P1，但 P0 必须先定义稳定的 adapter contract。

```text
ContextAssembler (read only)
  -> Narrative / interaction result
  -> DeltaExtractor (Detected Delta)
  -> DeltaValidator (Expected/Allowed scope)
  -> StateWriter (Approved Delta only)
```

## 3. Story Schema 字段纪律

Schema 严格使用 v0.3 正式字段，例如：

```json
{
  "persistence_policy": {
    "shared_canon_write": {
      "allowed": false
    }
  }
}
```

不得为便利新增简写字段，如 `shared_canon=false`。新输出不得使用废弃字段 `canon_scope`、`Route Canon`、`canon_writeback`。

## 4. Registry 与角色范围

- P0 建立所有主角色的稳定 ID；
- 首轮只要求 `TOKI` 具备 EP001 所需的完整运行资料：Eligibility、知识权限、视觉/TTS 引用和角色状态读取；
- 其余主角色可保留最小 stub（稳定 ID、基础状态、未引入）；
- 不因 Registry 建立而要求其他伙伴首轮出场，也不扩大同屏角色数。

## 5. 状态与分支范围

- 通用 `Branch Memory` 保持 P1；
- EP001 的 `first_action` 只作为 Route Continuity 内的轻量 choice state；
- 不为 EP001 建设开放式多分支、库存、道具、经济或兑换系统；
- `ROUTE_LOG` 是个人 artifact，不是 Shared Canon，也不是产品积分或可兑换权益。

## 6. 迁移与证据纪律

任何迁移前，必须备份并分类 `factory/courses/`：

| 分类 | 处理规则 |
|---|---|
| `legacy-v2` | 保持历史 profile，可重放，不自动升级。 |
| `baseline-v0.3-candidate` | 可进入 P0 adapter/Golden 验证，但未通过前不得宣称 v0.3 release。 |
| `review-required` | 来源、版本、profile 或状态不清，冻结并人工审阅。 |

历史缺失版本不得补造、不得从标题或课号推断为“已知版本”。新发布必须通过 Release Manifest 留下可追溯证据。

## 7. P0-06 测试门槛

M3 的硬要求是：

1. 一条可在干净环境重复执行的测试命令；
2. EP001 Golden regression；
3. legacy replay 的最低兼容验证。

GitHub Actions 可以在低成本时一并加入，但不是 M3 的架构阻塞条件。

## 8. 仍待李光确认的 P1 边界

- APP/RunS 统一身份状态的真实责任仓库、接口负责人及数据归属；
- State Adapter 的读取、批准写入、失败重试与离线降级契约；
- 现有 `factory/courses/` 在产内容的备份位置、分类责任人和迁移窗口。

