# 输出合同

## 硬性格式要求

默认最终交付物是一个 JSON 包文件：`meeting-content-package.json`。

如果当前环境支持创建文件，必须把完整结果写入 `meeting-content-package.json`，而不是只在对话中输出一大段 JSON。

如果当前环境不支持创建文件，最终响应才退化为裸 JSON 对象，且必须能被 `JSON.parse` 直接解析。

最终交付前必须通过 JSON 合格化闸门：

1. 有代码执行能力时，必须用标准 JSON 解析器检查最终包。
2. 解析失败时，先本地清洗：去掉代码围栏、JSON 前后说明文字、BOM、不可见字符，并提取第一个完整 JSON 对象。
3. 清洗后仍失败时，只允许修复格式，不允许改写语义、增加事实或补造业务内容。
4. 如果内容明显截断、字段大量缺失或无法判断语义，必须重跑当前阶段；不要靠猜测修复。
5. 只有能被标准 JSON 解析器解析的对象，才允许写入 `meeting-content-package.json` 或作为最终响应。

禁止输出：

- Markdown。
- 代码块。
- JSON 外的解释文字。
- 中间过程。
- 审计说明。
- 无法被标准 JSON 解析器解析的半成品。

如果用户没有明确要求其他格式，也必须交付本合同规定的四文件 JSON 包。不能因为用户说“结论”“总结”“整理”就改成 Markdown。

不要默认拆成四个独立文件。默认只交付一个总包文件：

```text
meeting-content-package.json
```

只有当用户明确要求“拆成四个文件”时，才从总包中原样拆出：

```text
content-pool.json
h5-content-package.json
infographic-content-package.json
ppt-content-package.json
```

总包内只包含四个生产 JSON。它们会被后续 H5、信息图、PPT 视觉生成流程直接读取，所以不要放审计字段、解释性字段或中间思考字段。

调用者必须在内部执行但不得输出“产物策略规划”：先确认 `content-pool.json` 的统一主题、核心判断、中心问题、子主题、行动项、关键边界和禁区，再分别派生 H5、信息图、PPT。禁止三个产物各自重新归纳主题。

顶层输出必须是：

```json
{
  "content-pool.json": {},
  "h5-content-package.json": {},
  "infographic-content-package.json": {},
  "ppt-content-package.json": {}
}
```

## 禁止出现在最终 JSON 里的字段

不要在四个最终 JSON 中输出这些字段：

```text
statement_id
statement_ids
evidence_statement_ids
source_anchor
source_anchors
evidence_anchors
selection_logic
omission_log
body_supports
semantic_schema
elevation_id
elevation_level
boundary_notes
content_director_basis
safety_log
audit_log
evidence_log
block_type
statement_usages
action_usages
topic_ids
thinking_basis
visual_handoff
downstream_use_rule
description
why_this_structure
organization_notes
page_intent
audience_task
narrative_path
infographic_intent
h5_intent
reading_structure
overall_structure
generation_constraints
content_form
numeric_highlights
recommended_content_schema
artifact_strategy_plan
product_strategy_plan
review_gate
quality_gate
```

最终 JSON 只保留两类内容：

1. 可展示内容：标题、正文、标签、表格项、模块项、页面项、章节项、短语、列表。
2. 严格禁止行为：下游视觉生成绝对不能做的语义改写、夸大、补造、承诺化。

## 标题约束

- 总标题通常控制在 6-18 个汉字，必要时可以略长，但不能写成摘要句。
- 页面标题、模块标题、章节标题通常控制在 6-16 个汉字。
- 标题必须围绕本页、本模块、本章节的具体内容特化。
- 少用“当前状态”“会议核心判断”“下一步”“收束”这类泛标题。
- 副标题不是必填；标题能说清楚时留空。
- 封面和结尾标题尤其要短。

## `content-pool.json`

内容池是三种产物共用的内容母版。

