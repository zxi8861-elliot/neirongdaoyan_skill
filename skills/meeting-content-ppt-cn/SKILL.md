---
name: meeting-content-ppt-cn
description: 会议 PPT 内容方向子导演。输入会议原文和 content-pool.json，输出 ppt-content-package.json，用于给下游 PPT 生成流程提供主题、议题、重点、优先级和边界引导；不直接决定页数、每页文案或版式。
---

# 会议 PPT 内容方向子导演

本 skill 只生成 `ppt-content-package.json`。它不是 PPT 分页脚本，不决定每页放什么，也不决定版式。下游会同时读取原文和本 JSON，所以本 JSON 要做方向控制，而不是替下游写完全部内容。


## 语言规则

- 可处理中文、英文、日文或多语言混合会议原文。
- 输出语言优先遵循用户要求。
- 用户未指定输出语言时，尽量沿用任务上下文和原文主要语言；不要因为 skill 名称或历史版本而拒绝其他语言输入。
- 产品名、人名、渠道名、市场术语和专有名词可保留原文语言。

## 输入

必须同时读取：

1. 会议原文。
2. `content-pool.json` 主内容池合同。

如果没有主内容池合同，应先使用 `meeting-content-pool-cn` 生成，不要直接生成 PPT 子 JSON。

## 输出

如果环境支持创建文件，写入 `ppt-content-package.json`。否则只输出一个可被 `JSON.parse` 解析的裸 JSON 对象。禁止 Markdown、解释文字、代码块。

必须阅读并遵守 [references/ppt-package-contract.md](references/ppt-package-contract.md)。

## 生成原则

- PPT JSON 是“展示叙事方向”，不是页面编排。
- 不输出 `pages`，不假想页数，不写“第 1 页、第 2 页”。
- 必须继承主合同的 `content_pool_id`、主题、核心判断、议题和边界。
- 不新增主合同之外的主题；如发现原文确有主合同遗漏，只能在 `contract_alignment_notes` 中提示，不要悄悄改主题。
- 内容覆盖度要全：标准版 PPT 能据此覆盖主要议题。
- 必须有优先级：让下游能生成标准版、简洁版、扩展版。
- 重点要突出：不要让低优先级背景压过高优先级议题。

## PPT 特有判断

PPT 更关注：

- 展示时最应该讲清的主张。
- 议题之间的叙事顺序和取舍关系。
- 哪些议题必须上屏，哪些只适合口播或扩展版。
- 哪些行动、风险、未定事项会影响推进判断。

## 禁止

- 不输出 `pages`、`page_no`、`page_type`。
- 不输出 `block_type`、`content_form`、版式、布局、视觉说明。
- 不写完整逐页文案。
- 不把未定事项写成已定。
