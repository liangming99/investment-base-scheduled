# 运行合同

## 0. 通用输入

- Base 地址：`https://foix66zekl.feishu.cn/base/HndRbPcSda4E16saPa3cmtgZnHc`
- Base 名称：`市场开发重点项目周报管理（V2）`
- 通过当前主机可用的飞书 Base connector/API 读取和写回；`lark-cli` 只是可选适配器，不是本包的本地文件依赖。
- 如无法访问 Base、解析地址、读取 schema 或确认当前用户权限，输出 `BLOCKED/PROVIDER_UNAVAILABLE` 或 `BLOCKED/FRESH_READ_MISSING`，不猜测配置。

## 1. Fresh read

1. 使用上方 Base 地址解析并读取 Base，再读取 schema/数据表/视图和 workflow 清单；若使用 `lark-cli`，对应动作是 `base +url-resolve`、`+base-get`、`+base-block-list` 和 `+workflow-list`。
2. 对选中的 workflow 执行 provider 对应的详情读取（`lark-cli` 为 `+workflow-get`），核对 `workflow_id`、标题、状态、`TimerTrigger.rule/start_time`、Base 时区和关键表/字段。以当前线上定义为权威；本包的 baseline 只是线索。

完成标准：选中的成员、workflow ID、状态、时间、时区和输入字段均有当前命令回执；任一项缺失就停在 `BLOCKED/FRESH_READ_MISSING`。

## 2. 执行

- 一次只运行一个成员；不要把相邻时间槽自动串成夜间总任务。
- 先用服务端筛选得到输入数量，再执行最小批次；保留稳定项目关系/record ID，名称和简称只作显示或当前兼容键。
- 以幂等键、状态标记或待通知锁保护重复运行；空输入必须是成功的 no-op。
- 外部 Agent 的 provider、凭据、workflow 运行入口、消息发送与出网能力在本候选包中仍是 `missing evidence`。不能把“有 TimerTrigger”当成可直接调用的 `workflow-run`。

完成标准：每个写入/消息动作都有目标、数量或回执；没有可证明的执行入口时只输出计划与阻塞，不重写 native workflow。

## 3. Read-back

回读目标记录、状态标记、候选/基线/消息和去重锁；把结果分为 `LOCAL_CHECK`、`EXTERNAL_READBACK`、`BUSINESS_ACCEPTANCE`。历史回执、受控调时和配置读回不能冒充自然时刻长期稳定或业务通过。

完成标准：成员的 output contract 每一项都有当前证据或明确 `UNVERIFIED`，并给出一个下一动作。

## 4. 停止与回滚

遇到 ID/字段漂移、重复匹配、关系不唯一、来源不完整、holiday-unverified 日期、消息收件人不确定或写入范围扩大时停止。外部调度的回滚边界是停止调度、恢复 `disable-model-invocation: true`，不自动删除或反写已有数据。
