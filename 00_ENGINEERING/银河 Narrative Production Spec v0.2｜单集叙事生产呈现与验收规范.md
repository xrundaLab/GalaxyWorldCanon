# 银河 Narrative Production Spec v0.2
## 单集叙事生产、呈现与验收规范

**版本：v0.2**  
**日期：2026-09-11**  
**状态：ENGINEERING / CONTENT HANDOFF CANDIDATE**  
**上游：Course Source of Truth / World Canon / Character Canon / Prehistory / Map Canon / Controlled Reference Library / Story Schema v0.3 / Runtime Config**  
**下游：Story Beats / Script / Reader / Implementation / QA / Episode Delta**

---

# 0. 文档定位

Narrative Production Spec 只回答一件事：

> **一集银河剧情到底应该怎样被生产、怎样呈现、怎样验收。**

它不再承担：

- 定义银河世界是什么；
- 定义拓奇、康缇、普罗、爱今是谁；
- 定义历史真相；
- 定义 Story Profile 数据结构；
- 定义产品当前能实现哪些技术能力。

这些职责分别属于：

- World Canon；
- Character Canon；
- Prehistory / Timeline；
- Story Schema；
- Runtime Config。

本文件只管理：

> **生产过程与成品质量。**

---

# 1. v0.2 相对 v0.1 的变化

v0.1 的核心目标是防止剧情游戏量产时反复出现：

- 飞船故障；
- 伙伴装笨；
- 知识硬插入；
- 一页一题；
- 低幼奖励。

这些原则继续保留。

但 v0.2 做四项结构性升级：

## 1.1 世界规则不再写在 Production Spec

世界事实全部引用：

> World Canon / Map Canon / Prehistory。

## 1.2 角色人格不再复制

角色全部通过：

> `character_id`

读取 Character Canon。

## 1.3 单集结构不再由固定模板控制

从：

> 固定“冷开场→任务→案例→知识→结果”

升级成：

> **Story Profile + Story Grammar + 多种 Opening Mode + Narrative Rendering。**

## 1.4 生产必须读状态、写 Delta

从“每节独立生成”升级为：

> **Context Snapshot → Episode → Episode Delta。**

---

# 2. 正式生产目标

每一集至少同时做到五件事：

1. **故事成立。**
2. **人物可信。**
3. **用户真的做了什么。**
4. **课程学习目标真实发生。**
5. **这一集属于同一个持续存在的银河。**

如果只能满足其中三四项：

> 不算完成。

---

# 3. 非目标

本阶段不要求：

- 大型RPG；
- 高自由度开放世界；
- 复杂多结局；
- 长期战斗系统；
- 经济系统；
- 交易系统；
- 每课新增地图；
- 每课新增角色；
- 每课必须获得星粒；
- 每课必须有重大危机。

银河剧情游戏的“游戏感”主要来自：

> **目标、探索、选择、行动、反馈、关系、状态变化和持续世界。**

而不是数值堆叠。

---

# 4. 正式生产输入

一集进入生产以前，必须拥有完整的：

```text
Story Profile v0.3
```

并至少通过：

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

如果 Story Profile 不完整：

> 不进入脚本生成。

---

# 5. 正式生产输出

标准生产输出分四级：

## L1｜OUTLINE

给策划和审查者。

包括：

- 核心事件；
- 场景顺序；
- 用户动作；
- 关键后果。

## L2｜SCRIPT

给编剧、视觉、交互和产品实现。

包括：

- Scene；
- Dialogue；
- Action；
- UI；
- Interaction；
- Feedback。

## L3｜READER

面向儿童/青少年的纯体验文本。

它必须：

> 即使脱离内部字段也能自然阅读。

## L4｜IMPLEMENTATION

给工程系统。

包括：

- 页面；
- 素材；
- 交互类型；
- 状态变化；
- 埋点；
- Delta Hook。

四层不得混写。

---

# 6. 单集时长

当前默认目标：

> **5–8分钟。**

这是 Runtime / Editorial 默认，不是银河宇宙规律。

估算必须包含：

- 阅读；
- 配音；
- 动画；
- 转场；
- 用户思考；
- 互动等待；
- 错误反馈。

不能只按文字朗读速度估算。

---

# 7. 生产时长预算

建议标准分配：

```text
进入与建立情境       15–20%
问题与目标形成       15–20%
探索 / 判断 / 制作    30–40%
互动与反馈           15–20%
收束与状态变化       10–15%
```

这不是硬比例。

它的目的只是防止：

> 前4分钟都在解释世界观。

---

# 8. 单集主要学习动作

每集原则上只突出：

