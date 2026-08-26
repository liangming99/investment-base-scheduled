---
name: investment-base-next-plan-aggregation
description: 手动执行 Base 22:10 周报下周工作目标按小类归集。
disable-model-invocation: true
---

# 下周工作目标按小类归集（Q）

对应 workflow：`wkf2rnPTlPB59l22`，当前应为 `TimerTrigger/DAILY/22:10/enabled`。

1. 按 [run-contract.md](../../references/run-contract.md) 使用包内 Base 地址 fresh-read 当前定义；筛选 `03-周报提交` 中 `已提炼=true`、`计划已归集=false` 且项目自动关联非空的记录。
2. 读取项目 `02` 推进项，并按 `对应推进大类` 限定总体工作。合并同一小类的下周目标，保留责任事项、里程碑和待协调事项，不把空计划写成内容。
3. 按项目关系和小类定位写 `02.下周工作目标`，再按原记录身份条件标记 `03.计划已归集=true`。

空计划、无匹配小类、无输入和重复运行均为 no-op。输出输入数、写回小类数、标记数、跳过原因和读回；不要启用已禁用的旧 `wkfxteIYYrG7IS6j`。
