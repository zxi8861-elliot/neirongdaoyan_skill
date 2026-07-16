# h5-content-package.json 输出合同

最终只输出一个 JSON 对象：

```json
{
  "schema_version": "2026-07-16.h5.v1",
  "content_pool_id": "必须与主合同一致",
  "artifact_type": "analysis_h5",
  "title": "H5 方向标题",
  "analysis_direction": "H5 应帮助读者深入理解什么",
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
      "h5_role": "background | explanation | decision_context | risk | action | open_question | boundary | implication",
      "direction": "该议题在 H5 中应承担什么解释作用",
      "must_cover_points": ["必须覆盖的内容"],
      "context_to_preserve": ["H5 比 PPT/信息图更应该保留的上下文"],
      "optional_points": ["可在扩展版中展开的内容"],
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
  "open_questions_to_explain": ["H5 中需要解释清楚的未定问题"],
  "risks_and_boundaries": ["H5 下游不能越界的风险和边界"],
  "contract_alignment_notes": ["如果发现主合同疑似遗漏或冲突，只在这里提示"],
  "forbidden_actions": ["下游绝对不能做的改写、夸大或补造"]
}
```

## 硬性要求

- 不输出章节、章节编号、页面或完整正文。
- `topic_guidance.topic_id` 必须来自主合同。
- H5 可以比 PPT/信息图更完整，但仍是方向引导，不是最终成文。
- 不输出 `block_type`、`content_form`、`recommended_content_schema`。