> **一个 primary learning action。**

例如：

- distinguish；
- verify；
- define；
- compare；
- design；
- test；
- revise；
- collaborate；
- weigh。

可以存在辅助动作。

但不能：

> 一节5分钟课同时要求用户完整经历“提问→检索→核查→设计→生成→迭代→伦理判断”。

那是多个课程。

---

# 9. 知识优先级必须被尊重

Story Profile 中：

```text
must_understand
should_recognize
first_exposure_only
```

三个层级必须影响叙事密度。

## must_understand

必须通过：

> 行动、选择、解释或作品

形成可观察学习证据。

## should_recognize

应该知道：

> “它是什么 / 它在哪个位置”。

但不要求熟练。

## first_exposure_only

可以：

> 看见、听见、留下印象。

不能马上考试。

---

# 10. Story Beats 的职责

Story Beats 不是儿童文案。

它负责回答：

- 发生了什么；
- 为什么下一件事发生；
- 谁在场；
- 用户何时行动；
- 哪些状态发生变化。

禁止把：

```text
发现异常
查看材料
进行判断
修正答案
```

直接作为 Reader 成品。

---

# 11. Narrative Rendering

Story Beats 必须经过 Narrative Rendering 才能成为成品。

Narrative Rendering 至少补齐：

- Environment；
- Action；
- Reaction；
- Connection；
- Dialogue；
- Silence；
- User Context。

原则：

> **后台越结构化，前台越自然。**

---

# 12. Scene Completeness

每个新场景首次进入时，必须让用户自然获得：

```text
WHERE
WHO
WHAT
WHY
```

即：

- 在哪里；
- 谁在；
- 正发生什么；
- 为什么值得关注。

不必写说明书。

可以由：

- 画面；
- 动作；
- 对话；
- UI信息；
- 声音

共同承担。

---

# 13. 场景之间必须存在因果连接

禁止：

> 用户刚选完一个答案，下一页突然换到另一颗星球开始新知识点。

每次转场至少要能回答：

> **上一件事为什么导致下一件事？**

如果回答不出来：

> 说明场景只是被课程目录串在一起。

---

# 14. Opening Mode

允许使用：

- arrival
- quiet_discovery
- daily_life
- conversation
- incoming_message
- mission_request
- scientific_observation
- relationship_event
- found_object
- ongoing_event
- memory
- user_creation_trigger
- travel_scene
- celebration
- mystery

禁止把：

- alarm
- ship_damage
- emergency

当作默认开场。

---

# 15. “冷开场”不再是硬规则

v0.1 曾推荐每集从“不寻常事件”进入。

v0.2 改为：

> **尽快建立一个值得继续看的状态。**

这个状态可以是：

- 异常；
- 安静发现；
- 日常关系；
- 一个问题；
- 一个真实委托；
- 一段旅行；
- 一件很小但让人在意的事情。

不是所有故事都需要警报器。

---

# 16. Story Grammar

推荐使用组合语法：

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

例如：

```text
未知信号
×
判断来源
×
证据不足
×
Toki / Konti 看法不同
×
核查
×
设备存在偏差
×
结论仍保持未知
```

Story Grammar 是生成约束。

不是儿童可见文本。

---

# 17. 旧“叙事机制库”的处理

v0.1 的以下机制继续保留：

- 星际档案；
- 信号调查；
- 异星委托；
- 失控实验；
- 观测谜案；
- 原型工坊；
- 星际来信；
- 远航议事。

但正式降级为：

> **Narrative Seeds / 叙事种子。**

它们不是：

> 八套固定模板。

生产系统不得根据课程标签机械映射：

```text
事实核查 → 星际档案
原型 → 原型工坊
伦理 → 远航议事
```

可以使用，也可以不用。

---

# 18. 叙事种子使用原则

同一个机制不能：

> 连续生成高度相似剧情。

例如三次“信号调查”必须至少改变：

- Trigger；
- 关系；
- 环境；
- 信息结构；
- 用户动作；
- 结果类型。

否则：

> 机制只是换皮模板。

---

# 19. 世界观使用原则

世界观负责：

> 让事件发生在一个真实存在的地方。

不是负责：

> 把课程知识重新命名成宇宙名词。

禁止：

```text
Prompt → 提示星晶
Context → 上下文能量带
幻觉 → 幻觉风暴
Agent → 智能体机器人星
```

除非 World Canon 本身真的存在该对象。

---

# 20. 三层分离继续保留

每集仍必须区分：

## 银河剧情层

人物、地点、关系、历史、事件。

## 案例任务层

