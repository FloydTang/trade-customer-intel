# Trade Customer Intel for OpenClaw

这个目录提供 OpenClaw-native 的单节点包装版本，目标不是单独造一套飞书工作容器，而是把这个节点稳定挂到总链路里。

角色定位：

- `stage_worker`
- `客户情报分析员`

当前职责：

- 接收单条 lead 和 `evidence_bundle`
- 做公开证据整合和实体判断
- 输出风险评级、情报报告和客户背调阶段 payload

## 安装归口说明

当前仓库采用两层结构：

- 根目录 README：公开最小可用说明
- 当前目录：龙虾 / OpenClaw 的单节点运行说明
- 这个单节点仓库本身也保留可独立执行的最小功能

这个 Skill 的开源版本身就可以单独使用，并能完成当前节点的最小可用功能。

如果你希望在龙虾 / OpenClaw 中获得更精致、更完整的使用体验，建议按下面流程复制增强执行词：

- [飞书增强入口：复制增强执行词给龙虾](https://evenbetter.feishu.cn/wiki/ADmiwiultihx6Yk1p2UcjfmVn6d)

如果链接打不开，请先确认使用和半斤九两科技会员群绑定的飞书账号登录。

如果你拿到的是半斤九两科技沟通过的执行包用户链接，也可以优先使用那个链接打开。

如果你暂时还没有绑定过，或当前还没有半斤九两科技的账号，请访问：[evenbetter.tech](https://evenbetter.tech)

仓库内源码基线：

- `../references/00-单节点增强执行词.md`
- `./SKILL.md`

## Feishu 接入约束

当前这个 OpenClaw 变体如果要接飞书，默认必须挂到同一个主 Base 下运行。

固定要求：

- 先查主 Base，再查 `Lead Workflow Master`
- 客户背调结果只复用同一套 `Customer Intel Docs`
- 已存在客户背调文档时，只追加版本，不新建平行文档
- 当前角色固定为 `stage_worker`
- 当前节点固定 `attach_only`
- `feishu_container_creation = forbidden`

统一目标：

- 所有数据最终统一挂到 `Trade Lead Workflow Hub`
- 单节点不独立声明飞书工作容器

## 推荐模型

- `coze/doubao-seed-2-0-pro-260215`

特殊情况：

- 如果上下文特别长，或需要长上下文整合，可临时切到 `coze/kimi-k2-5-260127`

## 会员增强价值

增强层重点不是换业务逻辑，而是减少安装试错：

- 给龙虾一个可以直接复制的单节点执行词
- 明确这个节点只做证据整合、实体判断和风险评级
- 明确它只复用 `Customer Intel Docs`
- 明确失败后要把证据和弱结论交回主代理统一回写

## Run Locally

```bash
python3 ./for-openclaw/scripts/build_customer_intel_report_from_evidence.py \
  --input-json ./for-openclaw/examples/sample-input.json \
  --markdown-out /tmp/customer-intel-openclaw.md \
  --json-out /tmp/customer-intel-openclaw.json
```

## Relationship to the Classic Version

- 根目录脚本仍然是本仓库 baseline
- `for-openclaw/` 只是 OpenClaw 适配层
- 两个版本应保持输出结构和保守边界一致
