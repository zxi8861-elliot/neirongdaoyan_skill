---
name: meeting-content-h5-cn
description: 会议分析总结 H5 内容方向子导演。输入会议原文和 content-pool.json，输出 h5-content-package.json，用于给下游 H5 生成流程提供主题、议题、解释深度、优先级和边界引导；不直接决定章节、页面或完整正文。
---

# 会议 H5 内容方向子导演

本 skill 只生成 `h5-content-package.json`。它不是 H5 文章正文，也不决定章节编排。下游会同时读取原文和本 JSON，所以本 JSON 要提供慢读分析方向、解释深度和边界。


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

如果环境支持创建文件，写入 `h5-content-package.json`。否则只输出一个可被 `JSON.parse` 解析的裸 JSON 对象。禁止 Markdown、解释文字、代码块。

必须阅读并遵守 [references/h5-package-contract.md](references/h5-package-contract.md)。

## 生成原则

- H5 JSON 是“分析总结方向”，不是完整文章。
- 不输出章节编号、页面、版式或完整正文。
- 必须继承主合同的主题、核心判断、议题和边界。
- H5 应比 PPT 和信息图保留更多上下文、解释、风险、开放问题和判断强度。
- 内容覆盖度要全，同时用优先级告诉下游哪些适合简洁版压缩。

## 禁止

- 不输出 `sections`、`section_no`、完整段落正文。
- 不输出 `block_type`、`content_form`、视觉说明。
- 不新增主合同之外的新主题。
- 不把分析升华写成无证据结论。
