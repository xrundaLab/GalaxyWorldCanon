# 银河 Story Schema v0.2
## 单集生产合同、Continuity 与叙事呈现规范

**版本：v0.2**  
**日期：2026-09-11**  
**所属：Galaxy Storyworld System**

---

# 0. 文档定位

Story Schema 不是儿童故事文本，也不是编剧 Prompt。

它是：

> **课程、世界 Canon、角色 Canon、Reference Library、Continuity 与下游剧情生产之间的结构化合同。**

它负责规定：

- 这一课真正要学什么；
- 这一集为什么会发生；
- 谁参与；
- 用户真正做什么；
- 知识如何进入事件；
- 行动造成什么后果；
- 人物和世界留下什么变化；
- 下一集必须记住什么。

Story Schema 之后，还必须经过一层：

> **Narrative Rendering / 叙事呈现**

才能成为真正给孩子看的剧情。

因此正式生产链修改为：

```text
Course Source of Truth
        ↓
World / Character Canon
        ↓
Controlled Reference Library
        ↓
Continuity Snapshot
        ↓
Story Profile
        ↓
Story Beats
        ↓
Narrative Rendering
        ↓
Child-facing Script / Story
        ↓
QA
        ↓
Episode Delta
        ↓
Canon Ledger
```

---

# 1. v0.2 新增的核心认识

Pilot 已经证明：

> **结构成立 ≠ 故事已经成立。**

一个故事可能拥有完整的：

- Trigger；
- Goal；
- Decision；
- Consequence；
- Learning Delta；

却仍然读起来：

> 生硬、跳跃、信息不足、像流程图。

因此今后必须明确区分三个产物。

## L1｜Story Profile

机器和策划可读。

回答：

> 这集为什么成立？

---

## L2｜Story Beats

编剧和导演可读。

回答：

> 场景如何一步一步发生？

---

## L3｜Narrative Output

儿童真正看到、读到、听到。

回答：

> 这件事情怎样自然地被经历？

三层不得混写。

---

# 2. 三个质量标准

每集必须同时满足：

## Education Test

> 用户真正练习了什么？

## Story Test

> 去掉课程标签，人物、事件、目标和结果仍然成立吗？

## Reading Test

> 一个9—16岁孩子第一次看到这一段，能不能自然理解“谁在哪里、发生了什么、为什么要继续看”？

第三项是 v0.2 正式新增。

---

# 3. Story Profile 总结构

```yaml
story_profile:

  meta:

  course_contract:

  learning_action:

  lesson_evidence:

  continuity_snapshot:

  character_eligibility:

  story_premise:

  cast:

  user_agency:

  knowledge_integration:

  success_criteria:

  consequence:

  user_artifact:

  branch_convergence:

  outcome:

  production_controls:

  narrative_rendering:

  episode_delta:
```

---

# 4. Meta

```yaml
meta:
  schema_version:
  episode_id:
  course_id:

  mode:
    PILOT_SANDBOX | PRODUCTION | CANON

  canon_writeback:
    true | false
```

`PILOT_SANDBOX` 中出现的新地点、NPC、关系或事件默认不能进入正式 Canon。

---

# 5. Course Contract

课程仍然是教育目标的唯一事实源。

```yaml
course_contract:
  package:
  unit:
  lesson_index:
  title:
  summary:
  knowledge_points:
  learning_goal:
  primary_competency:
  literacy_goal:
```

编剧不得自行修改课程目标。

---

# 6. Learning Action

```yaml
learning_action:
  primary:
  secondary:
```

主要动作优先从以下选择：

- 观察
- 提问
- 表达
- 分类
- 比较
- 核查
- 判断
- 设计
- 生成
- 测试
- 修改
- 迭代
- 协作
- 权衡
- 反思

每集原则上只突出一个主动作。

---

# 7. Lesson Evidence

课程自带材料与外部科学资料正式分离。

