# content-pool.json 输出合同

最终只输出一个 JSON 对象：

```json
{
  "schema_version": "2026-07-16.pool.v1",
  "content_pool_id": "meeting-YYYYMMDD-slug",
  "source_language": "auto",
  "shared_theme": "凝练主题",
  "core_judgment": "会议支持的核心判断；未形成判断时写当前状态",
  "central_question": "中心矛盾、主问题或推进线",
  "meeting_context": {
    "purpose": "会议目的或中性描述",
    "certainty": "clear | mixed | uncertain"
  },
  "topics": [
    {
      "topic_id": "T1",
      "topic": "议题名称",
      "priority": "high | medium | low",
      "summary": "该议题的方向性概括",
      "key_points": ["关键事实、判断、问题或动作"],
      "coverage_rule": "must_keep | compressible | optional"
    }
  ],
  "actions": [
    {
      "owner": "负责人；不明确写未明确",
      "task": "动作",
      "deadline": "截止时间；不明确写未明确",
      "status": "已决定 | 进行中 | 待确认 | 后续跟进 | 未明确",
      "related_topic_ids": ["T1"]
    }
  ],
  "open_questions": [
    {
      "question": "待确认问题",
      "related_topic_ids": ["T1"]
    }
  ],
  "risks_and_boundaries": ["风险、限制或不能越界表达的边界"],
  "forbidden_actions": ["下游绝对不能做的语义改写、夸大或补造"]
}
```

## 硬性要求

- `topics` 是三个子 JSON 的共同议题源。
- 子 JSON 不得新增主合同之外的新主题。
- `content-pool.json` 要凝练，不追求全文复述。
- 不输出证据编号、原文锚点、思考过程、版式建议或视觉说明。
- 不输出 `block_type`、`content_form`、`recommended_content_schema`。
