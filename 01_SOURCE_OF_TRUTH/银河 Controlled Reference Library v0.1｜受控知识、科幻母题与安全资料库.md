# 银河 Controlled Reference Library v0.1
## 受控知识、科幻母题与安全资料库

**版本：v0.1**  
**日期：2026-09-11**  
**状态：RECONSTRUCTED BASELINE / SOURCE-OF-TRUTH CANDIDATE**  
**说明：本文件依据本轮已经锁定的设计记录、后续 Story Schema / Production Spec / Engineering Handoff 中已引用的规则，以及既有课程知识治理原则重建。原始独立文件未能从 Library 或当前对话文件中找回。若未来发现原稿，应先做 diff，再决定是否替换本版。**

---

# 0. 文档定位

Controlled Reference Library 不是：

- 课程教材；
- 世界圣经；
- 科幻百科；
- Writer Agent 自由检索库；
- 网络素材收藏夹。

它是银河剧情系统用于约束：

> **现实知识、科学事实、科学假说、开放问题、科幻叙事母题、视觉素材与儿童安全边界**

的一套受控资料层。

它的核心原则是：

> **约束事实，释放表达。**

即：

- 事实必须受来源、状态和时效约束；
- 叙事表达可以有创造性；
- 科幻设定可以大胆；
- 但不能把假说说成事实；
- 不能把银河虚构反向包装成现实科学。

---

# 1. 为什么需要独立 Reference Library

银河同时处理：

- AI知识；
- 天文与宇宙科学；
- 生命与文明；
- 科幻母题；
- 儿童教育；
- 安全与伦理；
- 视觉素材。

如果这些信息直接由 Writer Agent 临时上网、凭模型记忆或自由补全，容易出现：

- 科学事实过时；
- 假说与事实混淆；
- NASA图片版权状态不明；
- “可居住”被写成“存在生命”；
- 未知信号被写成“外星文明已确认”；
- 现实科学与银河世界设定混写；
- 面向未成年人的风险边界不稳定。

因此必须建立：

```text
Research
→ Verify
→ Import
→ Approve
→ Generate
```

而不是：

```text
Generate
→ 再看看有没有说错
```

---

# 2. 与其他 Source of Truth 的边界

## World Canon

回答：

> 银河世界里什么是真的。

## Course Source

回答：

> 课程要教什么。

## Controlled Reference Library

回答：

> 现实世界哪些知识、事实、假说和外部资料允许被银河引用，以及应该怎样引用。

## Narrative Production Spec

回答：

> 怎样把这些材料写进故事。

---

# 3. Truth Domain

所有 Reference Asset 必须先标记所属真值域。

```text
REAL_WORLD_FACT
FICTIONAL_WORLD_FACT
METAPHOR
CREATOR_MODEL
```

Reference Library 的主要对象是：

> `REAL_WORLD_FACT`

但也可以登记：

- 现实科学假说；
- 开放问题；
- 叙事母题；
- 安全规则；
- 视觉素材许可。

---

# 4. Truth Label

统一采用：

```text
FACT
ESTIMATE
HYPOTHESIS
OPEN_QUESTION
FICTION
METAPHOR
NARRATIVE_TYPE
SAFETY_RULE
```

其中：

## FACT

有足够可靠来源支持。

## ESTIMATE

基于模型、测量或统计估计。

## HYPOTHESIS

合理但未确认。

## OPEN_QUESTION

当前科学仍没有可靠答案。

## FICTION

银河或其他明确虚构设定。

## METAPHOR

用于解释，但不是字面科学事实。

## NARRATIVE_TYPE

科幻史、文学与影视中反复出现的叙事母题类型。

## SAFETY_RULE

儿童、AI使用、隐私、行为等安全约束。

---

# 5. 核心来源等级

## Tier S｜最高优先

用于核心现实事实与安全边界。

建议包括：

- NASA
- NASA JPL
- NASA Astrobiology
- ESA
- SETI Institute
- IAA / 相关正式国际航天与天文机构
- UNESCO
- UNICEF
- 中国国家级正式政策、科研机构与教育主管部门

---

# 6. Tier A

适用于：

- 高质量大学；
- 同行评议论文；
- 正式科学机构；
- 标准组织；
- 高质量专业百科；
- Science Fiction Encyclopedia（SFE）等专业科幻研究资料。

特别说明：

> **SFE 只用于叙事类型、科幻母题与历史分类，不用于证明自然科学事实。**

---

# 7. 不作为核心事实来源

以下不得作为高风险事实的唯一依据：

