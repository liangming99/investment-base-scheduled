---
name: investment-base-overdue-candidate-reminder
description: 手动执行 Base 每日17:05 逾期待确认候选提醒。
disable-model-invocation: true
---

# 逾期待确认候选提醒

对应 workflow：`wkfJfjs68UuATwzl`，当前应为 `TimerTrigger/DAILY/17:05/enabled`。

1. 按 [run-contract.md](../../references/run-contract.md) 使用包内 Base 地址 fresh-read 当前定义；筛选 `05-推进状态候选` 中 `确认状态=待确认`、确认责任人非空且 `待通知=false` 的记录。
2. 对每条候选按 `确认截止时间` 做严格安全判断：提交时间次日起的第一个周一至周五 17:00；中国法定节假日目前是 `holiday-unverified`，不得假装已处理。
3. 仅严格逾期时通知动态确认责任人，并以候选记录身份写 `待通知=true`。未逾期、责任人缺失、状态已变化或重复运行均不发送。

输出候选输入数、严格逾期数、消息 ID、清单/锁读回和未验证项。日期计算或人员解析不确定时停止。