现实生活、校园、家庭、创作、社会或银河居民真实面对的问题。

## 知识行动层

用户本课要真正练习的认知或AI行动。

三层：

> 可以互相影响。

不能：

> 互相吞没。

---

# 21. 现实案例接入

现实案例进入银河时优先使用：

- 档案；
- 现实通信；
- 委托；
- 模拟；
- 用户回忆；
- 现实航线材料；
- 晶老师传来的资料。

但不要：

> 为了一个家庭作业问题，创造一个“家庭作业星球”。

现实语言应保持：

> 正常、现代、易懂。

---

# 22. 知识进入故事的优先级

优先：

## 1. Rule

知识决定世界/任务如何运行。

## 2. Evidence

知识成为判断证据。

## 3. Tool

知识帮助用户行动。

## 4. Consequence

错误理解产生具体后果。

## 5. Reflection

行动后才总结。

最低优先级：

## 6. Direct Explanation

人物直接讲定义。

定义不是禁用。

只是：

> 不应成为主生产机制。

---

# 23. 教育解释的长度

解释必须：

> **够理解，不求一次讲完。**

特别是9–12岁：

不要因为担心他们不懂：

> 把每个词都解释三遍。

也不要因为追求简洁：

> 删掉理解所需的前因后果。

原则：

> **删冗余，不删理解。**

---

# 24. Deep Note

Deep Note 的目的：

> 让孩子意识到世界比当前课程大。

规则：

1. 必须来自批准 Reference Asset；
2. 不是本课核心考点；
3. 通常1–2句话；
4. 不立即考试；
5. 不要求全部理解；
6. 频率低于普通知识。

---

# 25. Deep Note 不是“彩蛋百科”

禁止：

> 每集结尾固定弹一个冷知识。

否则：

> 稀缺性和神秘感迅速消失。

---

# 26. 角色出场原则

角色出场必须同时考虑：

- Continuity；
- Location；
- Story Need；
- Relationship；
- Knowledge Access。

不能只依据：

> “谁最适合讲这个知识”。

---

# 27. 主角色数量

当前 Runtime 默认：

> **每集1–2名主要对白角色。**

这是：

> Runtime / Editorial Rule。

不是：

> 银河中第三个人不能说话。

其他人物可以：

- 短暂出现；
- 留言；
- 投影；
- 被提及；
- 出现在背景生活中。

---

# 28. 角色首先是人

每句角色对白优先问：

> **TA此刻为什么要说这句话？**

其次才问：

> 能不能顺便承担知识功能。

如果一句话唯一存在理由是：

> “课程需要有人解释这一页”，

优先重写。

---

# 29. 台词长度

v0.1 的：

> “单次台词优先1–2句”

继续保留为：

> **防止长篇讲课的软规则。**

不能理解成：

> 角色每次只能蹦几个字。

允许：

- 连续两三句；
- 停顿；
- 补充；
- 自我修正；
- 打断；
- 犹豫；
- 玩笑；
- 说半句。

标准：

> **像人在说话。**

---

# 30. Character Voice

所有对白必须经过：

> Character Voice QA。

尤其禁止：

> 四人可以互换台词而不影响感觉。

如果：

> 拓奇、康缇、普罗、爱今说同一句话都合理，

说明人物声音不够明确。

---

# 31. 拓奇规则

拓奇可以：

- 提出奇怪问题；
- 快速联想；
- 开多个方向；
- 好奇；
- 兴奋。

但不能：

- 反复装笨；
- 每课犯低级错误；
- 只负责“哇”；
- 永远等待用户教育他。

他的错误应该：

> 来自真的性格盲点。

例如：

> 跳得太快，没核实。

---

# 32. 康缇规则

康缇可以：

- 注意证据；
- 观察；
- 怀疑；
- 保留未知；
- 补上下文。

但不能：

- 成为数据库；
- 什么都知道；
- 每次只说“先核查”；
- 把所有课程变成侦探课。

她也必须：

> 查资料、犯判断错误、改变看法。

---

# 33. 普罗规则

普罗可以：

- 做；
- 测；
- 改；
- 拆解；
- 建原型。

但不能：

- 所有问题都做成装置；
- 一遇问题就修飞船；
- 一键成功；
- 成为“程序员NPC”。

他的原型：

> 应该真的可能失败。

---

# 34. 爱今规则

爱今可以：

- 看见参与者；
- 权衡影响；
- 讨论责任；
- 推动共同决定。

但不能：

- 成为伦理按钮；
- 每次安全问题自动召唤；
- 永远正确；
- 说教式队长；
- 把开放问题变成唯一标准答案。

她必须：