```yaml
lesson_evidence:
  provided_materials:
  task_materials:
  answer_basis:
```

例如：

> 一份课程中的“观星活动资料”

属于 Lesson Evidence。

NASA资料则属于：

> External Reference Asset。

不得混为同一种“资料”。

---

# 8. Continuity Snapshot

每集生成前必须读取：

```yaml
continuity_snapshot:

  timeline:
    story_day:
    journey_phase:
    current_star_region:
    previous_episode:
    elapsed_time:

  user:
    known_characters:
    learned_capabilities:
    important_choices:
    story_items:
    unresolved_threads:

  characters:
    TOKI:
      relationship_state:
      current_goal:
      known_information:

    KONTI:
      relationship_state:
      current_goal:
      known_information:

    PROTI:
      relationship_state:
      current_goal:
      known_information:

    AJI:
      relationship_state:
      current_goal:
      known_information:

  world:
    current_location:
    known_locations:
    active_events:

  knowledge:
    confirmed:
    hypotheses:
    unknowns:

  open_threads:
```

---

# 9. Epistemic Continuity

连续性不仅记录：

> 发生了什么。

还必须记录：

> **谁知道什么。**

因此任何角色不得：

- 提前知道未来剧情；
- 使用自己尚未获得的信息；
- 因为生成 Agent 读取了完整 World Bible 就表现为全知。

角色知识状态必须来自：

```text
known_information
```

而不是系统总知识。

---

# 10. Character Eligibility

```yaml
character_eligibility:

  TOKI:
    introduced:
    available:

  KONTI:
    introduced:
    available:

  PROTI:
    introduced:
    available:

  AJI:
    introduced:
    available:
```

用于防止：

> 正式初遇之前人物已经和用户很熟。

---

# 11. Story Premise

```yaml
story_premise:
  title:
  opening_mode:
  trigger:
  central_question:
  immediate_goal:
  stakes:
  location:
```

其中 `stakes` 不等于“大危机”。

可以只是：

> 如果弄错，一张活动通知会误导别人。

小问题同样可以成为故事。

---

# 12. Opening Mode

允许：

```text
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

禁止：

> 默认所有课程以故障、警报或灾难开始。

---

# 13. Story Grammar

不建立大量固定剧本模板。

使用：

```text
Trigger
×
Goal
×
Constraint
×
Relationship
×
Knowledge Action
×
Twist
×
Outcome
```

它是结构语法，不是儿童可见文本。

---

# 14. Cast

```yaml
cast:
  primary_companion:
  secondary_companion:
  supporting_characters:

  cast_reason:
    narrative_reason:
    relationship_reason:
    competency_relevance:
```

角色不能只因为“课程知识匹配”而出现。

例如：

> 原型课 ≠ 普罗每次自动登场。

必须存在剧情理由。

---

# 15. 四位主伙伴的底层认知差异

正式采用：

> **拓奇 Toki × 康缇 Konti × 普罗 Proti × 爱今 Aji**

隐藏概念链：

> **Token → Context → Prototype → Agency**

对应：

> **可能性 → 理解 → 原型 → 主体性与责任**

---

## 拓奇

首先看到：

> 还有什么可能。

---

## 康缇

首先看到：

> 信息放在什么上下文里。

---

## 普罗

首先考虑：

> 怎样做出一个可以测试的版本。

---

## 爱今

首先考虑：

> 谁在选择，谁受影响，谁承担后果。

这些是人物底色。

不是人物调度按钮。

---

# 16. User Agency

```yaml
user_agency:
  user_goal:
  information_available:

  decision_point:

  available_actions:

  choice_effect:

  consequence_visibility:

  retry_allowed:

  alternative_reasoning_allowed:
```

至少一次用户行动必须真正改变：

- 信息；
-过程；
-状态；
-人物回应；

其中至少一项。

---

# 17. Branch Convergence

当前产品可以保持线性主线。

但用户选择不能被立即遗忘。

```yaml
branch_convergence:
  divergence:
  consequence:
  convergence_point:
  memory_to_keep:
