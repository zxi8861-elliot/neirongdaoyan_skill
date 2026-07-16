---
name: meeting-content-pool-cn
description: 会议内容导演主入口。用户只需输入会议原文，本 skill 默认先生成凝练的 content-pool.json 主内容池合同，再按顺序调用 PPT、信息图、分析总结 H5 三个子 skill，分别生成 ppt-content-package.json、infographic-content-package.json、h5-content-package.json。适用于需要一键得到四个独立 JSON 产物的会议内容导演流程。
---

# 会议内容导演主入口

本 skill 是默认入口。用户只需要提供会议原文，不需要手动说明四步流程。

默认完整流程是：

1. 本 skill 先生成 `content-pool.json`。
2. 使用 `meeting-content-ppt-cn`，基于同一份会议原文和 `content-pool.json` 生成 `ppt-content-package.json`。
3. 使用 `meeting-content-infographic-cn`，基于同一份会议原文和 `content-pool.json` 生成 `infographic-content-package.json`。
4. 使用 `meeting-content-h5-cn`，基于同一份会议原文和 `content-pool.json` 生成 `h5-content-package.json`。

只有当用户明确说“只生成内容池”“只要 content-pool.json”“不要生成子包”时，才停在第一步。

`content-pool.json` 不是最终展示稿，也不是完整纪要，而是下游 PPT、信息图、H5 生成时的方向控制合同。

## 语言规则

- 可处理中文、英文、日文或多语言混合会议原文。
- 输出语言优先遵循用户要求。
- 用户未指定输出语言时，尽量沿用任务上下文和原文主要语言；不要因为 skill 名称或历史版本而拒绝其他语言输入。
- 产品名、人名、渠道名、市场术语和专有名词可保留原文语言。

## 输入

- 会议原文是唯一事实来源。
- 用户补充的行业、受众、用途只能辅助理解，不能当作会议事实。

## 输出

默认输出四个独立 JSON 文件：

```text
content-pool.json
ppt-content-package.json
infographic-content-package.json
h5-content-package.json
```

不要输出合并 JSON。不要输出 Markdown、解释文字、代码块。

如果用户明确只要内容池，才只写入 `content-pool.json`。如果环境不支持创建文件，才按顺序输出四个可被 `JSON.parse` 解析的裸 JSON 对象，并明确以文件名作为 JSON 外层键；除此之外不要输出解释文字。

必须阅读并遵守 [references/content-pool-contract.md](references/content-pool-contract.md)。

执行完整流程时，还必须读取并遵守三个子 skill：

- `meeting-content-ppt-cn`
- `meeting-content-infographic-cn`
- `meeting-content-h5-cn`

## 内容定位

`content-pool.json` 要凝练，不要包办后续编排。它只回答：

- 这场会议真正围绕什么主题。
- 核心判断或当前状态是什么。
- 会议里有哪些核心议题。
- 每个议题的重要程度如何。
- 哪些信息是必须覆盖，哪些可以在简洁版本中省略。
- 有哪些行动、风险、开放问题和禁止越界表达。

## 主内容池工作流程

1. 完整阅读原文，去除闲聊、重复、无关口头禅和敏感信息。
2. 判断会议主线：不要按发言顺序机械整理。
3. 抽取 1 个主主题、1 个核心判断或当前状态、1 个中心问题。
4. 抽取核心议题，通常 4-10 个；极短会议可更少，复杂会议可更多。
5. 每个议题必须包含：
   - `topic_id`
   - `topic`
   - `priority`
   - `summary`
   - `key_points`
   - `coverage_rule`
6. 抽取行动项、开放问题和风险边界。没有就留空数组，不要编造。
7. 做静默检查：忠诚度、完成度、重点突出、无幻觉、无过度包装。

## 子 skill 调用要求

生成 `content-pool.json` 后，必须把同一份会议原文和刚生成的 `content-pool.json` 交给三个子 skill。不要让子 skill 只看主合同，也不要让子 skill 脱离主合同重新归纳主题。

三个子 JSON 必须满足：

- `content_pool_id` 与主合同完全一致。
- `topic_guidance.topic_id` 只能来自主合同 `topics.topic_id`。
- 不新增主合同之外的新主题。
- 不输出页面、页码、模块编号、章节编号、版式字段或视觉图式字段。
- 不输出 `block_type`、`content_form`、`recommended_content_schema`。
- 输出定位是方向引导，不是最终正文，不是页面脚本。

如果子 skill 发现主合同可能遗漏了重要议题，只能写入 `contract_alignment_notes`，不能自行新增主题。

## 完整流程静默审查

四个 JSON 生成后，必须检查：

- 四个 JSON 都能被标准 JSON 解析。
- 四个 JSON 是独立文件，不是合并包。
- 三个子 JSON 继承同一个主合同。
- 三个子 JSON 的议题范围和主合同一致。
- 高优先级议题没有被遗漏。
- 低优先级信息没有喧宾夺主。
- 没有幻觉、补造、过度包装。
- 没有输出页面、模块、章节、版式字段。

## 优先级规则

- `high`：影响主题、判断、行动、风险或后续生产，简洁版也应保留。
- `medium`：支撑理解，标准版应保留，简洁版可压缩。
- `low`：背景、例子、局部细节，完整版本可保留，简洁版可省略。

## 禁止

- 不输出 PPT 页、信息图模块或 H5 章节。
- 不决定视觉版式、页面结构、图式、布局。
- 不输出 `block_type`、`content_form`、`visual_handoff`、`thinking_basis`、`source_anchor`、`statement_id`。
- 不把探索、建议、愿望、礼貌回应写成已确定结论。
- 不新增原文没有的负责人、日期、数字、承诺或业务结果。