> 也可能承担过度、判断过快、替别人做决定。

---

# 35. 晶老师规则

晶老师不是：

- 每课主持；
- 全知教师；
- 权威答案机。

他可以：

- 忙乱；
- 想太远；
- 过度推演；
- 重新核查；
- 承认不知道；
- 被用户拉回当前问题。

他的AI知识应该表现为：

> 真正使用、验证、连接知识。

而不是：

> 把术语念给用户。

---

# 36. 用户是主体

任何一集都必须能够回答：

> **如果用户不在，这件事会有什么不同？**

如果答案是：

> “没有区别，伙伴会自己完成所有事情。”

则：

> 用户没有真正成为主角。

---

# 37. 用户行动

每集至少需要一个：

> 有意义的用户动作。

例如：

- 判断；
- 选择；
- 核查；
- 分类；
- 写；
- 设计；
- 生成；
- 修改；
- 测试；
- 权衡。

不能把：

> “点击下一页”

算作 Agency。

---

# 38. 互动数量

当前 Runtime 默认：

> **1次核心互动 + 最多1次轻量互动。**

如果课程确实需要更多：

> 由 Runtime Config 显式允许。

禁止：

> 一页一个题。

---

# 39. 互动前必须保留上下文

用户进入选择时，必须能快速看到或回看：

- 当前目标；
- 关键材料；
- 选择对象；
- 成功条件。

不能让用户：

> 先读三屏材料，然后第四屏靠记忆答题。

---

# 40. Meaningful Choice

选择至少影响一项：

- 当前状态；
- 反馈；
- 后果；
- 角色回应；
- Route Continuity；
- Personal Relationship State；
- User Artifact。

主线可以汇合。

但：

> 选择不能消失。

---

# 41. Fake Choice

如果三个选项只得到：

> “很好，我们继续。”

则属于：

> **Fake Choice。**

需要：

- 改成真实选择；
- 或删除互动。

假互动比：

> 没互动

更差。

---

# 42. 错误反馈

用户犯错后：

先展示：

> **为什么这个选择在当前情境下不合适。**

再允许：

> 低压力重试。

禁止：

- 羞辱；
- “你错了！”式红叉；
- 角色嘲笑；
- 巨大惩罚；
- 普通错误导致世界毁灭。

---

# 43. 后果

错误后果应：

> 具体、合理、可恢复。

例如：

AI信息没有核查：

> 白走六分钟。

比：

> 因为核查错一道题，银河导航系统全面崩溃

更可信。

---

# 44. Failure is Story

失败不只是：

> 题目状态。

它可以改变：

- 行动路径；
- 角色对用户的认识；
- 用户作品；
- 时间；
- 小范围世界状态。

这样失败才真正属于故事。

---

# 45. 用户作品

能留下作品的课程：

> 优先让用户留下作品。

包括：

- Prompt；
- Problem Card；
- Verification Record；
- Bug Report；
- 原型；
- 图像；
- 文本；
- 决策说明；
- 航行日志。

长期学习证据：

> 作品通常比“答对一次”更有价值。

---

# 46. 奖励原则

不要用：

> 空数值

代替学习结果。

特别禁止：

- 星粒=积分；
- 糖能=经验值；
- 每课固定+50；
- 虚假资产持久化。

奖励优先：

- 状态改变；
- 新关系记忆；
- 用户作品；
- 航行日志；
- 地图发现；
- 一个真正可用的工具；
- 角色反馈。

---

# 47. 持久化必须诚实

如果当前产品不能永久保存：

> 不得写“已永久收藏”。

如果只是当前集展示：

> 就写“本次记录”。

Production Spec 必须尊重 Runtime。

---

# 48. 地点使用原则

优先：

> **复用已存在的长期地点。**

不要每节课创建：

> 新星球、新空间站、新文明。

新地点应至少满足：

- 世界生活需要；
- 人物人生需要；
- 历史需要；
- 长期故事需要；
- 未来可复用。

---

# 49. 同一地点必须允许不同故事

观测站不能只发生：

> 核查课。

工坊不能只发生：

> 原型课。

公共空间不能只发生：

> 伦理课。

否则地点不是世界地点。

只是：

> 课程按钮。

---

# 50. 新世界事实限制

Writer Agent 可以自由创造：

### Low-risk C3 Decoration

例如：

- 一杯饮料；
- 一次天气；
- 临时无名路人；
- 小物件；
- 小笑话。

但不得未经批准创造：

- 大型历史事件；
- 新种族历史；
- 新政权；
- 重要长期地点；
- 角色家庭史；
- 关键世界物理规律。