- 营销文章；
- 未署名自媒体；
- 二次转载；
- 无来源短视频；
- Reddit；
- 知乎；
- 粉丝Wiki；
- AI生成答案；
- 影视作品本身。

它们可以用于：

> 发现问题、了解流行表达、寻找线索。

不能直接：

> 进入 VERIFIED FACT。

---

# 8. Reference Asset 最小字段

建议工程 Registry 使用：

```yaml
reference_asset:
  asset_id:

  title:
  topic:

  source_org:
  source_url:
  source_tier:

  truth_domain:
  truth_label:

  core_fact:

  what_it_does_not_prove:

  age_band:
    min:
    max:

  story_use_mode:
    RULE | EVIDENCE | TOOL | DEEP_NOTE | BACKGROUND_ONLY

  preferred_voice:

  verification_status:
    VERIFIED | REVIEW_REQUIRED | DEPRECATED

  last_verified:
  freshness_policy:
  review_due:

  copyright_status:
  visual_license_status:

  notes:
```

---

# 9. `what_it_does_not_prove`

这是 Reference Library 的重要字段。

例如：

```yaml
core_fact:
  "某颗行星位于恒星的可居住带"

what_it_does_not_prove:
  - "存在生命"
  - "适合人类居住"
  - "存在文明"
```

目的是防止：

> Agent 把“一个事实”顺手扩写成“更大的结论”。

---

# 10. 科学叙事的黄金认知转折

以下可以作为银河长期使用的科学思维母题。

---

## 10.1 看不到，也能知道它存在

科学并不总依靠直接看见。

可以通过：

- 引力；
- 光谱；
- 轨道变化；
- 周期；
- 信号；
- 间接测量

推断某种存在。

教育价值：

> 证据可以是间接的，但仍然需要可靠链条。

---

## 10.2 发现线索，不等于发现生命

例如：

- 有机分子；
- 大气成分；
- 液态水可能性；
- 特殊化学信号

都可能是：

> 值得继续研究的线索。

不能直接写成：

> “已经发现生命”。

---

## 10.3 可居住，不等于有人住

Habitable：

> 通常表示某些条件可能允许生命存在。

不等于：

- 已有生命；
- 有智慧生命；
- 有文明；
- 适合人类居住。

这是极重要的科学表达边界。

---

## 10.4 冰下面可能是一整个海洋

冰卫星、地下海洋等真实科学研究非常适合银河叙事。

它们的价值不是：

> “那里一定有生命”。

而是：

> **一个看起来冰冷、封闭的地方，内部可能存在完全不同的环境。**

---

## 10.5 探索首先意味着不污染

行星保护（Planetary Protection）是银河重要的安全与伦理母题。

探索陌生世界时：

> 不仅要考虑自己能不能进去，也要考虑自己会不会破坏那里。

适用于：

- 未知生态；
- 样本采集；
- 新文明接触；
- 外部生命研究。

---

# 11. 外星生命 / ETI 信号原则

最重要表达：

> **发现 ≠ 确认 ≠ 宣布 ≠ 回复。**

收到异常信号时应区分：

```text
Detection
↓
Verification
↓
Independent Confirmation
↓
Interpretation
↓
Public Communication
↓
Response Decision
```

不得把：

> “检测到异常信号”

直接写成：

> “外星人联系我们了”。

---

# 12. Technosignatures

允许引入：

> **Technosignatures / 技术迹象**

作为真实科学研究方向。

它可以包括：

- 人工无线电信号；
- 特殊光学信号；
- 大气工业成分；
- 可能与技术活动有关的异常。

必须说明：

> 这些是寻找技术活动证据的方法，不等于已经确认文明存在。

---

# 13. 科学会保存“不知道”

银河应该允许科学角色说：

> “不知道。”

更准确的知识状态：

```text
CONFIRMED
LIKELY
POSSIBLE
HYPOTHESIS
UNKNOWN
```

开放问题不能因为剧情需要：

> 强行选一个答案。

---

# 14. Voyager Golden Record

Voyager Golden Record 可以作为优秀的真实世界 Deep Note / 文化母题。

适合讨论：

- 人类如何向未知表达自己；
- 我们选择什么代表人类；
- 信息怎样跨越语言与文明；
- 如果不知道接收者是谁，应该怎样设计信息。

避免：

> 把它写成已被外星文明收到或理解。

---

# 15. 远方的“现在”其实是过去

光传播需要时间。

因此我们观察遥远宇宙时：

> 看到的是它过去的状态。

