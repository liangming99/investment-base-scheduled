# 评测状态

当前总包与成员均为 `disable-model-invocation: true`，因此本轮不把 trigger 分数伪装成自动路由证据。`route_cases.json` 先固定定时/即时边界；当任一成员改为模型可自动发现时，再用 `yao-meta-skill/scripts/trigger_eval.py` 对该成员单独跑正例、负例、近邻和对抗集。

必要的当前检查：

```bash
python3.11 <yao-meta-skill-dir>/scripts/validate_skill.py <package-dir>
python3.11 <yao-meta-skill-dir>/scripts/resource_boundary_check.py <package-dir>
```

其中 `<yao-meta-skill-dir>` 是运行主机上已安装的 `yao-meta-skill` 根目录；它不是本包的运行时依赖。另一台主机只需保留包本身、Base 地址和可用的飞书 Base connector/API。