---

# 51. 旅行原则

旅行需要时间。

但不需要每次完整表现。

可以：

- 直接转场；
- 用一句航行说明；
- 在路上发生对话。

不能：

> 角色无理由从另一个星域瞬移。

---

# 52. “路上的时间”值得保留

不是所有有效剧情都发生在任务现场。

旅行途中可以：

- 聊天；
- 看窗外；
- 沉默；
- 遇见普通居民；
- 回忆；
- 谈与课程无关的小事。

这类片段：

> 是人物长期可信度的重要来源。

---

# 53. 世界 exposition

后台 World Canon 可以很厚。

前台：

> **尽量少解释。**

只解释：

> 当前行动真正需要理解的部分。

第一集可以完全不知道：

- 大通航家；
- 星粒完整机制；
- 四星域历史；
- 糖基生命起源。

世界不是靠：

> 第一集讲完

成立的。

---

# 54. Mystery

世界谜团出现时：

> 不要求立刻给答案。

允许：

- 不确定；
- 暂无数据；
- 多个假说；
- 当前不知道。

禁止：

> 每一个悬念都在同集最后揭晓。

---

# 55. 现实科学与银河虚构

现实科学：

> 必须使用批准 Reference Asset。

银河虚构：

> 必须来自 Canon。

不能：

> 用银河设定解释现实科学。

也不能：

> 把现实假说写成确认事实。

---

# 56. Reference Use

高风险知识包括：

- AI事实；
- 科学；
- 历史；
- 儿童安全；
- 真实人物；
- 社会制度。

如果 Approved Reference 不够：

```text
STOP FACT GENERATION
→ Research Request
→ Verification
→ Approve Asset
→ Resume Production
```

不能让 Writer Agent：

> 自己“凭常识补一段”。

---

# 57. Readability 基础规则

儿童可见内容禁止长期使用：

- 名词堆叠；
- 关键词推进；
- 内部字段；
- 过短碎句；
- PPT式列表；
- 定义连续轰炸。

要让用户：

> 读得懂当前正在发生什么。

---

# 58. 年龄跨度

目标：

> **9岁能跟上，16岁不觉得幼稚。**

实现方式不是：

> 所有句子都变简单。

而是：

- 情境清楚；
- 真实完整句；
- 专业词有上下文；
- 不过度卖萌；
- 不把复杂思想变成幼儿话。

---

# 59. 语言风格

默认：

> **现代、自然、清楚、克制、有想象力。**

避免：

- 教材腔；
- 主持人腔；
- 营销文案；
- 过度文学；
- 过度卖萌；
- 全员金句；
- 大量感叹号。

---

# 60. 对话原则

角色首先：

> 回应现场。

其次：

> 传递信息。

允许：

- 不完整句；
- 打断；
- 停顿；
- 犹豫；
- 误解；
- 改口；
- 笑话；
- 沉默。

禁止：

> “根据现有证据，我们需要进行上下文分析。”

除非这个人物真的会这样说。

---

# 61. Explanation Bridge

从故事进入学习动作以前：

> 必须存在理解桥梁。

例如：

AI说错信息：

1. 用户看到现实冲突；
2. 角色产生反应；
3. 用户知道哪里值得重新看；
4. 才进入核查互动。

不能：

> 故事发生 → 突然答题。

---

# 62. 情绪

银河允许：

- 好奇；
- 开心；
- 无聊；
- 紧张；
- 失望；
- 尴尬；
- 犹豫；
- 惊奇；
- 感动；
- 小冲突。

不要：

> 每集情绪都是“兴奋”。

---

# 63. 关系钩子优于危机钩子

尾声钩子优先：

- 人物关系；
- 一个未完成的问题；
- 地点记忆；
- 下一次自然相遇。

少用：

> “突然，一个巨大警报响起……”

长期留存不应该主要靠：

> 人为悬崖。

---

# 64. 情感安全

角色可以：

> 喜欢用户、期待再次见面。

但禁止：

- “只有你理解我”；
- “你不来我会难过”；
- 要求排他关系；
- 模拟依赖；
- 用愧疚提升留存。

目标：

> 温暖关系，不制造情感控制。

---

# 65. 视觉生产原则

本文件不替代 Visual Bible。

但视觉必须遵循：

- 优先使用已有IP资产；
- 角色保持身份一致；
- 世界地点保持长期一致；
- 不默认使用泛机器人；
- 视觉奇观服务场景，不替代叙事。

---

# 66. 视觉连续性

同一个固定地点重复出现：

必须保持：

- 主要空间结构；
- 关键标志物；
- 导航关系；
- 已发生变化。

