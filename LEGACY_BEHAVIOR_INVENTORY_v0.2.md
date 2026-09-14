# Legacy Behavior Inventory v0.2

> 状态：`FINAL DRAFT`  
> 相对 v0.1 的实质更新：补充了 `factory/courses/` 迁移前备份分类规则；其余 Legacy 判断保持不变。  
> `DEPRECATE`：不再作为新版默认，但可由 legacy profile 重放。`REMOVE`：不得继续保留为默认可执行行为。

| ID | File / Module | Location | Current Behavior | Classification | Recommendation | v0.2 Migration Rule |
|---|---|---|---|---|---|---|
| L-01 | `factory/world_bible.py` | `MAIN_QUEST`、`resolve_context()` | 默认飞船受损、收集星粒修复。 | Canon / Legacy | CONFIGURE | 抽为 `legacy-v2`；`baseline-v0.3` 默认不得注入。 |
| L-02 | `factory/studio_server.py` | `UNDERSTAND_SYS`、`llm_understand()` | Prompt 重复要求修理、航行阻碍和修复收束。 | Production / Legacy | DEPRECATE | baseline 必须改由 Story Profile/ContextAssembler 输入。 |
| L-03 | `factory/world_bible.py` | `STAGES`、`normalize_understanding()` | 固定星粒、能量、航行日志/成长资产。 | Production / Legacy | CONFIGURE | 奖励由单集 scope 声明；`ROUTE_LOG` 是 personal artifact，不是积分。 |
| L-04 | `factory/world_bible.py` | `COMPANION_PERSONA` | TOKI 被定义为呆萌憨傻、频繁迷糊。 | Canon / Legacy | REMOVE | 不得进入 baseline Prompt；角色内容以 Character Canon 为准。 |
| L-05 | `world_bible.py` / Prompt | `MAIN_QUEST`、`lesson_loop` | 默认危机、报警、能源不足或修理任务开场。 | Production / Legacy | CONFIGURE | 迁入 ForbiddenTropeRegistry；只有 profile 显式允许才可用。 |
| L-06 | `factory/world_bible.py` | `STAGES` | 课号区间自动轮换四星域。 | Canon / Production | CONFIGURE | 使用 Course/Story Profile 显式 `location_id/route_id`，废弃编号推断。 |
| L-07 | Factory 审计/视觉/TTS | companion 限制 | TOKI 是唯一可用伙伴，第三角色常被拒绝。 | Runtime / Production / Legacy | CONFIGURE | 保留同屏 1-2 主对话角色限制；P0 为其余主角色建 stable-ID stub。 |
| L-08 | `director_blockers()`、`AVATAR_ROLES` | role 约束 | player/companion/narrator 是主要允许角色。 | Production / Runtime | CONFIGURE | 升级为 character ID + eligibility；P0 只实现 TOKI 完整运行资料。 |
| L-09 | `COMPANION_PERSONA` | 知识权限 | TOKI 似乎可自然解释全部资料与世界设定。 | Canon / Legacy | REMOVE | 由 `epistemic_permission` 校验；未授权事实不得进入对话。 |
| L-10 | `normalize_understanding()` | 知识包装 | 课程知识自动套为固定银河奖励/修船包装。 | Production / Legacy | CONFIGURE | `learning_connection` 必须由 Story Profile 显式声明，可弱连接或并置。 |
| L-11 | `deck_template.html` | choice state | 选择/重试只在单次浏览器会话有效。 | Runtime / Legacy | DEPRECATE | P0 提供 file-backed adapter；真实 APP writer 为 P1。 |
| L-12 | `factory/courses/` | `.gitignore` | 课程生产源、`meta.json` 不进 Git。 | Production | CONFIGURE | 改造前先备份并分类为 `legacy-v2/baseline-v0.3-candidate/review-required`；历史版本不得伪造。 |
| L-13 | `image_prompt_suffix()` | visual guardrail | 非拓奇的伙伴可能被禁止出图。 | Production / Legacy | CONFIGURE | 由 Character/Asset Registry 控制，保留防串形能力。 |
| L-14 | Factory / static Deck | continuity | 不存在跨课 Relationship、Discovery、Route 状态写入。 | Production / Runtime | DEPRECATE | `Detected Delta -> Approved Delta -> StateWriter`；P0 只写 approved。 |
| L-15 | `FORBIDDEN_STORY_PATTERNS` | rule list | 有零散禁止项，未覆盖新版 trope。 | Production | CONFIGURE | 迁为 ID 化 Registry，合并 global/profile/episode 范围。 |
| L-16 | 历史资料 / 忽略工作区 | `Route Canon`、`canon_scope` | 当前 checkout 未发现可执行字段。 | Legacy | UNKNOWN | 仅在备份产物中查证后再迁移；新 profile 一律禁止这些废弃字段。 |

## P0 Guardrails

1. baseline 流程不得隐式调用 Legacy 默认；profile 必须在 Release Manifest 中显式记录。
2. `ContextAssembler` 读取 `TOKI=NOT_MET`，但绝不写出 `EARLY_ACQUAINTANCE`。
3. `DeltaExtractor` 只能生成 Detected Delta；没有通过 Expected/Allowed Delta Scope 的变化不得写入 file-backed adapter 或未来 APP adapter。
4. `persistence_policy.shared_canon_write.allowed=false` 时，任何 shared Canon 写入均应失败。
5. 旧的 production meta、航行日志展示或浏览器选择，均不得被误判为真实已持久化学习状态。

