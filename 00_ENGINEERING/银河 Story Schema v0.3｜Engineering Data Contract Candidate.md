# 银河 Story Schema v0.3
## Engineering Data Contract Candidate

**状态：ENGINEERING CANDIDATE**  
**目标：作为 CodeX / 李光生产系统的单集数据合同基线**  
**上游：Course / World Canon / Character Canon / Prehistory / Map / Reference Library / Runtime**  
**下游：Story Profile → Story Beats → Script/Reader → QA → Episode Delta**

---

# 0. v0.3 的核心变化

v0.3 不再只是回答“故事需要哪些字段”，而开始回答：

> **系统应该从哪里读、允许谁改、生成以后写回哪里，以及哪些变化绝不能自动进入 Canon。**

正式生产链：

```text
Source of Truth
    ↓
Context Assembly
    ↓
Story Profile
    ↓
Validation Gates
    ↓
Story Beats
    ↓
Narrative Rendering
    ↓
Runtime Output
    ↓
Post-generation Extraction
    ↓
Episode Delta
    ↓
State Update / Canon Proposal
```

其中最重要的约束是：

> **生成器只消费 Context，不直接拥有 Canon。**

---

# 1. Source-of-Truth 原则

任何 Story Profile 都必须声明自己基于哪些版本生成。

```yaml
release_context:
  galaxy_release_id: GALAXY-CONTENT-2026.09-R1

  course_source_version: "5.0.7"
  world_canon_version: "1.0"
  character_canon_version: "0.1"
  prehistory_version: "0.1"
  map_canon_version: "0.1"
  reference_library_version: "0.1"
  story_schema_version: "0.3"
  production_spec_version: "0.2"
  runtime_config_version: "TBD"
```

以后必须能够回答：

> “EP037 是根据哪一版世界和哪一版人物生成的？”

---

# 2. ID 优先，禁止复制 Canon

Story Profile 不复制完整角色设定。

错误：

```yaml
primary_character:
  name: 康缇
  personality: 冷静、谨慎……
  backstory: ...
```

正确：

```yaml
cast:
  primary_character_id: KONTI
```

运行时：

> 从 Character Canon Registry 读取最新允许版本。

同理适用于：

- `course_id`
- `character_id`
- `location_id`
- `reference_asset_id`
- `guest_registry_id`
- `thread_id`

原则：

> **Reference, don't duplicate.**

---

# 3. v0.3 顶层结构

```yaml
story_profile:

  meta:
  release_context:

  course_contract:

  persistence_policy:

  context_snapshot:

  character_eligibility:

  story_contract:

  cast:

  user_agency:

  lesson_evidence:

  knowledge_contract:

  success_criteria:

  user_artifact:

  branch_policy:

  narrative_rendering:

  runtime_constraints:

  safety_controls:

  output_contract:
```

生成结束后另产生：

```yaml
episode_delta:
```

不要把：

> `story_profile`

和：

> `episode_delta`

混成一个对象。

前者是：

> **生产前合同。**

后者是：

> **生产后变化。**

---

# 4. Meta

```yaml
meta:
  episode_id: GALAXY-001
  course_id: COURSE-001
  schema_version: "0.3"

  status:
    CANDIDATE | APPROVED | RELEASED | DEPRECATED

  production_mode:
    PILOT | PRODUCTION
```

`PILOT`：

> 不自动更新长期状态。

`PRODUCTION`：

> 允许产生正式 Route Continuity / Personal State Delta。

---

# 5. Persistence Policy
## 取代旧的 `canon_scope`

这是 v0.3 的关键修正。

正式采用：

```yaml
persistence_policy:

  shared_canon_write:
    allowed: false

  route_continuity_write:
    allowed: true

  personal_state_write:
    allowed: true

  simulation_state:
    enabled: false

  shared_canon_proposal:
    allowed: false
```

---

# 6. 为什么不用 `Route Canon`

银河只有：

# Shared Canon

即：

> 全局公共世界事实。

例如：

- 四星域存在；
- 拓奇存在；
- 大通航历史发生过。

而用户产生的是：

## Route Continuity

“我经历过什么”。