```

即：

> 路线可以重新汇合，但刚才发生过的事仍然发生过。

---

# 18. Knowledge Integration

```yaml
knowledge_integration:

  lesson_evidence:

  external_reference_assets:

  truth_labels:

  use_mode:
    RULE
    EVIDENCE
    TOOL
    REFLECTION
    DEEP_NOTE
```

优先级：

> **规则 / 证据 / 工具 / 反思 > 直接讲课。**

---

# 19. Deep Note

```yaml
deep_note:
  enabled:
  asset_id:
  voice:
  placement:
  testable: false
```

Deep Note：

- 不每集出现；
- 不要求立即理解；
- 不立即考试；
- 不强行解释完全。

作用：

> **让孩子感觉世界比当前课程更大。**

---

# 20. Success Criteria

课程成功不能只写：

> “理解了。”

必须尽量可观察。

```yaml
success_criteria:
  observable_behavior:
  test_method:
  required_evidence:
```

例如：

不是：

> 理解原型。

而是：

> 能用至少一组非正常输入发现问题，并完成一次修改与复测。

---

# 21. User Artifact

很多银河课程最终应该留下：

> 一件孩子真正做出来的东西。

```yaml
user_artifact:
  enabled:
  type:
  title:
  required_fields:
  evaluation_criteria:
  persistence:
```

可能包括：

- Prompt；
- 任务说明；
- 核查记录；
- Bug记录；
- 原型；
- 图像；
- 故事；
- 方案；
- 个人AI原则。

---

# 22. Consequence

```yaml
consequence:
  failure_type:
  immediate_effect:
  explanation:
  recovery_path:
  world_effect:
  character_effect:
```

错误不是：

> 红叉 + 再来一次。

而应尽可能成为：

> **故事事件。**

---

# 23. Learning Delta 与 Canon Delta 分离

正式拆开：

```yaml
outcome:

  learning_delta:
    learned:
    practiced:
    artifact:

  story_delta:
    character_change:
    world_change:
    narrative_change:

  canon_delta:
    proposed_changes:
```

孩子答错一次通常属于：

> Learning Record。

不应该自动进入：

> 世界永久正史。

---

# 24. Narrative Rendering Layer

这是 v0.2 最重要的新模块。

Story Profile 不能直接输出给儿童。

必须经过：

```text
Story Profile
↓
Story Beats
↓
Scene Writing
↓
Dialogue
↓
Child-facing Narrative
```

---

# 25. Story Beats

每一个 Beat 至少回答：

```yaml
story_beat:
  location:
  characters_present:
  what_user_sees:
  what_changes:
  emotional_state:
  new_information:
  user_action:
