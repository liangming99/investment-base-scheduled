---
name: investment-base-scheduled
description: 手动执行和核验「市场开发重点项目周报管理（V2）」的定时工作流家族。
disable-model-invocation: true
---

# 投资项目 Base 定时工作流

这是一个候选家族包，当前只接受用户手动调用。它把 Base 的 `TimerTrigger` 链路整理成可迁移的成员 skill；不把事件驱动的即时 workflow 改写成定时任务，也不声明外部调度已经可用。

## 通用运行上下文

本包只需要可访问的飞书多维表格和当前用户明确选择的成员，不依赖发送端项目仓库的控制文件或固定本地路径。

- Base 地址：`https://foix66zekl.feishu.cn/base/HndRbPcSda4E16saPa3cmtgZnHc`
- Base 名称：`市场开发重点项目周报管理（V2）`
- 运行时必须具备：读取 Base schema/workflow/记录的权限；只有在成员合同要求时才执行写回或发消息
- 默认时区：以线上 Base 实际读回为准；当前基线为 `Asia/Shanghai`

## 路由

先按 [run-contract.md](references/run-contract.md) 使用上方地址 fresh-read 当前 Base。用户必须明确选择一个成员；总包不自动串联成员。

成员入口：

- [weekly-report-synthesis](members/weekly-report-synthesis/SKILL.md)：21:00 周报凝练与 `05` 候选
- [completion-aggregation](members/completion-aggregation/SKILL.md)：22:00 完成情况按小类归集
- [next-plan-aggregation](members/next-plan-aggregation/SKILL.md)：22:10 下周目标按小类归集
- [cumulative-progress](members/cumulative-progress/SKILL.md)：22:30 累计进展
- [master-data-sync](members/master-data-sync/SKILL.md)：23:00 基础资料同步
- [leadership-change-notification](members/leadership-change-notification/SKILL.md)：23:45 领导重要变化判定与通知
- [overdue-candidate-reminder](members/overdue-candidate-reminder/SKILL.md)：每日 17:05 逾期待确认候选提醒
- [legacy-weekly-plan-loop](members/legacy-weekly-plan-loop/SKILL.md)：已禁用的 21:20 历史闭环，仅供对照，不执行

## 共同边界

新增项目、改名、选项/基线同步、项目模板初始化、人工确认写回、映射失败通知和阻塞通知继续由 Base 内置即时 workflow 负责；对应清单见 [authority-and-boundaries.md](references/authority-and-boundaries.md)。

## 统一交付

每次成员运行都返回：当前定义与时间、输入筛选与数量、动作/写回数量、消息回执、幂等结果、完整读回、未验证项和下一步。没有可证明的当前定义、权限或输入时停止，不猜 ID、不启用 disabled workflow、不把本地检查写成业务验收。

路由边界样例见 [evals/route_cases.json](evals/route_cases.json)，输出风险见 [reports/output-risk-profile.md](reports/output-risk-profile.md)。