## Personal Relationship State

“我和谁是什么关系”。

## Personal Discovery State

“我现在知道什么”。

## Personal Artifact State

“我做过什么作品”。

这些：

> 不是另一套银河正史。

---

# 7. Shared Canon 默认只读

普通单集 Agent：

```yaml
shared_canon_write:
  allowed: false
```

因此不得自动做出：

> “从今天起银河所有中继站永久改变规则。”

如果剧情产生值得升级为公共事实的变化，只能生成：

```yaml
shared_canon_proposal:
  eligible: true
  proposal_id: CP-001
  proposed_change: ...
  reason: ...
  approval_required: true
```

之后进入：

```text
PROPOSED
→ CANON REVIEW
→ APPROVED
→ RELEASE
```

---

# 8. Course Contract

课程是教育事实源。

```yaml
course_contract:
  course_id: COURSE-001

  source_fields:
    package:
    unit:
    lesson_index:
    title:
    summary:
    knowledge_points:
    learning_goal:
    primary_competency:
    literacy_goal:

  learning_priority:
    must_understand: []
    should_recognize: []
    first_exposure_only: []
```

`learning_priority` 是 EP001 评审后新增。

因为：

> **课程覆盖 ≠ 所有知识同强度掌握。**

---

# 9. Learning Action

```yaml
learning_action:
  primary: distinguish
  secondary:
    - decide
```

推荐枚举：

```text
observe
ask
describe
classify
distinguish
compare
verify
define
judge
design
generate
test
revise
iterate
collaborate
weigh
reflect
```

每集：

> 一个主要动作。

最多一到两个辅助动作。

---

# 10. Context Snapshot

这是生产系统每集真正需要自动组装的核心。

```yaml
context_snapshot:

  timeline:
    current_phase:
    previous_episode_id:
    elapsed_story_time:

  location:
    current_location_id:
    previous_location_id:
    available_routes: []

  route_continuity:
    previous_events: []
    unresolved_route_threads: []

  user:
    known_characters: []
    learned_capabilities: []
    important_choices: []

  relationship_state:
    TOKI:
      state:
      remembered_events: []

  discovery_state:
    known_locations: []
    heard_of_locations: []
    known_world_facts: []

  character_state:
    TOKI:
      current_location:
      current_goal:
      known_information: []
      unresolved_personal_threads: []

  world_state:
    active_events: []
    changed_locations: []

  knowledge_state:
    confirmed: []
    likely: []
    hypotheses: []
    unknown: []
```

---

# 11. Context Assembly Layer

李光的生产工具以后不应继续维护一个巨大 Prompt。

应该建立：

# `ContextAssembler`

输入：

```text
course_id
user_id / route_id
episode_id
runtime_version
```

自动读取：

```text
Course Source
+
World Canon
+
Character Canon
+
Relevant Prehistory
+
Map State
+
Route Continuity
+
Personal States
+
Approved Reference Assets
+
Runtime Constraints
```

输出：

```yaml
context_snapshot:
```

然后再交给生成 Agent。

---

# 12. Relevant Context，而不是 Full Context

禁止每次把：

> 6万字前史 + 完整世界圣经 + 全部角色圣经

全部塞进生成 Prompt。

ContextAssembler 必须执行：

```text
Retrieve
→ Filter
→ Resolve Permissions
→ Assemble
```

例如 EP001：

不需要读取：

- 大通航家全部历史；
- SHARED_EVENT_01；
- 爱今私人秘密；
- 先锋星域全部地点。

只读：

> 当前真正相关的信息。

---

# 13. Epistemic Permission

这是强约束。

角色能够使用的信息必须满足：

```text
World Truth
AND
Character Has Access
```

不能：

```text
World Truth
→ Character Automatically Knows
```

因此：

```yaml
character_state:
  KONTI:
    known_information:
      - K-019
      - K-024
```

Agent只能让康缇基于这些信息讲话。

---

# 14. Character Eligibility

```yaml
character_eligibility:

  TOKI:
    introduced_to_user: true
    available_in_timeline: true
    location_reachable: true
    appearance_allowed: true

  KONTI:
    introduced_to_user: false
    appearance_allowed: false
```