```json
{
  "content_pool_id": "meeting-YYYYMMDD-slug",
  "shared_theme": "中文统一主题",
  "core_judgment": "会议支持的核心判断；若未形成判断，写当前状态",
  "central_question": "中心矛盾、主问题或最重要的推进线；非决策场景也要写成适合该场景的主线",
  "meeting_context": {
    "purpose": "会议目的或中性描述",
    "audience_or_use": "可选；仅在用户提供或原文可安全推断时填写",
    "certainty": "clear | mixed | uncertain"
  },
  "key_topics": [
    {
      "topic": "议题名称",
      "display_summary": "这个议题在产物中可以如何概括展示",
      "display_points": ["可展示要点", "可展示要点"],
      "suitable_artifacts": ["ppt", "infographic", "h5"]
    }
  ],
  "action_items": [
    {
      "owner": "负责人；不明确写未明确",
      "task": "需要执行的具体动作",
      "deadline": "截止时间；不明确写未明确",
      "status": "已决定 | 待确认 | 进行中 | 后续跟进 | 未明确"
    }
  ],
  "open_questions": ["未定问题或待验证事项"],
  "forbidden_actions": [
    "不能把未确定事项写成已确定",
    "不能新增原文没有的负责人、日期、数字或承诺"
  ]
}
```

## `ppt-content-package.json`

PPT 内容包按页面组织。字段只描述页面上可展示的内容，以及严格禁止行为。

```json
{
  "content_pool_id": "meeting-YYYYMMDD-slug",
  "deck_title": "中文 PPT 标题",
  "deck_subtitle": "可选；必须短，不需要就留空",
  "pages": [
    {
      "page_no": 1,
      "page_type": "cover | content | closing",
      "title": "页面可见标题",
      "subtitle": "可选可见副标题；内容页通常留空",
      "display_text": ["可见短句、标签、引导语；可为空"],
      "content_blocks": [
        {
          "block_title": "可选可见块标题",
          "items": ["可见内容项"],
          "columns": [
            {
              "header": "可见列标题",
              "items": ["可见内容项"]
            }
          ],
          "rows": [
            ["表格单元格", "表格单元格"]
          ],
          "actions": [
            {
              "owner": "负责人；不明确写未明确",
              "task": "具体动作",
              "deadline": "截止时间；不明确写未明确",
              "status": "状态"
            }
          ]
        }
      ],
      "forbidden_actions": [
        "本页不能新增原文没有的结论",
        "本页不能把探索中内容写成已完成"
      ]
    }
  ],
  "forbidden_actions": [
    "整套 PPT 不能改变统一主题",
    "不能新增原文没有的数字、负责人、日期或承诺"
  ]
}
```

### PPT 输出要求

- 封面低密度：只放标题、可选短副标题、最多两条短标签。
- 结尾低密度：只收束行动、边界或提醒；不要塞解释内容。
- 内容页不固定三句话。根据内容关系在 `content_blocks` 中组织成列表、列、表格、流程、层级、风险行动等内容。
- PPT 不要过度压缩。复杂页面可以有多组 `content_blocks`，以支撑展示讲述。
- 只有构成关键结论、行动要求、风险边界、进度节点或业务约束的数字、日期、金额、数量、比例、版本号或里程碑，才需要自然写入 `display_text` 或 `content_blocks`。不要为了展示数字而展示数字。
- 不输出 `content_form` 或 `block_type`。不要提前替视觉导演决定页面版式，也不要把内容锁死成列表、表格、流程、卡片等形式。
- `content_blocks` 是主要可见内容来源。
- 不要默认生成“行动收口”“行动清单”“下一步”页面。只有原文有 3 条以上明确行动项、行动项包含明确负责人/截止时间/跨团队分工、行动项是会议最重要产出，或不单独成页会影响理解时，才允许单独生成行动页。
- 不满足单独成页条件时，行动项应并入相关内容页、结尾页短提醒，或只进入 `content-pool.json` / H5。
- 有行动项时，尽量写清楚“谁 / 做什么 / 何时 / 状态”；不明确就写“未明确”。
- 行动页标题不能只写“行动收口”“行动清单”“关键行动”“下一步”，必须写成具体推进事项。
- 禁止生成只有 `block_title`、没有 `items` / `rows` / `columns` / `actions` 的空内容块。

## `infographic-content-package.json`

信息图内容包按模块组织。它要表达信息关系，而不是只给口号。