这是非常适合青少年的 Deep Note。

可以自然引出：

- 光年；
- 观测；
- 时间；
- 信息延迟；
- “现在”在宇宙尺度上的复杂性。

不需要立即进入：

> 相对论数学。

---

# 16. 科幻母题库

Reference Library 可以保存：

> 叙事母题，不保存“可照搬剧情”。

母题只描述：

- 类型；
- 哲学问题；
- 风险；
- 适龄性；
- 可借鉴的结构。

禁止：

> 复制具体作品人物、台词、世界观或剧情。

---

# 17. 推荐母题

可以长期使用：

- First Contact / 初次接触
- Deep Time / 深时
- Generation Ship / 世代飞船
- Lost Signal / 失联信号
- Alien Ecology / 异星生态
- Nonhuman Intelligence / 非人类智能
- Relativistic Communication / 长延迟通信
- Archive / Lost Knowledge / 档案与失落知识
- Multi-species Society / 多物种共同社会
- Planetary Protection / 行星保护
- Technosignature Search / 技术迹象
- Different Perception / 不同生命的感知方式
- Translation Across Civilizations / 跨文明翻译
- Unreliable Map / 不完整地图
- Re-Appraisal of Old Knowledge / 重审旧认知

---

# 18. 高风险母题

以下不能作为默认剧情：

- 全宇宙战争；
- 灭绝威胁；
- 儿童承担银河存亡；
- 反复生死危机；
- 未知生命默认敌人；
- 外星文明默认高于或低于人类；
- AI觉醒后天然反叛；
- 所有秘密都来自古代超级文明；
- 每周发现“宇宙最大秘密”。

---

# 19. 时间旅行

当前银河 v1：

> **不开放自由时间旅行。**

可以存在：

- 历史记录；
- 模拟；
- 光传播导致的“看到过去”；
- 记忆；
- 档案；
- 历史重建。

但：

> 不允许角色为了剧情方便随意穿越过去改历史。

如果未来启用：

> 必须作为 Canon Breaking / Major World Rule。

---

# 20. Deep Note 机制

Deep Note 是：

> **超出当前课标、但真实、有趣、能让孩子感觉世界更大的知识片段。**

适合回应：

> “哪怕我现在还没完全懂，也觉得有意思。”

---

# 21. Deep Note 规则

1. 必须来自 Approved Reference Asset；
2. 必须真实或明确标注假说；
3. 通常 1–2 句话；
4. 不立即测试；
5. 不要求用户掌握；
6. 不连续高频出现；
7. 不打断主故事；
8. 不用来炫技。

原则：

> **不是为了让孩子觉得课程更难，而是让孩子知道世界比这一课更大。**

---

# 22. Deep Note 推荐表达

角色优先。

例如：

拓奇：

> “你说，会不会有一种东西，我们永远看不见，只能从它留下的变化知道它来过？”

康缇：

> “有可能。不过‘像证据’和‘足够证明’是两回事。”

不要：

> 弹出三屏百科。

---

# 23. 角色与 Reference 的关系

Reference Library 可以定义：

```yaml
preferred_voice:
  TOKI:
  KONTI:
  PROTI:
  AJI:
  JING:
```

同一个知识点：

> 可以由不同人物以不同视角触发。

但不能因为：

> 康缇适合证据

就所有科学 Reference 都只能由康缇讲。

---

# 24. 安全原则

面向9–16岁用户：

## 24.1 AI 支持 Agency

AI应该：

> 帮助用户理解、创作、判断、行动。

不能：

> 代替用户承担全部判断。

---

## 24.2 不制造依赖

禁止：

- “只有AI懂你”；
- “只有角色会一直陪你”；
- 用孤独或愧疚提高留存；
- 鼓励儿童绕过家长或教师。

---

## 24.3 不盲从AI

所有涉及：

- 事实；
- 健康；
- 安全；
- 真实人物；
- 重要决定

的场景：

> AI结果都不能被当作自动权威。

---

## 24.4 未知必须允许未知

不要为了：

> 给孩子明确答案

而把开放问题：

> 写成确定答案。

---

## 24.5 年龄适配不等于低幼化

面向儿童可以：

- 简化语言；
- 增加故事；
- 提供上下文。

但不能：

> 把复杂思想改成错误思想。

---

# 25. 未成年人信息与现实行动

涉及：

- 个人信息；
- 账号注册；
- 公开发布；
- 外部平台；
- 购买；
- 定位；
- 与陌生人联系；
- 现实实验

时：

> 必须读取 Safety / Runtime / Product Rules。