```

这样可以防止：

> 一句话突然跳到下一件事。

---

# 26. Scene Completeness

每个新场景首次进入时，至少让用户自然获得：

> **Where / Who / What / Why**

即：

- 在哪里；
- 谁在场；
- 正在发生什么；
- 为什么值得关注。

不要求写成说明文字。

可以通过：

- 画面；
- 动作；
- 对话；
-声音；
- 界面信息；

组合完成。

---

# 27. 禁止“关键词式叙事”

儿童可见内容不得长期使用这种形式推进：

```text
发现信号。
打开资料。
进行核查。
结果错误。
重新判断。
```

这属于内部 Beat。

真正呈现时必须转译成：

> 场景、动作、对白和必要叙述。

---

# 28. 信息不能吝啬到破坏理解

“短”不是价值本身。

当孩子理解一个动作需要：

- 前因；
- 对象；
- 材料；
- 人物态度；

就必须给足。

原则：

> **删掉冗余，不删掉理解所需的信息。**

---

# 29. 台词长度规则修正

过去“角色一次优先1—2句”只能理解为：

> 避免连续长篇讲课。

不能理解为：

> 每个人每次只能蹦几个字。

允许角色在真正需要时：

- 连续说两三句；
- 停顿；
- 改口；
- 补充；
- 被打断。

关键标准：

> **像人在说话，而不是像界面提示。**

---

# 30. 儿童友好不等于低龄化

9—16岁用户可以阅读：

- 正常完整句；
- 少量陌生词；
- 真正科学词汇；
- 更成熟的知识表达。

但上下文应该帮助孩子：

> 大致知道这句话在说什么。

不要求：

> 每个词第一次出现就解释。

---

# 31. Narrative Density

故事文本必须拥有足够的：

### Environment

环境与空间感。

### Action

人物正在做什么。

### Reaction

人物对事情的反应。

### Connection

上一事件为什么导致下一事件。

### Silence

必要的停顿和留白。

缺少这些，故事会退化成：

> PPT提纲。

---

# 32. Dialogue Rule

角色对白首先必须：

> **回应眼前正在发生的事。**

其次才负责：

> 知识、价值观和剧情信息。

禁止：

> 为了告诉儿童一个知识点，让角色突然念定义。

---

# 33. Explanation Bridge

当故事从事件进入学习动作时，需要存在一个：

> **理解桥梁。**

例如：

事件：

> AI把没有来源的信息写成已确认事实。

不能直接跳：

> “请选择如何核查。”

应该先让用户看到：

- 哪里不对；
- 为什么值得重新看；
- 人物有什么反应。

然后才进入行动。

---

# 34. Choice Presentation

选择必须让孩子清楚：

> **我现在在决定什么。**

因此互动前应尽量保持：

- 当前目标；
- 关键材料；
- 选择对象；

仍然可见或可快速回看。

---

# 35. Child-facing Output 不显示内部字段

以下内容原则上只属于后台：

```text
Trigger
Learning Delta
Canon Delta
Success Criteria
Truth Label
Story Grammar
Branch Convergence
Episode Delta
```

儿童不会看到：

> “你刚完成了Learning Delta。”

它们应变成自然体验。

---

# 36. Narrative Output 三种模式

建议生产系统支持：

## OUTLINE

策划和 Agent 使用。

---

## SCRIPT

编剧、分镜、产品实现使用。

包含：

- Scene；
- Dialogue；
- Interaction；
- UI；
- Action。

---

## READER

真正儿童阅读文本。

语言完整、自然、有画面。

今后三集 Pilot 如果再展示给策划人员审阅，我建议同时提供：

> **OUTLINE + 一段 READER 样稿。**

这样不会再发生：

> 看结构时误以为这就是孩子最终看到的文字。

---

# 37. 年龄适配不应只靠字数

至少考虑：

```yaml
narrative_rendering:
  age_band:
  sentence_complexity:
  vocabulary_level:
  abstraction_level:
  emotional_intensity:
  explanation_density:
```

但不能机械执行。

目标是：

> **9岁能跟得上，16岁不觉得幼稚。**

---

# 38. 推荐基础语言风格

银河默认：

> **现代、自然、清楚、克制、有想象力。**

避免：

- 教材腔；
- 主持人口吻；
- 营销文案；
- 过度卖萌；
- 过度文学；
- 大量感叹号；
- 所有人都说完整金句。

---

# 39. Narrative QA

在正式生产 QA 中新增：

## Readability Gate

检查：

1. 孩子是否知道当前在哪里？
2. 是否知道谁正在说话/行动？
3. 是否知道为什么事情值得处理？
4. 是否存在突然跳步？
5. 是否有只有内部人员才懂的省略？
6. 是否把 Beat 当成成品文案？
7. 对话是否自然？
8. 知识出现前是否有情境支持？
9. 互动前是否知道自己为什么要选？
10. 读完以后是否像经历了一件事，而不是完成了一组页面？

---

# 40. Production Controls

```yaml
production_controls:
  duration_target:
  interaction_limit:
  runtime_constraints:
  visual_assets:
  forbidden_tropes:
  safety_tags:
