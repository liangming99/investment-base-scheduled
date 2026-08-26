---
name: investment-base-master-data-sync
description: 手动执行 Base 23:00 基础资料来源检查与语义同步。
disable-model-invocation: true
---

# 基础资料同步（E）

对应 workflow：`wkfvdxKwo0RBHPOl`，当前应为 `TimerTrigger/DAILY/23:00/enabled`。

1. 按 [run-contract.md](../../references/run-contract.md) 使用包内 Base 地址 fresh-read 当前定义；逐项目读取 `领导重要变化基线（系统）` 和 `02` 的四类来源：项目概况、投资构成、运作模式、主要商务条款。只处理已有且唯一基线、四类来源齐全的项目。
2. 先比较来源原文快照；只有原文变化时才做语义变化判断。`NO_CHANGE` 只刷新来源快照，不覆盖 `01`。
3. `SOURCE_CHANGED` 时生成并写回 `01.项目概况`、`合作条件`、`投资效益分析`，同时保存处理后的来源快照与时间。来源缺失、歧义或 AI 质量不足时保留原值并记录原因。

输出项目/来源完整性计数、NO_CHANGE/SOURCE_CHANGED/跳过数、三项写回数、快照读回和未验证项。C/Q 的上游 AI 退化属于输入质量风险，不得在本成员中静默修饰成事实。
