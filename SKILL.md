---
name: trade-customer-intel
description: Build bilingual, evidence-backed customer intelligence reports for foreign-trade leads from a company name, contact name, email, or website. Use when Openclaw or a sales operator needs to research a prospect's company website, public web footprint, and social presence across LinkedIn, Facebook, Instagram, X/Twitter, YouTube, and general web results, then assemble a company/person profile, interest signals, sales angles, and a Low/Medium/High risk rating.
---

# Trade Customer Intel

## Overview

Use this skill to turn sparse lead data into a structured public-web due-diligence report for sales development.

角色定位：

- `客户情报分析员`
- 负责公开网页证据收集、实体匹配、风险判断和情报报告生成
- 不负责批量搜客户入口
- 不负责替代人工直接发送外部邮件

## Chain Role

- 在总链路中固定作为 `stage_worker`
- 默认单节点策略：`attach_only`
- 默认不独立声明飞书工作容器
- 所有数据最终统一挂到 `Trade Lead Workflow Hub`

## Agent-First Installation Notes

这个仓库默认提供两层说明：

- 公开层：根目录 `README.md`，保证最小可用
- 增强层：`for-openclaw/README.md` 和 `references/00-单节点增强执行词.md`

如果你要在龙虾里使用这个节点，优先复制增强执行词给龙虾，而不是先看教程型长文。

## Workflow

1. Normalize input into the standard lead shape.
2. Extract company clues from company name, website, or email domain.
3. Search in this order: website, LinkedIn, Facebook/Instagram, X/YouTube, general web.
4. Resolve identities conservatively and mark weak conclusions as inference.
5. Assemble a report using [report-template.md](./references/report-template.md).
6. Score risk conservatively using [source-playbook.md](./references/source-playbook.md).

## Output Requirements

- Keep the report structured and CRM-friendly.
- Keep core analysis in Chinese.
- Attach source URLs to every material claim when possible.
- Use `Low`, `Medium`, or `High` risk ratings only.
- If the person match is weak, say so explicitly instead of inventing a firm personal profile.
- OpenClaw 单节点默认只 attach，不单独建表。

## Main Script

Use [build_customer_intel_report.py](./scripts/build_customer_intel_report.py) as the default entrypoint.

### Example run

```bash
python3 ./scripts/build_customer_intel_report.py --input-json /path/to/input.json --markdown-out /tmp/customer-intel.md --json-out /tmp/customer-intel.json
```

## References

- [00-单节点增强执行词.md](./references/00-单节点增强执行词.md)
- [report-template.md](./references/report-template.md)
- [source-playbook.md](./references/source-playbook.md)
- [for-openclaw/README.md](./for-openclaw/README.md)
- [for-openclaw/SKILL.md](./for-openclaw/SKILL.md)