必须同时检查：

- 已经认识吗？
- 时间线上可用吗？
- 地理上合理吗？
- 当前故事需要吗？

---

# 15. Cast Reason

即使角色：

> technically eligible

也不能自动出现。

```yaml
cast:

  primary_character_id: TOKI

  supporting_character_ids: []

  cast_reason:
    narrative_reason:
      "拓奇正在初航锚区附近活动，并对新的现实探索者产生兴趣。"

    relationship_reason:
      "建立用户第一段银河人物关系。"

    competency_relevance:
      "好奇心适合开启AI概念探索，但不是唯一原因。"
```

原则：

> **Narrative reason 必须成立。**

---

# 16. Story Contract

```yaml
story_contract:

  working_title:

  opening_mode:
    arrival

  trigger:

  central_question:

  immediate_goal:

  stakes:

  location_id:

  story_grammar:
    trigger:
    goal:
    constraint:
    relationship:
    knowledge_action:
    twist:
    outcome:
```

其中 `story_grammar`：

> 仅供生产内部。

绝不进入儿童文本。

---

# 17. Opening Mode Enum

```text
arrival
quiet_discovery
daily_life
conversation
incoming_message
mission_request
scientific_observation
relationship_event
found_object
ongoing_event
memory
user_creation_trigger
travel_scene
celebration
mystery
```

禁止默认：

```text
alarm
damage
emergency
```

成为万能开头。

---

# 18. Lesson Evidence

```yaml
lesson_evidence:

  provided_materials: []

  task_materials: []

  answer_basis: []

  authoritative_for_episode: []
```

区别于外部事实资料。

例如：

> 本课提供的任务卡

属于 Lesson Evidence。

NASA科学资料属于：

> Reference Assets。

---

# 19. Knowledge Contract

```yaml
knowledge_contract:

  required_concepts: []

  reference_asset_ids: []

  truth_domain:
    - REAL_WORLD_FACT
    - FICTIONAL_WORLD_FACT

  truth_labels: []

  allowed_use_modes:
    - RULE
    - EVIDENCE
    - TOOL
    - REFLECTION
    - DEEP_NOTE
```

---

# 20. Truth Domain
## 与 Truth Label 分离

正式区分：

### Truth Domain

```text
REAL_WORLD_FACT
FICTIONAL_WORLD_FACT
METAPHOR
CREATOR_MODEL
```

### Truth Label

```text
FACT
ESTIMATE
HYPOTHESIS
OPEN_QUESTION
FICTION
SAFETY_RULE
```

为什么分开？

例如：

> “星粒能够产生信息相关反应。”

后台可能是：

```yaml
truth_domain: FICTIONAL_WORLD_FACT
truth_label: FACT
```

意思是：

> 在银河世界里是真的。

但不是：

> 现实科学事实。

---

# 21. Deep Note

```yaml
deep_note:
  enabled: false

  asset_id:

  preferred_voice:

  placement:

  testable: false
```

硬规则：

> Deep Note 不得立即考。

---

# 22. User Agency

```yaml
user_agency:

  user_goal:

  information_available: []

  decision_points:
    - decision_id:
      prompt:
      actions: []

  consequence_visibility: true

  retry_allowed: true

  alternative_reasoning_allowed: true
```

至少有一个：

> 真正改变体验状态的用户行为。

---

# 23. Fake Choice Validator

QA 应检测：

如果三个选项：

```text
A → 同一回复
B → 同一回复
C → 同一回复
```

且：

> 没有任何关系/状态/反馈变化，

则标记：

```text
FAKE_CHOICE_WARNING
```

即使最终剧情汇合：

> 用户选择也必须留下某种记忆。

---

# 24. Branch Policy

```yaml
branch_policy:

  branching_level:
    LIGHT

  convergence_required:
    true

  decisions:
    - decision_id: D001
      convergence_point: SCENE-07

      memory_write:
        state_key: first_action
```

当前产品：

> 不需要复杂多结局。

但：

> 不能假装用户没选过。

---

# 25. Success Criteria

