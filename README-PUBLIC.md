# ScaleBench v1.3 —— ARM 公开基准（裁判权基石）

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22933596.svg)](https://doi.org/10.5281/zenodo.22933596)

归档引用：LSWM Project Team. (2026). ScaleBench (v1.3). Zenodo. https://doi.org/10.5281/zenodo.22933596

> **测的是"自动重整化"**：给定微观模拟器，机器能否自己发现宏观定律、
> 并诚实地给出适用边界与误差界。防刷分是设计目标，不是事后补丁。
> 榜单唯一准绳：`leaderboard.json`（当前 overall **0.9556**，9 题）。

## 这是什么

ScaleBench 是 LSWM/ARM 项目的公开基准。五个任务族对应"自动发现有效理论"的
五种失败模式；每题都有防刷分机制（见下表"防什么"列）。所有考题数据来自
仓库内模拟器（可复现、带解析参考），不依赖外部数据。

## 任务与当前得分

| 任务族 | 考题 | 得分 | 防什么 |
|---|---|---|---|
| Known-Recovery | `known_recovery_kpp`：恢复 KPP 行波律 c=2√(rD) | 0.826 | "看起来对"——系数被 FD 系统性滞后如实扣分 |
| Cross-Resolution | `cross_resolution_kpp`：3 种网格同一宏观律 | 1.000 | 过拟合网格 |
| Long-Horizon | `longhorizon_closure_drift`：闭合 100× 视界漂移审计 | 1.000 | 短程拟合（未标定闭合必然低分） |
| Discovery | `spectrum_law_turb2d`：湍流有效谱斜率 + 双级联 | 1.000 | 教科书复读——评 regime 定律与 theory_gap |
| Discovery | `m3_partition_law`：跨域塔分配律 γ（审计账发现） | 0.976 | 闭式代案——发现数据取自守恒审计账 |
| Extrapolation | `extrapolation_kpp`：校准域外 6 配置对拍 | 0.808 | 定律 vs 插值的分水岭；求解器适配是考题内容 |
| Discovery | `nis_prune_consistency`：NIS 剪枝维度一致性 | 1.000 | 几何给定宏观态——k 由数据定，序参量可重构 |
| Discovery（混沌审计） | `chaos_audit_gate`：混沌指标验收门 | 0.994 | 混沌约束进验收（Lyapunov/吸引子距离卡指标化） |
| Discovery（RG 不变量） | `rg_invariant_sir`：异质混合粗粒化不变量判定 | 0.997 | 攻击率 ε-collapse 对拍解析解；峰压峰时判为非不变量 |

## 运行与复现

```bash
cd arm
pip install -e .            # 或 pip install numpy scipy pytest
python -m scalebench.reproduce            # 全量复现 + 与榜单比对（约 3–5 分钟）
python -m scalebench.reproduce --only nis_prune_consistency   # 单题
python -m scalebench.reproduce --report out.json              # 另存比对报告
```

复现脚本逐题比对 `|实测 − 榜单| ≤ 0.02`（确定性管线应严格一致；容差留给
跨平台浮点差异），不一致项非零退出码。

## 外部提交与仲裁

1. 复现公开榜单（reproduce 全绿）是任何外部提交的前置条件；
2. 提交物必须含：ETC 卡（误差界带校准记录）、考题输出、复现日志；
3. 误差界校准按二项 z 核验（SPEC-002）：宣称覆盖率与实测不符 → 降级；
4. 防刷分的最终仲裁题是 Discovery 族——对无教科书答案的系统输出
   带误差界的宏观方程，由 held-out 实验数据仲裁。

## 诚实边界

- 榜单全部数字来自确定性管线（种子固定），跨平台微小浮点差由容差吸收；
- 0.826（KPP）与 0.808（外推）不是失败：前者是 FD 离散滞后的如实记录，
  后者是域外难度的如实记录——ScaleBench 的分数下限就是物理本身的难度；
- Extrapolation 族目前只有 KPP 一题，更多域（湍流区制外推、跨域塔参数）
  在路线图上。

*ScaleBench 是 LSWM 世界模型的裁判权组件（白皮书 §6）。治理：基准与理论卡格式开源——用开放换标准地位。*
