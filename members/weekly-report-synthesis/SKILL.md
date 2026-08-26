---
name: investment-base-weekly-report-synthesis
description: 手动执行 Base 21:00 周报凝练、项目周总报和推进状态候选生成。
disable-model-invocation: true
---

# 周报提炼与候选生成（G）

对应 workflow：`wkfLhxcU7iU3S0gU`，当前应为 `TimerTrigger/DAILY/21:00/enabled`。

1. 按 [run-contract.md](../../references/run-contract.md) 使用包内 Base 地址 fresh-read；只处理 `03-周报提交` 中 `已提炼=false` 且 `项目（自动关联）` 非空的记录。
2. 按记录读取 `01` 项目、`02` 推进项、`04` 上周计划和本周槽位。合并多人原文，生成目前进展、推进量/难/要点、下周工作计划、计划完成度和计划偏差；不得虚构，首周无上周计划时完成度为 `0`。
3. 以“项目 + 周槽位”幂等更新/创建 `04`，写回 `01` 周级只读链所消费的 AI 原始值，并将 `累计待更新=true`。再在所选大类范围内识别会改变当前状态的具体小类：有新事实且状态改变才写 `05` 待确认候选；映射失败写异常行；无证据输出 `NONE`。
4. 标记该 `03` 已提炼并进入归集队列。空输入、同周重跑和状态不变必须是 no-op；AI 不得直接覆盖 `02.推进状态`。

输出：输入数、`04` 更新/创建数、`05` 成功/失败/NONE 数、`01/03` 写回数、幂等结果、逐项读回和未验证项。