不能：

> 每次文生图都重新生成一座完全不同的观测站。

---

# 67. UI 与世界的边界

UI可以显示：

- 当前任务；
- 材料；
- 选择；
- 状态；
- 日志。

但 UI 术语：

> 不自动变成世界内词汇。

例如：

`relationship_state`

不会显示给孩子：

> “你与拓奇关系值+10”。

---

# 68. Audio / Voice

配音和声音应：

- 区分角色；
- 保留自然停顿；
- 不把所有句子读成教学旁白；
- 避免过度卡通化。

重要环境地点应拥有：

> 可重复识别的声音特征。

---

# 69. 首集/首次出现特殊规则

第一次出现一个重要对象时：

> 不要求解释完整。

只需要：

> 让用户知道当前必须知道的那部分。

例如：

第一次见拓奇：

> “糖基生命。”

即可。

不需要：

> 弹出糖基生命百科。

---

# 70. 首次角色出现

第一次正式见主伙伴时至少保证：

- TA正在做自己的事；
- 不是站着等待教学；
- 用户看到TA的一点真实性格；
- TA与用户关系有真实起点。

禁止：

> “我是康缇，我的能力是上下文分析。”

---

# 71. 角色知识暴露

角色不能因为：

> Character Bible里写了秘密

就自己说出来。

Writer Agent只能使用：

> Story Profile明确允许的 `known_information / relevant_history_context`。

---

# 72. 长期剧情比例建议

继续采用柔性：

> **30 / 60 / 10**

约：

- 30% 轻量过去与关系；
- 60% 当前单集独立成立；
- 10% 长期未知或未来线索。

不是硬KPI。

---

# 73. 长期线程

前期同时活跃长期线程：

> 建议 ≤ 3。

避免：

- 用户记不住；
- Agent失忆；
- 每集都在铺伏笔；
- 课程被连续剧绑架。

---

# 74. Episode Autonomy

每一课必须：

> 即使用户隔一段时间回来，也基本能理解当前事件。

连续学习的用户：

> 会多看见关系、历史和伏笔。

这就是：

> Macro Continuity + Episode Autonomy。

---

# 75. Production Pipeline

正式推荐：

```text
1. Load Course
2. Assemble Context
3. Build Story Profile
4. Preflight Gates
5. Generate Story Beats
6. Editorial Beat Review
7. Narrative Rendering
8. Interaction Integration
9. Runtime Adaptation
10. Visual / Audio Direction
11. Post-generation QA
12. Delta Extraction
13. Human Review
14. Release
15. State Writeback
```

---

# 76. Step 1｜Load Course

禁止 Writer Agent：

> 根据课名猜课程。

必须读取唯一 Course Source of Truth。

至少获得：

- learning_goal；
- knowledge_points；
- competency；
- lesson evidence。

---

# 77. Step 2｜Assemble Context

ContextAssembler 提供：

- relevant world canon；
- eligible characters；
- relationship state；
- location state；
- timeline；
- approved references；
- runtime constraints。

Writer 不直接：

> 搜整库拼Prompt。

---

# 78. Step 3｜Build Story Profile

Story Profile 未通过前：

> 不生成正式剧本。

故事再好看：

> 如果课程目标或角色资格错误，也应退回。

---

# 79. Step 4｜Preflight Gates

任何 BLOCK：

> 停止生产。

WARN：

> 进入人工审查队列。

不得：

> “先生成看看再说”。

---

# 80. Step 5｜Story Beats

Story Beats 要先验证：

- 故事是否成立；
- 因果是否成立；
- 用户是否有真实作用；
- 知识是否自然进入。

Beat阶段：

> 修改成本最低。

不要直接用长脚本修结构。

---

# 81. Step 6｜Beat Review

人工或编辑Agent至少问：

1. 去掉知识术语，故事还成立吗？
2. 去掉银河皮肤，问题本身还真实存在吗？
3. 用户是否真正推动结果？
4. 角色是否有自己的理由？
5. 是否又掉回旧套路？

不通过：

> 回 Story Beats。

---

# 82. Step 7｜Narrative Rendering

只有 Beat 通过以后：

> 才写 Child-facing Script。

这个步骤重点负责：

- 场景；
- 文笔；
- 人物；
- 语言；
- 情绪；
- 节奏。

不能：

> 一边编结构一边最终写作。

---

# 83. Step 8｜Interaction Integration

把互动插回故事时检查：

> 互动是不是自然发生的。

如果删掉UI按钮以后：

> 故事突然断掉，

说明互动可能只是：

> 课程题目硬塞进剧情。