```yaml
success_criteria:

  observable_behaviors:
    - "能够区分AI大类与具体AI应用"

  evidence_method:
    - choice
    - generated_artifact

  required_for_completion: []
```

禁止只写：

> “理解了。”

---

# 26. User Artifact

```yaml
user_artifact:

  enabled: true

  artifact_type:
    route_log

  artifact_id:

  required_fields: []

  persistence:
    PERSONAL

  evaluation_criteria: []
```

Artifact persistence：

```text
EPHEMERAL
ROUTE
PERSONAL
SHARED_PROPOSAL
```

---

# 27. Narrative Rendering

```yaml
narrative_rendering:

  output_mode:
    SCRIPT

  target_age:
    min: 9
    max: 16

  language:
    zh-CN

  style:
    modern
    natural
    clear
    restrained
    imaginative

  sentence_complexity:
    medium

  explanation_density:
    low_to_medium

  emotional_intensity:
    low

  world_exposition_limit:
    minimal
```

---

# 28. Output Modes

正式支持：

```text
OUTLINE
SCRIPT
READER
IMPLEMENTATION
```

## OUTLINE

策划使用。

## SCRIPT

对白、动作、互动、UI。

## READER

纯儿童阅读体验。

## IMPLEMENTATION

工程层页面、素材、交互状态。

不要再把四种格式混成一份。

---

# 29. Scene Schema

```yaml
scene:

  scene_id:

  location_id:

  characters_present: []

  what_user_sees:

  active_goal:

  emotional_state:

  new_information: []

  dialogue_blocks: []

  user_action:

  state_change:
```

---

# 30. Scene Completeness Validator

每个新场景检查：

```text
WHERE
WHO
WHAT
WHY
```

不要求显式说明。

但用户必须能理解：

- 在哪；
- 谁在；
- 正发生什么；
- 为什么要继续看。

否则：

```text
SCENE_CONTEXT_INCOMPLETE
```

---

# 31. Runtime Constraints

Runtime Config 注入，不属于 Canon。

```yaml
runtime_constraints:

  target_duration_minutes:
    min: 5
    max: 8

  max_core_interactions: 2

  max_primary_dialogue_characters: 2

  complex_branching: false

  persistent_inventory: false

  supported_interaction_types:
    - choice
    - sort
    - select
```

Runtime 可升级。

世界不需要随之 Retcon。

---

# 32. Safety Controls

```yaml
safety_controls:

  age_band:
    9_16

  forbidden_patterns:
    - emotional_dependency
    - humiliation_as_motivation
    - ordinary_error_causes_catastrophe
    - AI_as_omniscient_authority
    - blind_obedience
    - unsafe_real_world_instruction

  child_privacy_required: true
```

---

# 33. Forbidden Tropes

单独维护 Registry。

Story Profile只引用：

```yaml
forbidden_trope_ids:
  - TROPE-SHIP-DAMAGE-DEFAULT
  - TROPE-TOKI-PLAYS-DUMB
  - TROPE-MANDATORY-CRISIS
  - TROPE-AJI-ETHICS-CALLBACK
```

这样新增规则：

> 不必修改117份Prompt。

---

# 34. Output Contract

生成器最终必须返回：

```yaml
output_contract:

  required_outputs:
    - story_beats
    - script
    - detected_state_changes
    - proposed_episode_delta

  optional_outputs:
    - visual_direction
    - deep_note
```

---

# 35. Episode Delta v0.3

生成结束后：

```yaml
episode_delta:

  episode_id:

  route_continuity_delta:

  relationship_delta:

  discovery_delta:

  artifact_delta:

  location_delta:

  character_delta:

  knowledge_delta:

  thread_delta:

  shared_canon_proposal:
```

---

# 36. Route Continuity Delta

```yaml
route_continuity_delta:

  important_events_added: []

  route_choices_added: []

  unresolved_route_threads_added: []

  unresolved_route_threads_resolved: []
```

---

# 37. Relationship Delta

```yaml
relationship_delta:

  TOKI:
    previous_state:
      NOT_MET

    new_state:
      EARLY_ACQUAINTANCE

    memories_added:
      - USER_FIRST_ARRIVAL

    impression_changes:
      curious_about_user: true
```