```

注意：

> Runtime Constraints 不是 World Canon。

---

# 41. Episode Delta

```yaml
episode_delta:

  timeline_delta:

  learning_record_delta:

  character_delta:

  world_delta:

  knowledge_delta:

  thread_delta:

  canon_writeback_request:
```

---

# 42. Canon Ledger

仍至少维持：

```text
Timeline Ledger
Character Ledger
World State Ledger
Knowledge Ledger
Thread Ledger
```

---

# 43. Canon Level

```text
C0 IMMUTABLE
C1 STABLE
C2 FLEXIBLE
C3 EPISODE
```

所有非C3的新事实必须根据级别进入审批。

---

# 44. Retcon

任何历史修改必须显式记录。

```yaml
retcon_request:
  canon_id:
  old_value:
  new_value:
  reason:
  affected_episodes:
  approved_by:
```

禁止静默覆盖。

---

# 45. Reference Library 对接

每条外部资料至少传入：

```text
asset_id
truth_label
story_use_mode
preferred_voice
verification_status
```

未经批准的高风险事实不得进入正式剧本。

---

# 46. Character Pack 对接

角色通过：

```text
character_id
```

读取最新 Canon。

不把完整人格复制进每课 Prompt。

---

# 47. Guest Registry 对接

具名 NPC 通过：

```text
registry_id
cameo_mode
```

调用。

不在单集脚本里重新发明人物。

---

# 48. Reality Anchor

晶老师单独支持：

```yaml
reality_anchor:
  enabled:
  mode:
  topic:
  appearance_depth:
```

允许：

- opening；
- communication；
- deep_note；
- reflection；
- return_to_reality；
- special_episode。

---

# 49. v0.2 QA Gate

正式生产前至少通过：

1. Course Gate
2. Canon Gate
3. Character Gate
4. Continuity Gate
5. Reference Gate
6. Safety Gate
7. **Readability Gate**

生成后再进行：

8. Delta QA

---

# 50. v0.2 MVP 字段

给第一版工程系统至少实现：

```text
course_id

mode
canon_writeback

story_trigger
story_goal

primary_companion
supporting_characters
character_eligibility

lesson_evidence
reference_assets

user_action
decision_point

success_criteria
failure_consequence

user_artifact

previous_state
episode_delta

narrative_output_mode

forbidden_tropes
safety_tags
```

---

# 51. v0.2 最重要的原则

Story Schema 的任务不是：

> 把故事压缩成字段。

而是：

> **保证故事在进入创作之前逻辑成立，在创作完成以后仍然属于同一个银河。**

真正给孩子的故事则必须重新长出：

> 场景、声音、动作、人物反应、理解过程、悬念、幽默、停顿和情绪。

所以以后生产中应牢记：

> **后台越结构化，前台越应该自然。**

> **Schema负责秩序，故事负责生命。**

> **不能把给Agent看的东西直接给孩子看。**

---

# 52. v0.2 冻结新增项

相较 v0.1 正式新增：

1. `mode / canon_writeback`
2. `character_eligibility`
3. `lesson_evidence`
4. `epistemic continuity`
5. `user_artifact`
6. `success_criteria`
7. `branch_convergence`
8. `learning_delta / canon_delta 分离`
9. `cast_reason`
10. `Narrative Rendering Layer`
11. `Story Beat`
12. `Scene Completeness`
13. `Readability Gate`
14. `Narrative Output Mode`

---

# 53. 最终定义

银河的一集不能只是：

> **一个设计正确的学习流程。**

它必须首先让孩子感受到：

> **“刚才真的发生了一件事。”**

在这件事里：

有人看见了什么；

有人误会了什么；

有人提出了不同意见；

孩子自己做了一个决定；

决定产生了结果；

然后孩子才发现：

> **原来我刚刚学会了一种新的看世界、看AI、看问题的方法。**

这才是 Story Schema 最终要保护的东西。