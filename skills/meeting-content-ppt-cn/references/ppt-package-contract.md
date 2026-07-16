# ppt-content-package.json 输出合同

最终只输出一个 JSON 对象：

```json
{
  "schema_version": "2026-07-16.ppt.v1",
  "content_pool_id": "必须与主合同一致",
  "artifact_type": "ppt",
  "title": "PPT 方向标题",
  "core_direction": "这份 PPT 应该围绕什么主线展开",
  "coverage_mode": {
    "standard": "标准版应覆盖的范围",
    "concise": "简洁版应保留什么、压缩什么",
    "extended": "扩展版可展开什么"
  },
  "topic_guidance": [
    {
      "topic_id": "必须来自主合同",
      "topic": "必须与主合同一致或仅做同义短化",
      "priority": "high | medium | low",
      "ppt_role": "opening_context | core_claim | evidence | contrast | decision | risk | action | boundary",
      "direction": "该议题在 PPT 中应承担什么表达作用",
      "must_cover_points": ["必须覆盖的内容"],
      "optional_points": ["标准版可保留、简洁版可省略的内容"],
      "omit_in_concise": true
    }
  ],
  "actions_to_reflect": [
    {
      "owner": "负责人；不明确写未明确",
      "task": "动作",
      "deadline": "截止时间；不明确写未明确",
      "status": "状态",
      "related_topic_ids": ["T1"]
    }
  ],
  "risks_and_boundaries": ["PPT 下游不能越界的风险和边界"],
  "contract_alignment_notes": ["如果发现主合同疑似遗漏或冲突，只在这里提示"],
  "forbidden_actions": ["下游绝对不能做的改写、夸大或补造"]
}
```

## 硬性要求

- 不输出页面、页码、章节页、版式或视觉图式。
- `topic_guidance.topic_id` 必须来自主合同。
- 高优先级议题不能在简洁版中被省略。
- `omit_in_concise` 只能用于 medium / low 议题或次要点。
- 不输出 `block_type`、`content_form`、`recommended_content_schema`。