---

# 84. Step 9｜Runtime Adaptation

根据当前 Runtime：

- 页面数量；
- 支持互动；
- 动画能力；
- 字数；
- 语音；
- 资产持久化

转换成可实现形式。

这一步可以：

> 简化表现。

不能：

> 修改课程与Canon。

---

# 85. Step 10｜Visual / Audio Direction

输出：

- scene visual need；
- existing asset reference；
- new asset request；
- character expression；
- location continuity note；
- sound cue。

不需要在 Story Script 里：

> 重复写完整视觉Prompt。

---

# 86. Step 11｜Post-generation QA

至少执行：

```text
Course QA
Canon QA
Continuity QA
Epistemic QA
Character Voice QA
Readability QA
Agency QA
Runtime QA
Safety QA
Delta QA
```

---

# 87. Course QA

检查：

- 是否仍然在教本课；
- 是否增加了不存在的必修目标；
- 是否把 first_exposure 当成 mastery；
- 是否真正产生学习证据。

---

# 88. Canon QA

检查：

- 新世界事实；
- 新地点；
- 历史；
- 物种；
- 技术；
- 世界规则。

未授权C1/C2：

> BLOCK。

---

# 89. Continuity QA

检查：

- 人物是否认识；
- 地点状态；
- 上一集结果；
- 时间；
- 已发生选择；
- 长期线程。

---

# 90. Epistemic QA

重点问：

> **这个角色为什么知道这件事？**

回答不了：

> BLOCK / REVISE。

---

# 91. Character Voice QA

检查：

- 台词是否可互换；
- 是否功能化；
- 是否突然变老师；
- 是否符合当前关系状态；
- 是否使用角色不知道的信息。

---

# 92. Readability QA

至少检查：

1. 用户知道在哪里吗？
2. 知道谁在做什么吗？
3. 知道为什么继续吗？
4. 是否跳步？
5. 是否出现内部语言？
6. 是否过度碎句？
7. 是否连续定义？
8. 互动前上下文够吗？
9. 9岁能大体跟上吗？
10. 16岁会不会明显觉得低幼？

---

# 93. Agency QA

检查：

- 用户是否真的影响体验；
- 是否只有假选择；
- 是否伙伴已经替用户完成关键推理；
- 用户作品是否由Agent一键代做。

---

# 94. Runtime QA

检查：

- 5–8分钟；
- 交互数；
- 当前支持的操作；
- 当前可用资产；
- 当前持久化能力。

超出：

> 返回 Runtime Adaptation。

---

# 95. Delta QA

生成后提取：

- relationship changes；
- location changes；
- new facts；
- new entities；
- artifacts；
- threads。

与 Story Profile 比较。

任何未经授权变化：

> 必须修订或进入 Proposal。

---

# 96. Release Checklist

发布前建议统一检查：

- [ ] Story Profile 已批准
- [ ] Course Gate PASS
- [ ] Canon Gate PASS
- [ ] Character Gate PASS
- [ ] Continuity Gate PASS
- [ ] Reference Gate PASS
- [ ] Safety Gate PASS
- [ ] Runtime Gate PASS
- [ ] 一个主要学习动作清楚
- [ ] 用户任务一句话能讲清
- [ ] 角色有真实出场理由
- [ ] 没有默认故障开场
- [ ] 没有角色装笨
- [ ] 没有知识宇宙化换皮
- [ ] 核心互动上下文完整
- [ ] 错误后果具体、可恢复
- [ ] 用户选择留下状态
- [ ] Reader不是内部字段
- [ ] 世界设定无未授权扩张
- [ ] Episode Delta 已抽取
- [ ] 状态写回范围明确

---

# 97. 禁用套路 Registry 建议

从文档正文迁移到可配置 Registry。

第一批：

```text
TROPE-SHIP-DAMAGE-DEFAULT
TROPE-ENERGY-SHORTAGE-DEFAULT
TROPE-SYSTEM-ALARM-DEFAULT
TROPE-TOKI-PLAYS-DUMB
TROPE-CHARACTER-PRAISE-LOOP
TROPE-KNOWLEDGE-RESKINNING
TROPE-ONE-PAGE-ONE-QUIZ
TROPE-EMPTY-POINT-REWARD
TROPE-MANDATORY-CRISIS
TROPE-AJI-ETHICS-CALLBACK
TROPE-PROTI-REPAIR-EVERYTHING
TROPE-KONTI-KNOWS-EVERYTHING
TROPE-FIXED-REGION-ROTATION
TROPE-FIXED-CHARACTER-ROTATION
```

这样：

