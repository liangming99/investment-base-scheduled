---
name: investment-base-completion-aggregation
description: 手动执行 Base 22:00 周报完成情况按小类归集。
disable-model-invocation: true
---

# 完成情况按小类归集（C）

对应 workflow：`wkfVPIfLSLEBgHyZ`，当前应为 `TimerTrigger/DAILY/22:00/enabled`。

1. 按 [run-contract.md](../../references/run-contract.md) 使用包内 Base 地址 fresh-read 当前定义；筛选 `03-周报提交` 中 `已提炼=true`、`已归集=false` 且项目自动关联非空的记录。
2. 读取项目 `02-项目推进清单` 的总体工作、小类、当前完成情况。填报人选择的 `对应推进大类` 只限定候选范围；逐小类先判断是否有新增事实，再只在明确 `UPDATE` 时合并写 `02.完成情况`。
3. 保留原文事实、去重多人重复内容，不把大类选择当成完成证据。完成归集后按记录身份和原文/提交时间条件标记 `03.已归集=true`。

空输入、无新增事实、未命中大类或重复运行均不新增写入。输出 `03` 输入数、`02` 更新数、静默数、标记数和完整读回；若当前定义、项目关系或字段漂移，停止。