---

# 38. Discovery Delta

```yaml
discovery_delta:

  locations:
    L01:
      UNKNOWN -> VISITED

  people:

  world_facts:

  historical_facts:
```

注意：

> 用户发现某个事实

不等于：

> 这个事实刚刚发生。

---

# 39. Artifact Delta

```yaml
artifact_delta:

  created:
    - artifact_id:
      artifact_type:
      persistence:

  modified: []

  archived: []
```

---

# 40. Character Delta

只记录角色自身真实变化。

例如：

> 普罗修正了一项长期观点。

不能因为：

> 用户认识了普罗

就修改普罗的 Shared Character Canon。

关系变化通常属于：

> Personal Relationship State。

---

# 41. Knowledge Delta

必须区分：

```yaml
knowledge_delta:

  user_learned: []

  character_learned: []

  world_truth_changed: []
```

一般情况下：

```text
world_truth_changed = []
```

事实不会因为角色：

> 今天才知道

而今天才变成真的。

---

# 42. Thread Delta

```yaml
thread_delta:

  opened: []

  advanced: []

  dormant: []

  resolved: []
```

状态：

```text
OPEN
ACTIVE
DORMANT
RESOLVED
ABANDONED
```

DORMANT：

> 故意暂时不提。

不是：

> Agent忘了。

---

# 43. Shared Canon Proposal

默认：

```yaml
shared_canon_proposal:
  eligible: false
```

必要时：

```yaml
shared_canon_proposal:

  eligible: true

  proposal_id:

  canon_level:
    C2

  proposed_change:

  reason:

  affected_entities: []

  approval_required:
    true
```

生成 Agent：

> 永远没有 APPROVE 权限。

---

# 44. Post-generation Extraction

系统生成完故事以后，应再跑一个：

# `DeltaExtractor`

自动抽取：

```text
new characters
new locations
new facts
relationship changes
world changes
new objects
new promises
new unresolved questions
```

然后比较：

```text
Expected Delta
vs
Detected Delta
```

---

# 45. Unauthorized Mutation

如果 Story Profile 预计：

```text
world_delta = none
```

生成文本却出现：

> “原来创想星域三百年前毁灭过一次。”

系统应：

```text
BLOCK
```

并报告：

```text
UNAUTHORIZED_CANON_MUTATION
```

不能：

> 因为故事写得不错就自动接受。

---

# 46. Validation Gates

生成前：

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

生成后：

```text
Narrative QA
Readability QA
Delta QA
Canon Mutation QA
Character Voice QA
```

---

# 47. Gate Fail Policy

三类：

## BLOCK

不得生成/发布。

例如：

> 未认识康缇却写成多年好友。

## WARN

允许生成，但需要人工审核。

例如：

> 新建C3临时NPC。

## INFO

记录即可。

例如：

> 本集第一次访问某地点。

---

# 48. Story Profile EP001 示例

```yaml
meta:
  episode_id: GALAXY-001
  course_id: COURSE-001
  schema_version: "0.3"
  production_mode: PRODUCTION

release_context:
  world_canon_version: "1.0"
  character_canon_version: "0.1"
  prehistory_version: "0.1"
  map_canon_version: "0.1"
  story_schema_version: "0.3"

persistence_policy:
  shared_canon_write:
    allowed: false
  route_continuity_write:
    allowed: true
  personal_state_write:
    allowed: true
  shared_canon_proposal:
    allowed: false

course_contract:
  learning_priority:
    must_understand:
      - AI_IS_BROAD_CATEGORY
      - APPLICATION_IS_CONCRETE_USE
      - HUMAN_DECIDES
    should_recognize:
      - CHATBOT
    first_exposure_only:
      - LLM
      - GENAI
      - AIGC

context_snapshot:
  location:
    current_location_id: L01

  user:
    known_characters: []

  relationship_state:
    TOKI:
      state: NOT_MET

character_eligibility:
  TOKI:
    introduced_to_user: false
    available_in_timeline: true
    location_reachable: true
    appearance_allowed: true

  KONTI:
    appearance_allowed: false

story_contract:
  working_title: 谁在和我说话？
  opening_mode: arrival
  central_question: AI到底是什么？
  immediate_goal: 完成首次抵达并理解眼前AI应用
  stakes: low

cast:
  primary_character_id: TOKI

  cast_reason:
    narrative_reason:
      拓奇正在接入区域附近活动，对现实探索者好奇。

user_agency:
  decision_points:
    - decision_id: D001
      purpose: concept_relationship
    - decision_id: D002
      purpose: first_route_choice

branch_policy:
  branching_level: LIGHT
  convergence_required: true

narrative_rendering:
  output_mode: SCRIPT
  target_age:
    min: 9
    max: 16

runtime_constraints:
  target_duration_minutes:
    min: 5
    max: 8
  max_core_interactions: 2

safety_controls:
  child_privacy_required: true
```

