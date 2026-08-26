---
name: investment-base-cumulative-progress
description: 手动执行 Base 22:30 按变化重算项目累计进展。
disable-model-invocation: true
---

# 累计进展生成（A）

对应 workflow：`wkfJ1qtMnfTZtvon`，当前应为 `TimerTrigger/DAILY/22:30/enabled`。

1. 按 [run-contract.md](../../references/run-contract.md) 使用包内 Base 地址 fresh-read 当前定义；读取 `01-项目主档` 的项目、累计说明、累计待更新和累计最近生成时间。
2. 对每个项目检查锚点之后是否有 `04` 周快照或 `02` 推进清单变化。无变化时保持 no-op；有变化才读取全部历史快照和当前推进清单。
3. 基于可追溯历史生成累计进展，写回 `01.累计进展情况说明`、`累计最近生成时间`，并清除 `累计待更新`。不得把空历史、解析失败或 AI 猜测写成事实。

输出项目数、变化项目数、重算数、跳过数、锚点前后值和完整读回。若项目关系不唯一或时间字段不可比较，停止而不是扩大筛选。
