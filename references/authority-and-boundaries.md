# 权威与边界

## 当前对象

线上 Base 地址：`https://foix66zekl.feishu.cn/base/HndRbPcSda4E16saPa3cmtgZnHc`。对象为 `市场开发重点项目周报管理（V2）`，revision `118`，时区 `Asia/Shanghai`，fresh read 日期 `2026-08-26`。当前 workflow 共 17 条：15 enabled、2 disabled；其中 8 条为 `TimerTrigger`。

表关系的业务语义以本包列出的约束和线上 Base 为准：稳定项目身份是 Base record identity/`项目关联`；项目名称、简称是显示或兼容查找键。外部移植若要改成稳定关系，必须先读当前 schema 并单独验证，不得在 skill 中静默改写业务契约。

## 留在 Base 内置即时链路

以下是事件驱动或人工确认，不纳入本家族的定时成员：

- `wkfMTvjL1oeNEXRX` 周报自动关联项目（按简称）
- `wkfokIqAekeOghJE` 项目选项（简称）与领导口径基线同步
- `wkfmDixAKRbuslsE` 项目推进模板初始化（确认后按模板幂等）
- `wkfzj1NfGe6CvnQl` 推进候选确认责任人回退
- `wkfgoAEG9Sh4kRpU` 推进状态候选人工确认写回
- `wkfOmld9ZYzxueaQ` 可行动异常统一通知
- `wkfN3aVkIFepXoRA` 推进项阻塞异常通知
- `wkf4dj0bv26urBmM` 项目基线名称同步

因此，“新增项目后同步选项/基线”“改名”“人工确认”等继续走内置 workflow；本包只承接 TimerTrigger 的周期职责。

历史 `[TEST][E2E-*]` 克隆不属于当前 17 条 live workflow；若在旧快照中出现，只作审计/回滚参考，外部 Agent 不得启用或执行。

## 业务验收边界

本包完成只证明本地文档结构、当前配置清单和手动路由草案；不改变 `TECHNICAL_READY / BUSINESS_ACCEPTANCE_PENDING`，不证明外部 Agent 的 provider/权限/出网/消息能力，也不替代真实多账号走查。
