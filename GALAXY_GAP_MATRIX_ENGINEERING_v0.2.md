# GALAXY Engineering Gap Matrix v0.2

> 状态：`FINAL DRAFT`  
> 状态含义：`EXISTING` 可直接复用；`PARTIAL` 有相邻能力；`MISSING` 未见实现；`CONFLICT` 现有默认违反新版基线；`NOT_NEEDED_NOW` 不进入 P0。

| Baseline requirement | Observed evidence | Status | v0.2 minimum implementation | Priority |
|---|---|---|---|---|
| Release Manifest | 本地 `meta.json` 被忽略；静态 Deck 无完整版本快照。 | MISSING | 冻结 `release_id/course_id/episode_id/course_version/canon_version/schema_version/prompt_version/model_version/world_profile_id` 与 hashes。 | P0 |
| Stable IDs | 当前使用 slug、标题、课号。 | PARTIAL | `COURSE-001/GALAXY-001/L01/TOKI` 等 immutable IDs。 | P0 |
| CourseRegistry | outline/slug 已有。 | PARTIAL | course manifest 包装现有输入，显式 episode/profile/source version。 | P0 |
| CanonRegistry | `world_bible.py` 混合事实、Prompt 和策略。 | CONFLICT | 只读 snapshot，运行时不解析/塞入全量 Markdown。 | P0 |
| CharacterRegistry | TOKI hardcode；其他伙伴无工程实体。 | CONFLICT | 所有主角色 stable ID；P0 仅 TOKI 完整运行资料，其余 stub。 | P0 |
| LocationRegistry | 星域由课号推断。 | PARTIAL | `L01` + route/location 最小事实与可达关系。 | P0 |
| ReferenceRegistry | 仅官方 IP URL/图片批次。 | MISSING | 先定义 `reference_id/version/purpose` 合同。 | P1 |
| ForbiddenTropeRegistry | 有零散 `FORBIDDEN_STORY_PATTERNS`。 | PARTIAL | ID 化 global/profile/episode 规则；封禁 legacy 默认危机。 | P0 |
| ContextAssembler | `resolve_context()` 根据课号制造固定 context。 | CONFLICT | 只读装配 Course + relevant Canon + eligibility + current relationship/discovery/route + references + runtime。 | P0 |
| Relationship state read | 无用户状态模型。 | MISSING | 输入可读 `TOKI=NOT_MET`；Assembler 不写关系变化。 | P0 |
| Character Eligibility | companion 固定 TOKI，第三角色常被阻断。 | CONFLICT | 按 introduced/location/route/runtime slot 判断；EP001 TOKI 适格。 | P0 |
| Epistemic Permission | TOKI 近乎全知；无角色知识合同。 | CONFLICT | 角色可知事实与用户已发现事实双重许可。 | P0 |
| Story Profile v0.3 | `understanding.json` 相近但未含正式字段/版本。 | PARTIAL | `story_profile.json` + adapter，严格使用 `persistence_policy.shared_canon_write.allowed`。 | P0 |
| StoryProfileValidator | 已有页面/术语/角色审计。 | PARTIAL | schema、废弃字段、profile、trope、eligibility 验证。 | P0 |
| `persistence_policy` | 未见正式字段。 | MISSING | 实现 v0.3 字段原样合同；禁止 `shared_canon=false` 等简写。 | P0 |
| Route Continuity | 只有课号阶段推断；无用户路线状态。 | MISSING | 记录 `USER_FIRST_ARRIVAL` 和轻量 `first_action` choice state。 | P0 |
| Personal relationship | 无跨课关系存储。 | MISSING | Approved Delta 写入 TOKI `NOT_MET -> EARLY_ACQUAINTANCE`。 | P0 |
| Personal discovery | 无发现存储。 | MISSING | Approved Delta 写入 L01 `UNKNOWN -> VISITED`。 | P0 |
| Personal artifact | 无正式合同；有旧航行日志文案。 | PARTIAL | `ROUTE_LOG` 作为 EP001 personal artifact；不映射积分/兑换。 | P0 |
| General Branch Memory | Deck 仅会话内选择。 | MISSING | 不独立建设；EP001 first_action 归入 route continuity。 | P1 |
| Episode Delta | 无 state diff。 | MISSING | `Detected Delta` 与 `Approved Delta` 分离，禁止完整 Canon snapshot。 | P0 |
| DeltaExtractor | 无实现。 | MISSING | 只提取 detected candidates，不持久化。 | P0 |
| Delta approval gate | 无实现。 | MISSING | 对照 Expected/Allowed Delta Scope，拒绝模型自由推断越界变化。 | P0 |
| StateWriter | 无状态回写；`meta.json` 不是用户状态。 | MISSING | writer interface + file-backed adapter；只写 Approved Delta。 | P0 |
| APP/统一身份 adapter | 当前仓库未见。 | MISSING | 定义 interface，真实接入保留 P1。 | P1 |
| Shared Canon proposal | 不存在。 | MISSING | 本集明确 none；禁止写入。 | NOT_NEEDED_NOW |
| 静态 Deck 分支/重试 | `deck_template.html` 已支持。 | EXISTING | 作为 renderer 保留，追加事件/adapter 边界。 | P0 |
| 视觉/TTS 生产 | 图片、IP URL、TTS 流水线已存在。 | EXISTING | P0 仅将 TOKI ID 映射到现有插槽。 | P0 |
| 视觉参考可追溯 | 无 reference ID、许可/版本记录。 | PARTIAL | Release Manifest/asset metadata 先留字段。 | P1 |
| Golden EP001 | 有文档 Reader，未见机器 fixture。 | PARTIAL | fixture 校验 M0 输入、Expected Delta、shared Canon none、废弃字段无泄漏。 | P0 |
| EP002/EP003 regression | 有 Pilot 文档，未见工程测试。 | MISSING | P0 后扩展多集回归。 | P1 |
| 单一可重复测试命令 | 有 unittest；当前运行受依赖/cwd 影响。 | PARTIAL | 固定环境与单一命令；这是 M3 硬要求。 | P0 |
| GitHub Actions | 未发现 workflows。 | MISSING | 低成本可选增强，不阻塞 M3。 | P1 / optional |
| 发布迁移/回滚 | 未见 changeset/manifest migration 实现。 | MISSING | 先导出 manifest、changeset 和分类备份。 | P1 |
| `factory/courses/` 迁移安全 | 被 `.gitignore` 排除。 | CONFLICT | 迁移前备份并分类为 `legacy-v2/baseline-v0.3-candidate/review-required`。 | P0 |

## EP001 Golden Assertions

```text
Given:  course_id=COURSE-001, episode_id=GALAXY-001,
        location_id=L01, primary_character_id=TOKI,
        relationship[TOKI]=NOT_MET, discovery[L01]=UNKNOWN

When:   ContextAssembler runs
Then:   it reads the current states and eligibility only;
        it does not change TOKI to EARLY_ACQUAINTANCE.

When:   episode result is evaluated
Then:   Detected Delta must pass Expected/Allowed Delta Scope before writing.

Then:   Approved Delta contains only:
        TOKI NOT_MET -> EARLY_ACQUAINTANCE;
        L01 UNKNOWN -> VISITED;
        USER_FIRST_ARRIVAL;
        ROUTE_LOG;
        no shared canon delta.
```

## P0 Success Boundary

P0 成功不以“做出完整游戏系统”衡量，而以以下可验证闭环衡量：版本可追溯、Context 正确、Delta 受控、file-backed state round-trip 正确、旧默认套路未渗漏、现有静态生产线仍能渲染。