---

# 49. EP001 Episode Delta 示例

```yaml
episode_delta:

  episode_id: GALAXY-001

  route_continuity_delta:
    important_events_added:
      - USER_FIRST_ARRIVAL

    route_choices_added:
      - FIRST_ACTION

  relationship_delta:
    TOKI:
      previous_state: NOT_MET
      new_state: EARLY_ACQUAINTANCE

  discovery_delta:
    locations:
      L01:
        previous: UNKNOWN
        new: VISITED

  artifact_delta:
    created:
      - artifact_type: ROUTE_LOG
        persistence: PERSONAL

  knowledge_delta:
    user_learned:
      - AI_IS_BROAD_CATEGORY
      - APPLICATION_IS_CONCRETE_USE
      - HUMAN_DECIDES

  shared_canon_proposal:
    eligible: false
```

---

# 50. Deprecated Fields

从 v0.3 起：

```text
canon_writeback
canon_scope
Route Canon
```

正式废弃。

替代：

```text
persistence_policy
route_continuity
personal_state
shared_canon_proposal
```

---

# 51. 旧生产系统优先迁移项

第一批建议 CodeX 搜索当前代码/Prompt中的：

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

原则：

> **无显式配置 = 不发生。**

而不是：

> 没写就使用旧默认套路。

---

# 52. 推荐工程模块

最小模块：

```text
CanonRegistry
CharacterRegistry
LocationRegistry
ReferenceRegistry

ContextAssembler

StoryProfileValidator

NarrativeGenerator

DeltaExtractor

StateWriter

CanonProposalQueue

RegressionTestRunner
```

---

# 53. 最重要的权限设计

## 内容团队

拥有：

- Canon Proposal
- Character Update
- Reference Update
- Story Spec

## Canon Owner

拥有：

- C0/C1 Approve
- Canon Release

## CodeX / 工程

拥有：

- Schema implementation
- Validator
- migration
- automated QA

## Generator Agent

拥有：

- Create Episode
- Propose Delta

没有：

> Canon mutation permission.

---

# 54. v0.3 最小实现 MVP

李光第一轮**不必实现完整宇宙数据库**。

P0：

```text
1. version manifest
2. ContextAssembler
3. Story Profile JSON/YAML schema
4. character eligibility
5. route continuity
6. relationship state
7. episode delta
8. validation
9. deprecated hardcode removal
```

P1：

```text
Location Ledger
Knowledge Ledger
Thread Ledger
Reference automatic retrieval
Canon proposal workflow
```

P2：

```text
Community Timeline
多人公共事件
复杂Branch
自动Canon Promotion
```

---

# 55. v0.3 冻结原则

1. **银河只有一套 Shared Canon。**
2. **用户个性化属于 Continuity / State，不制造多套正史。**
3. **Story Profile 是生产前合同。**
4. **Episode Delta 是生产后变化。**
5. **Canon 默认只读。**
6. **生成 Agent 只有 Proposal 权，没有 Canon 写权限。**
7. **角色只允许使用自己知道的信息。**
8. **课程只通过 ID 引用唯一 Source of Truth。**
9. **Runtime 约束不能写成世界规则。**
10. **Context 应按需组装，不把全部 Canon 塞进 Prompt。**
11. **用户选择可以汇合，但必须留下记忆。**
12. **生成后必须自动检查未授权的新事实。**
