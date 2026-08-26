# 定时 workflow 目录

来源：Base `https://foix66zekl.feishu.cn/base/HndRbPcSda4E16saPa3cmtgZnHc` 于 2026-08-26、revision 118 的 `+workflow-list`/`+workflow-get` fresh read。`start_time` 是定义中读回的起始值；平台是否把日期当首次运行日需另测。

| 成员 | workflow | 状态/时间 | 输入门 | 主要输出 | 证据边界 |
|---|---|---|---|---|---|
| `overdue-candidate-reminder` | `wkfJfjs68UuATwzl` 待确认候选逾期提醒 | enabled / DAILY 17:05 | `05`：确认状态=待确认、责任人非空、待通知=false | 严格逾期才通知责任人；写待通知锁 | 配置与历史消息有证据；自然时刻长期稳定未证明；法定节假日未纳入 |
| `weekly-report-synthesis` | `wkfLhxcU7iU3S0gU` 周报提炼与候选生成 | enabled / DAILY 21:00 | `03`：已提炼=false、项目自动关联非空 | `04` 项目周总报/槽位，`01` 周级字段，`05` 待确认或映射失败候选，标记 `03` 已提炼 | 受控全流程和当前定义有证据；自然调度长期稳定未证明 |
| `completion-aggregation` | `wkfVPIfLSLEBgHyZ` 周报按小类归集 | enabled / DAILY 22:00 | `03`：已提炼=true、已归集=false、项目自动关联非空 | 按所选大类限定，AI 仅 UPDATE 时写 `02.完成情况`；标记已归集 | 受控链路有证据；C/Q 的 AI 输出质量仍需观察 |
| `next-plan-aggregation` | `wkf2rnPTlPB59l22` 下周工作目标按小类归集 | enabled / DAILY 22:10 | `03`：已提炼=true、计划已归集=false、项目自动关联非空 | 按所选大类合并写 `02.下周工作目标`；标记计划已归集 | 当前定义已读回；长期自然运行未单独证明 |
| `cumulative-progress` | `wkfJ1qtMnfTZtvon` 项目累计进展生成 | enabled / DAILY 22:30 | `01` 项目非空；检查 `04/02` 是否晚于累计生成锚点 | 有变化才 AI 重算 `01.累计进展情况说明`，写生成时间并清 `累计待更新` | 受控写回成功；锚点与历史全量读回需持续监测 |
| `master-data-sync` | `wkfvdxKwo0RBHPOl` 项目基础资料同步 | enabled / DAILY 23:00 | `01` 项目 + 唯一基线；四类 `02` 来源齐全；原文先变化 | 语义变化才写 `01.项目概况/合作条件/投资效益分析`，保存来源快照 | 受控运行有证据；来源完整性和 AI 生成质量是风险点 |
| `leadership-change-notification` | `wkfMX4NEjcOGVvCZ` 领导重要变化统一判定与通知 | enabled / DAILY 23:45 | `01` 最终口径 + 系统基线；先处理待发送重试 | `NOTIFY/NO_CHANGE`、新增/删除处理、基线更新、梁铭消息和发送锁 | 受控消息读回有证据；外部 Agent 消息能力未验证 |
| `legacy-weekly-plan-loop` | `wkfxteIYYrG7IS6j` 周报计划闭环（上周计划对比） | **disabled** / DAILY 21:20 | 计划未提炼周报 | 旧版写 `01/04` 计划字段 | 历史定义只作对照；不得启用，现行链路由 G 的字段/公式承接 |

## 依赖顺序（只作参考）

自然时间槽的业务顺序是 `G → C/Q → A → E → F`；17:05 逾期提醒是独立支线。总包不自动串联，成员运行仍按当前输入和读回决定。