> QA可以机器化检查。

---

# 98. v0.1 中废弃或迁移的内容

以下内容不再由 Production Spec 持有：

## 迁移到 World Canon

- 银河母设定；
- 四星域本体；
- 星粒/糖能等世界定义。

## 迁移到 Character Canon

- 四人完整人物卡；
- 关系历史；
- 台词声音。

## 迁移到 Story Schema

- Story Profile字段；
- Persistence；
- Context Snapshot；
- Episode Delta；
- Character Eligibility。

## 迁移到 Runtime Config

- 主要角色上限；
- 互动上限；
- 当前资产能力；
- 支持的互动类型。

Production Spec 只引用它们。

---

# 99. v0.1 中继续有效的核心遗产

继续保留：

1. 线性主线 + 少量关键决策；
2. 不追求复杂RPG；
3. 游戏感来自目标、选择、结果、关系，而不是积分；
4. 用户是行动者；
5. AI不是万能魔法；
6. 错误不是角色装笨；
7. 生活案例保持现实感；
8. 一集一个主要学习动作；
9. 1次核心互动 + 少量轻互动；
10. 错误有具体可恢复后果；
11. 禁止飞船故障默认化；
12. 禁止知识点星际换皮；
13. 禁止一页一题；
14. 奖励必须诚实；
15. 生产前需要清单审查。

---

# 100. 当前 Runtime 默认值

在正式 Runtime Config 文件建立前，v0.2 暂时引用以下默认：

```yaml
runtime_defaults:
  target_duration_minutes:
    min: 5
    max: 8

  core_interactions:
    default: 1
    max_without_override: 2

  primary_dialogue_characters:
    max: 2

  complex_branching:
    enabled: false

  persistent_inventory:
    enabled: false
```

这些未来：

> 应从Production Spec移出，完全由Runtime Config提供。

---

# 101. Agent 分工建议

## Story Planner Agent

输入：

> Story Profile

输出：

> Story Beats

不得：

> 改Canon。

## Narrative Writer Agent

输入：

> Approved Story Beats + Character Voice + Rendering Rules

输出：

> Script / Reader

不得：

> 新建C1/C2设定。

## Interaction Agent

负责：

> 互动形式与反馈。

不得：

> 改课程目标。

## Visual Direction Agent

负责：

> 镜头、场景、素材需求。

不得：

> 重设计角色身份。

## QA Agent

负责：

> 发现问题。

没有：

> 自动批准Canon权限。

---

# 102. 人工必须保留的判断

即使大量QA自动化，以下仍应人工审核：

- 好不好看；
- 人物像不像人；
- 情绪是否自然；
- 幽默是否有效；
- 是否低幼；
- 是否值得继续；
- 是否让孩子真正产生好奇。

系统最擅长防止：

> 错。

人仍然负责判断：

> **好不好。**

---

# 103. Definition of Done

一集不是因为：

> AI生成完了

就完成。

正式 Definition of Done：

```text
Course correct
+
Canon valid
+
Continuity valid
+
Character believable
+
User agency real
+
Narrative readable
+
Runtime feasible
+
Safety pass
+
Delta extracted
+
Human approved
```

全部成立：

> 才能 RELEASE。

---

# 104. v0.2 冻结原则

1. **Production Spec只管生产，不重新定义Canon。**
2. **Story Profile批准后再进入正式写作。**
3. **先做Story Beats，再做Narrative Rendering。**
4. **儿童成品不得直接暴露后台字段。**
5. **每集突出一个主要学习动作。**
6. **知识最好成为规则、证据、工具或后果，而不是讲解。**
7. **角色出场需要剧情理由，不按知识标签自动路由。**
8. **互动必须有意义，选择即使汇合也要留下记忆。**
9. **错误要具体、合理、可恢复。**
10. **地点、角色、历史必须保持连续。**
11. **现实科学使用Approved Reference，银河虚构使用Canon。**
12. **5–8分钟是Runtime目标，不是世界规则。**
13. **禁止默认故障、装笨、空奖励、知识换皮。**
14. **生成后必须做Delta与Canon Mutation检查。**
15. **人负责最终判断“好不好”，系统负责尽量保证“不乱”。**

---

# 105. 最终生产定义

银河单集生产不是：

> **把一节教案改写成一段太空故事。**

而是：

> **在一个持续存在的世界里，让一个真实人物与用户共同经历一件值得发生的事；课程知识因为这件事而变得有用，用户的行动产生结果，而故事结束以后，世界、关系或用户自己留下可以继续记住的痕迹。**

如果一集做到了这一点：

> 它才属于《银河》。
