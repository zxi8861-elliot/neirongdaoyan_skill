---
name: meeting-content-infographic-cn
description: 会议信息图内容方向子导演。输入会议原文和 content-pool.json，输出 infographic-content-package.json，用于给下游信息图生成流程提供主题、议题、层级、重点、优先级和边界引导；不直接决定信息图模块、版式或视觉图式。
---

# 会议信息图内容方向子导演

本 skill 只生成 `infographic-content-package.json`。它不是信息图模块脚本，不决定图式、布局或模块顺序。下游会同时读取原文和本 JSON，所以本 JSON 要提供内容覆盖和优先级，而不是替下游完成编排。


## 语言规则

- 可处理中文、英文、日文或多语言混合会议原文。
- 输出语言优先遵循用户要求。
- 用户未指定输出语言时，尽量沿用任务上下文和原文主要语言；不要因为 skill 名称或历史版本而拒绝其他语言输入。
- 产品名、人名、渠道名、市场术语和专有名词可保留原文语言。

## 输入

必须同时读取：

1. 会议原文。
2. `content-pool.json` 主内容池合同。

如果没有主内容池合同，应先使用 `meeting-content-pool-cn` 生成。

## 输出

如果环境支持创建文件，写入 `infographic-content-package.json`。否则只输出一个可被 `JSON.parse` 解析的裸 JSON 对象。禁止 Markdown、解释文字、代码块。

必须阅读并遵守 [references/infographic-package-contract.md](references/infographic-package-contract.md)。

## 生成原则

- 信息图 JSON 是“关系和层级方向”，不是视觉编排。
- 不输出模块、版式、图式或 `recommended_content_schema`。
- 必须继承主合同的主题、核心判断、议题和边界。
- 内容覆盖度要全：保证下游能生成丰富版；同时标注优先级，支持简洁版裁剪。
- 信息图更关注：已定/未定、因果、对比、风险、行动、边界、主次关系。

## 禁止

- 不输出 `modules`、`module_no`、`recommended_content_schema`。
- 不输出 `block_type`、`content_form`、视觉说明。
- 不把口号当信息图内容。
- 不新增主合同之外的新主题。
