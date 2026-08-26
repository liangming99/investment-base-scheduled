---
name: investment-base-leadership-change-notification
description: 手动执行 Base 23:45 领导口径变化判定、基线和通知。
disable-model-invocation: true
---

# 领导重要变化判定与通知（F）

对应 workflow：`wkfMX4NEjcOGVvCZ`，当前应为 `TimerTrigger/DAILY/23:45/enabled`。

1. 按 [run-contract.md](../../references/run-contract.md) 使用包内 Base 地址 fresh-read 当前定义、Base 时区、基线表、收件人解析和消息能力。先处理基线中的待发送重试，再扫描 `01` 项目最终口径。
2. 按项目关系读取 `领导重要变化基线（系统）`。原始字段没有变化时只接受当前文字并更新基线；原始字段变化时才做严格二值 `NOTIFY/NO_CHANGE` 判断。
3. `NOTIFY` 时生成领导可读的增量说明，写入基线待发送状态，发送至已解析的梁铭收件人，成功后清除待发送锁。新增、删除、失效关联和重试分支保持幂等。

输出项目数、NO_CHANGE/NOTIFY/新增/删除/重试数、基线写回、消息 ID、清锁结果和完整读回。收件人无法唯一解析、消息回执缺失或重复锁不安全时停止，不发送。