Reference Library 不能单独授权用户执行现实高风险行为。

---

# 26. Visual Reference

视觉素材也必须进入受控管理。

最少字段：

```yaml
visual_asset:
  asset_id:
  source:
  creator:
  license:
  allowed_use:
  attribution_required:
  derivative_allowed:
  age_safe:
  status:
```

---

# 27. NASA / ESA 等素材

不能因为：

> 来源是NASA

就默认：

> 所有图片都可以任意商业使用。

必须逐素材确认：

- 来源；
-版权；
-第三方署名；
-Logo；
-使用限制。

---

# 28. Writer Agent 外部资料协议

普通 Writer Agent：

> 不允许自由把网络搜索结果直接写进故事。

如果需要外部新事实：

```text
Research Request
↓
Research Agent
↓
Verify
↓
Reference Asset
↓
Approve
↓
Writer
```

---

# 29. Research Request 最小格式

```yaml
research_request:
  topic:
  story_need:
  required_truth_level:
  age_band:
  urgency:
  preferred_sources:
  prohibited_sources:
```

---

# 30. Verify Gate

引入新事实至少检查：

- 来源可信；
- 日期；
- 是否仍有效；
- 原文是否支持；
- 是否被过度推断；
- 年龄适合；
- 是否有版权问题。

---

# 31. Dynamic Knowledge

AI工具、产品能力、模型功能变化很快。

此类 Reference 必须增加：

```text
last_verified
tool_version
tested_region
account_requirement
age_requirement
review_due
```

不能把：

> 2026年某个平台功能

写成：

> 永久世界知识。

---

# 32. Stable vs Dynamic

推荐：

## Stable

- 基础科学；
- 经典AI概念；
- 科学方法；
- 安全原则。

## Evolving

- 最新AI研究；
- 新发现；
- 新政策。

## Dynamic

- 工具界面；
- 产品功能；
- 价格；
- 平台入口；
-模型名称。

三层更新频率不同。

---

# 33. Reference 与课程目标

任何新奇资料都必须服务：

> 已经存在的课程或故事价值。

禁止：

> 因为刚看到一个热点，就改变课程知识主线。

Reference Library 是：

> 资源池。

不是：

> 热点驱动课程系统。

---

# 34. Reference 与 World Canon

现实事实不能自动修改银河世界。

例如：

现实科学新发现：

> 某颗卫星存在地下海洋证据。

不能自动推出：

> 银河里的某颗星球也有同样结构。

需要：

> World Canon Proposal。

---

# 35. World Canon 与现实科学

反过来也一样。

银河世界中：

> 知识与星粒发生能量反应

属于：

```text
FICTIONAL_WORLD_FACT
```

不能通过 Reference Library：

> 包装成现实科学。

---

# 36. 典型 Reference Asset 示例

```yaml
asset_id: REF-ASTRO-001

title:
  可居住带不等于存在生命

truth_domain:
  REAL_WORLD_FACT

truth_label:
  FACT

core_fact:
  可居住带通常描述允许液态水等条件存在的可能区域之一。

what_it_does_not_prove:
  - 该行星存在生命
  - 该行星存在智慧文明
  - 该行星适合人类居住

story_use_mode:
  - EVIDENCE
  - DEEP_NOTE

verification_status:
  VERIFIED
```

---

# 37. 典型开放问题示例

```yaml
asset_id: REF-ETI-001

title:
  银河中是否存在其他智慧文明

truth_domain:
  REAL_WORLD_FACT

truth_label:
  OPEN_QUESTION

core_fact:
  当前不存在被科学共同体普遍确认的地外智慧文明证据。

story_use_mode:
  - BACKGROUND_ONLY
  - DEEP_NOTE
```

故事可以：

> 想象。

但必须清楚：

> 现实世界仍然不知道答案。

---

# 38. 典型 Narrative Type 示例

```yaml
asset_id: NARR-FIRST-CONTACT

truth_domain:
  CREATOR_MODEL

truth_label:
  NARRATIVE_TYPE

title:
  First Contact

narrative_question:
  当两个彼此不了解的文明第一次相遇，怎样建立理解、边界与信任？

risks:
  - 敌我二元化
  - 文化中心主义
  - 人类默认主导
```

---

# 39. Reference QA

生产后 QA 至少检查：

- 事实是否与 Asset 一致；
- Truth Label 是否保持；
- 是否多推了一步；
- 是否删除关键限定词；
- 是否把假说写成事实；
- 是否把“未知”写成答案。

---

