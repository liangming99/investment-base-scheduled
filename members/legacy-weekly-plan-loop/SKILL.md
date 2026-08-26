---
name: investment-base-legacy-weekly-plan-loop
description: 仅查看已禁用的旧版周报计划闭环定义，禁止执行或启用。
disable-model-invocation: true
---

# 已禁用历史计划闭环

对应 workflow：`wkfxteIYYrG7IS6j`，当前 live 状态为 `disabled`、原定每日 21:20。

它曾按周报计算本周/上周槽位、完成度和偏差，并写回 `01/04`；现行链路已由 G 的 `04` 字段与公式承接。此成员只用于迁移比对、解释旧字段或审计残留：按 [run-contract.md](../../references/run-contract.md) 使用包内 Base 地址 fresh-read 定义，记录与现行链路的差异，然后停止。

完成标准：输出 `DISABLED_REFERENCE_ONLY` 和不启用原因；不得启用、复制、重跑或把旧输出写回生产表。
