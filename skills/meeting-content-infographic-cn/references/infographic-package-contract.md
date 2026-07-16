# infographic-content-package.json 输出合同

最终只输出一个 JSON 对象：

```json
{
  "schema_version": "2026-07-16.infographic.v1",
  "content_pool_id": "必须与主合同一致",
  "artifact_type": "infographic",
  "title": "信息图方向标题",
  "one_sentence_direction": "信息图应让读者快速理解什么",
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
      "infographic_role": "context | hierarchy | contrast | cause | decision | risk | action | boundary",
      "direction": "该议题在信息图中应承担什么关系表达作用",
      "must_cover_points": ["必须覆盖的内容"],
      "optional_points": ["可在丰富版中展开的内容"],
      "omit_in_concise": true
    }
  ],
  "relationship_hints": [
    {
      "from_topic_id": "T1",
      "to_topic_id": "T2",
      "relationship": "因果、支撑、对比、先后、约束等内容关系"
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
  "risks_and_boundaries": ["信息图下游不能越界的风险和边界"],
  "contract_alignment_notes": ["如果发现主合同疑似遗漏或冲突，只在这里提示"],
  "forbidden_actions": ["下游绝对不能做的改写、夸大或补造"]
}
```

## 硬性要求

- 不输出模块、模块编号、视觉图式或版式。
- `topic_guidance.topic_id` 必须来自主合同。
- 高优先级议题不能在简洁版中被省略。
- `relationship_hints` 只描述内容关系，不描述视觉画法。
- 不输出 `block_type`、`content_form`、`recommended_content_schema`。