# 40. Phrase Risk

以下词语使用时需要特别检查：

```text
证明
确定
一定
已经发现
首次确认
科学家认为
专家一致认为
可居住
生命迹象
外星文明
AI理解
AI知道
```

不是禁词。

但：

> 必须有足够证据支持其强度。

---

# 41. 引用方式

儿童端：

> 一般不需要显示学术脚注。

但后台 Episode 必须可追溯到：

```text
reference_asset_id
```

家长端 / 教师端 / Deep Note详情页：

> 可显示来源机构与进一步阅读。

---

# 42. 外部来源时效

建议：

```text
Stable Science: 12–24 months review
Evolving Science: 6–12 months
Policy: 3–6 months / event-driven
AI Tools: 1–3 months
Safety: event-driven + scheduled review
```

具体周期由工程与内容团队后续配置。

---

# 43. Reference Library 不负责什么

不负责：

-完整课程体系；
-角色人格；
-世界历史；
-用户状态；
-剧情结构；
-营销事实。

这些有各自 Source of Truth。

---

# 44. 工程接口

Story Schema 中：

```yaml
knowledge_contract:
  reference_asset_ids: []
```

ContextAssembler 根据：

```text
course
story need
character knowledge
age band
```

加载：

> 最小必要 Reference。

---

# 45. 不加载完整资料库

同样遵守：

> Relevant Context。

不要每集把所有天文知识：

> 塞进 Prompt。

---

# 46. Reference 权限

普通 Generator：

```text
READ approved asset
```

Research Agent：

```text
PROPOSE new asset
```

Reference Editor：

```text
VERIFY / UPDATE
```

Canon Owner / Content Owner：

```text
APPROVE high-impact asset
```

---

# 47. 旧内部层级标签说明

此前设计记录中曾出现：

```text
F / H / N / E / V / C
```

六层内部标签。

由于原始独立文件未找回：

> **本重建版不擅自恢复这六个缩写的完整英文展开。**

为避免错误，本版工程实现优先使用：

- Truth Domain；
- Truth Label；
- Story Use Mode；
- Safety；
- Visual License；
- Verification Status；

这些显式字段。

若未来发现原始版本：

> 再对六层标签做映射。

---

# 48. 当前可批准的“黄金 Reference 母题”

建议首批进入 Registry：

```text
REF-SCI-001 看不到，也能知道它存在
REF-ASTRO-001 发现线索，不等于发现生命
REF-ASTRO-002 可居住，不等于有人住
REF-ASTRO-003 冰下面可能是一整个海洋
REF-SAFE-001 探索首先意味着不污染
REF-ETI-001 发现 ≠ 确认 ≠ 宣布 ≠ 回复
REF-ETI-002 Technosignatures
REF-EPI-001 科学会保存“不知道”
REF-CULTURE-001 Voyager Golden Record
REF-ASTRO-004 远方的“现在”其实是过去
```

注意：

> 这些只是 Asset 主题ID建议，正式事实文本仍需逐条导入并验证来源。

---

# 49. 第一阶段不需要大规模建库

P0 目标：

> 让系统知道“事实不能自由生成”。

第一阶段只需要：

- 20–50条高价值 Reference；
- 几条 Deep Note；
- 关键安全规则；
- 基础来源治理。

不需要：

> 建一个宇宙百科全书。

---

# 50. v0.1 冻结原则

1. **现实事实与银河虚构必须分域。**
2. **FACT / HYPOTHESIS / OPEN_QUESTION 不得混写。**
3. **发现线索不等于确认结论。**
4. **权威来源优先。**
5. **AI生成不能替代来源核验。**
6. **动态知识必须有日期与复核机制。**
7. **Writer Agent 不直接把外部搜索结果写进正式剧情。**
8. **Deep Note 不立即考试。**
9. **未知可以长期保持未知。**
10. **科学素材服务故事与教育，但不能被故事强行改写。**
11. **视觉素材必须单独治理版权与许可。**
12. **儿童安全优先于戏剧刺激。**
13. **Reference Library约束事实，不限制创造性表达。**
14. **不为“显得高级”堆科学名词。**
15. **约束事实，释放表达。**

---

# 51. 最终定义

Controlled Reference Library 的作用不是：

> 给故事提供更多知识。

而是：

> **让银河可以大胆讲科学、未知、文明和宇宙，同时仍然知道哪里是事实、哪里是假说、哪里是虚构、哪里必须停下来承认“不知道”。**

只有这样：

> 这个世界才能既有想象力，又值得信任。