```json
{
  "content_pool_id": "meeting-YYYYMMDD-slug",
  "artifact_type": "infographic",
  "title": "中文信息图标题",
  "subtitle": "可选短副标题",
  "one_sentence_judgment": "可见或准可见的一句话判断",
  "modules": [
    {
      "module_no": 1,
      "module_title": "可见模块标题",
      "display_text": "可见模块短说明",
      "content_blocks": [
        {
          "block_title": "可选可见块标题",
          "items": ["可见内容项"],
          "columns": [
            {
              "header": "可见列标题",
              "items": ["可见内容项"]
            }
          ],
          "actions": [
            {
              "owner": "负责人；不明确写未明确",
              "task": "具体动作",
              "deadline": "截止时间；不明确写未明确",
              "status": "状态"
            }
          ]
        }
      ],
      "forbidden_actions": [
        "本模块不能把待验证事项说成结论"
      ]
    }
  ],
  "required_visible_text": ["必须出现在图上的中文关键词或短句"],
  "text_density": "low | medium | high",
  "forbidden_actions": [
    "不能把信息图做成只有口号的海报",
    "不能删除原文明确保留的限制条件"
  ]
}
```

### 信息图输出要求

- 模块数量随内容变化，不固定。
- 文字密度随内容变化，用 `text_density` 表示即可，不要写固定字数。
- 只有构成关键结论、行动要求、风险边界、进度节点或业务约束的数字、日期、金额、数量、比例、版本号或里程碑，才需要自然写入信息图模块或 `required_visible_text`。弱相关数字不要进入信息图。
- 不输出 `recommended_content_schema`。信息图的关系通过模块顺序、模块标题和 `content_blocks` 自然体现。
- `required_visible_text` 是可见中文，不是后台说明。

## `h5-content-package.json`

H5 是最完整的阅读版本。它可以比 PPT 和信息图更长，但仍只保留可展示内容和禁止行为。

```json
{
  "content_pool_id": "meeting-YYYYMMDD-slug",
  "title": "中文 H5 标题",
  "subtitle": "可选短副标题",
  "executive_summary": "可见摘要；应是三种产物中最完整的摘要",
  "sections": [
    {
      "section_no": 1,
      "section_title": "可见章节标题",
      "summary": "可见章节摘要",
      "content_blocks": [
        {
          "block_title": "可选可见块标题",
          "items": ["可见内容项或段落"],
          "rows": [
            ["表格单元格", "表格单元格"]
          ],
          "actions": [
            {
              "owner": "负责人；不明确写未明确",
              "task": "具体动作",
              "deadline": "截止时间；不明确写未明确",
              "status": "状态"
            }
          ]
        }
      ],
      "forbidden_actions": [
        "本章节不能新增原文没有的背景"
      ]
    }
  ],
  "forbidden_actions": [
    "H5 不能比 PPT 更薄，除非原文确实极短",
    "不能把未解决的问题包装成已解决"
  ]
}
```

### H5 输出要求

- H5 通常应包含更多上下文、解释、风险、开放问题和决策背景。
- 不要重复 PPT 的短句列表就结束。
- 可以使用段落、列表、表格、风险说明、开放问题等形式。

## 最终静默检查

输出前检查并直接修正：

- 最终包能被标准 JSON 解析器解析。
- 四个 JSON 的 `content_pool_id` 一致。
- 三份产物的主题、事实、判断强度、行动状态一致。
- 三份产物必须能看出是由同一个 `content-pool.json` 派生，而不是各自重新归纳。
- 内部产物策略规划不得进入最终 JSON。
- 没有禁止字段。
- 没有 `block_type`。内容块只能给内容本身，不能提前给列表、表格、流程、卡片等形式标签。
- 没有解释性长字段。
- 所有 `forbidden_actions` 都是严格禁止行为，不是普通建议。
- 可展示内容足够具体，不是空泛口号。
- 构成关键行动、边界或结论的日期、金额、数量、比例、版本号或里程碑没有被遗漏；弱相关数字没有被当成重点展示。
- H5 比 PPT 更完整；信息图能看出信息关系；PPT 内容组织不要单调。
- PPT 没有默认塞“行动收口 / 行动清单 / 下一步”页面；如果有行动页，它必须满足单独成页条件，并包含具体 `items` / `rows` / `columns` / `actions`。
- 没有只有块标题、没有实际可展示内容的空 `content_blocks`。

如果最终包无法解析：

- 先做格式清洗和 JSON 提取。
- 再做纯格式修复。
- 仍失败则重跑失败阶段。
- 不得把无法解析的内容交付给用户或下游流程。
