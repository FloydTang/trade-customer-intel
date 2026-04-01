# Trade Customer Intel

中英双语的开源 Codex Skill，用公开网页线索为外贸销售、客户开发和线索筛查生成结构化客户情报报告。

An open-source Codex skill that turns sparse public-web signals into bilingual, structured customer intelligence reports for sales research, lead verification, and conservative outreach preparation.

当前状态：可演示

角色定位：`客户情报分析员`

链路角色：

- 在总链路里是 `stage_worker`
- 组合包 / 主代理是 `workflow_owner`
- 单节点默认 `attach_only`
- `feishu_container_creation = forbidden`
- 单节点不独立声明飞书工作容器
- 所有数据最终统一挂到同一个 `Trade Lead Workflow Hub`

上下游关系：

- 上游：[trade-lead-screening](https://github.com/FloydTang/trade-lead-screening) 或人工提供的稀疏线索
- 下游：[trade-outreach-email](https://github.com/FloydTang/trade-outreach-email)

## 公开最小可用说明

这个仓库公开层只解决一个问题：

- 把稀疏线索变成保守、可复核的客户情报报告
- 和组合包一样，这个单节点仓库本身就拥有可独立执行的最小功能

当前最小能力：

- 收集公开网页证据
- 做实体匹配和风险判断
- 输出结构化客户情报报告
- 给出证据清单和销售切入角度

## 飞书增强入口

如果你要把这个节点接进龙虾 / OpenClaw 多代理链路，优先不要先看长教程，直接复制增强执行词给龙虾：

- [飞书增强入口：复制增强执行词给龙虾](https://evenbetter.feishu.cn/wiki/ADmiwiultihx6Yk1p2UcjfmVn6d)

如果链接打开失败，请使用登记在半斤九两群里的飞书账号打开。

如果你拿到的是半斤九两科技沟通过的执行包用户链接，也可以优先使用那个链接打开。

如果你当前还没有半斤九两科技的账号，需要联系半斤九两科技，请访问：[evenbetter.tech](https://evenbetter.tech)

仓库内对应的源码基线在：

- `references/00-单节点增强执行词.md`
- `for-openclaw/README.md`
- `for-openclaw/SKILL.md`

## 推荐模型

- `coze/doubao-seed-2-0-pro-260215`

特殊情况：

- 如果上下文特别长，或需要长上下文整合，可临时切到 `coze/kimi-k2-5-260127`

## What It Produces

输入可以很稀疏，例如：

```json
{
  "company_name": "Acme Industrial",
  "person_name": "Jane Smith",
  "email": "jane@acme-industrial.com",
  "company_website": "",
  "country_or_market": "United States",
  "notes": ""
}
```

输出会包含：

- `Executive Summary / 执行摘要`
- `Identity Snapshot / 身份快照`
- `Company Profile / 公司画像`
- `Digital Footprint / 数字足迹`
- `Interest & Topic Signals / 主题信号`
- `Sales Angles / 销售切入角度`
- `Risk Rating / 风险评级`
- `Evidence / 证据清单`

## Quick Start

```bash
python3 ./scripts/build_customer_intel_report.py \
  --input-json ./examples/sample-input.json \
  --markdown-out /tmp/customer-intel.md \
  --json-out /tmp/customer-intel.json
```

```bash
cat <<'EOF' | python3 ./scripts/build_customer_intel_report.py
{
  "company_name": "Acme Industrial",
  "person_name": "Jane Smith",
  "email": "jane@acme-industrial.com",
  "country_or_market": "United States"
}
EOF
```

## Feishu / OpenClaw Stage Export

如果你只想单独使用客户背调 Skill，也建议把结果并入统一主表。

在生成客户背调 JSON 后，再运行：

```bash
python3 ./scripts/build_feishu_stage_payload.py \
  --input-json /tmp/customer-intel.json \
  --combo-run-id manual-run \
  --lead-id lead-001
```

这个脚本不会重新搜索或改写报告。

它只负责把已有客户背调报告转成 OpenClaw 可消费的文档 payload，用于：

- 创建或更新客户背调云文档
- 回写 `Lead Workflow Master`
- 支持“外部导入后直接跑背调”的单点接入方式

## Chain Position

推荐链路：

`trade-lead-discovery -> trade-lead-screening -> trade-customer-intel -> trade-outreach-email`

关联仓库：

- 客户搜索 Skill: [trade-lead-discovery](https://github.com/FloydTang/trade-lead-discovery)
- 线索整理 Skill: [trade-lead-screening](https://github.com/FloydTang/trade-lead-screening)
- 开发信 Skill: [trade-outreach-email](https://github.com/FloydTang/trade-outreach-email)

## Agent-First 增强价值

会员增强层当前不是改业务逻辑，而是补这几件事：

- 单节点在龙虾里有明确的 `stage_worker` 角色
- 单节点默认只 attach，不独立建飞书工作容器
- 飞书里提供可直接复制给龙虾的增强执行词
- 与总编排链路保持同一套文档复用、失败回报和协作口径

## Repository Structure

```text
.
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── 00-单节点增强执行词.md
│   ├── report-template.md
│   └── source-playbook.md
├── scripts/
│   └── build_customer_intel_report.py
├── for-openclaw/
│   ├── SKILL.md
│   ├── README.md
│   ├── examples/
│   ├── references/
│   ├── schemas/
│   └── scripts/
├── examples/
│   └── sample-input.json
└── .github/
```

## OpenClaw Variant

`for-openclaw/` 提供和总仓一致口径的单节点 OpenClaw 包装版本：

- 角色固定为 `stage_worker`
- 默认只允许 attach 到 `Trade Lead Workflow Hub`
- 不允许独立创建 Base、主表或平行工作容器

## Attribution

Created and maintained by 半斤九两科技.
