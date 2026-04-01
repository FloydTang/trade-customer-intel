---
name: trade-customer-intel-for-openclaw
description: OpenClaw-native version of Trade Customer Intel. Use coze-web-search for search, scrapling-official for primary page extraction, and coze-web-fetch as fallback. Generate a bilingual, evidence-backed customer intelligence report from a structured evidence bundle.
openclaw_role: stage_worker
workspace_owner_skill: trade-active-outreach-combo
single_skill_policy: attach_only
feishu_container_creation: forbidden
requires_master_base: true
requires_master_record: true
---

# Trade Customer Intel for OpenClaw

## Overview

This skill is the OpenClaw-native companion to the repository's classic version.

It is designed for cloud OpenClaw environments where search and page retrieval are performed by platform tools first, and a Python report builder then turns the resulting evidence bundle into a structured bilingual report.

## Inputs

Normalize operator input into this lead shape:

```json
{
  "company_name": "",
  "person_name": "",
  "email": "",
  "company_website": "",
  "country_or_market": "",
  "notes": ""
}
```

The final report-builder input must wrap that lead together with an evidence bundle:

```json
{
  "lead": {},
  "evidence_bundle": {
    "search_results": [],
    "page_snapshots": [],
    "search_runs": [],
    "errors": []
  }
}
```

## Feishu Runtime Contract

- 当前角色固定为 `stage_worker`
- 默认只允许附着到 `Trade Lead Workflow Hub`
- 只允许复用 `Customer Intel Docs`
- 不允许独立创建 Base、主表或平行工作容器
- 必须先查 `Lead Workflow Master`
- 已有客户背调文档时，只追加版本，不新建平行文档

## Output Requirements

- Keep analysis mainly in Chinese.
- Keep risk scoring conservative.
- Preserve `Low`, `Medium`, `High` ratings only.
- Generate the customer-intel report, risk rating, evidence list, and stage payload.
- Do not auto-send outreach messages.
