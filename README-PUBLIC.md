# ScaleBench v1.5 —— ARM 公开基准（裁判权基石）

> **测的是"自动重整化"**：给定微观模拟器，机器能否自己发现宏观定律、
> 并诚实地给出适用边界与误差界。防刷分是设计目标，不是事后补丁。
> 榜单唯一准绳：`leaderboard.json`（当前 overall **0.9920**，13 题）。

## 这是什么

ScaleBench 是 LSWM/ARM 项目的公开基准。五个任务族对应"自动发现有效理论"的
五种失败模式；每题都有防刷分机制（见下表"防什么"列）。所有考题数据来自
仓库内模拟器（可复现、带解析参考），不依赖外部数据。

## 任务与当前得分

| 任务族 | 考题 | 得分 | 防什么 |
|---|---|---|---|
| Known-Recovery | `known_recovery_kpp`：恢复 KPP 行波律 c=2√(rD) | 0.998 | "看起来对"——系数误差曾被 FD 滞后压到 0.290；EvS 渐近外推修复后 0.004 |
| Cross-Resolution | `cross_resolution_kpp`：3 种网格同一宏观律 | 1.000 | 过拟合网格 |
| Long-Horizon | `longhorizon_closure_drift`：闭合 100× 视界漂移审计 | 1.000 | 短程拟合（未标定闭合必然低分） |
| Discovery | `spectrum_law_turb2d`：湍流有效谱斜率 + 双级联 | 1.000 | 教科书复读——评 regime 定律与 theory_gap |
| Discovery | `m3_partition_law`：跨域塔分配律 γ（审计账发现） | 0.976 | 闭式代案——发现数据取自守恒审计账 |
| Extrapolation | `extrapolation_kpp`：校准域外 6 配置对拍 | 0.960 | 定律 vs 插值的分水岭；求解器适配 + EvS 渐近测量同口径 |
| Discovery | `nis_prune_consistency`：NIS 剪枝维度一致性 | 1.000 | 几何给定宏观态——k 由数据定，序参量可重构 |
| Discovery（混沌审计） | `chaos_audit_gate`：混沌指标验收门 | 0.994 | 混沌约束进验收（Lyapunov/吸引子距离卡指标化） |
| Discovery（RG 不变量） | `rg_invariant_sir`：异质混合粗粒化不变量判定 | 0.997 | 攻击率 ε-collapse 对拍解析解；峰压峰时判为非不变量 |
| Long-Horizon（闭合救援） | `closure_rescue_sir`：在线微调救援判定 | 1.000 | SPSA 目标 −58.8%；held-out 救援 23.6%；表示层上界比 2.64× 自报 |
| Extrapolation（湍流区制） | `extrapolation_turb2d`：谱律跨耗散区制对拍 | 1.000 | ν×4 斜率 −2.019→−2.265 单调变陡检出；逐探针偏差上报 |
| Long-Horizon（闭环切换） | `closure_switch_rescue_sir`：政策中途切换的在线适应 | 0.971 | 切换救援 86.7%；反空洞自证（i_switch=0.206、峰压 ×0.72）；稳定性相对差 9.7e-5——空洞考场零分 |
| Long-Horizon（区制移位） | `regime_shift_adaptation`：门控在线适应族判决 | 1.000 | 族平均改善 38.1%、最差 0.0%（残差平静期门不开）；无门 RLS ts=2 改坏 −1.9 倍定罪 |

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
- 未满分的题是物理难度的如实记录；而曾经的测量伪影也被同样诚实地修复：
  KPP 题的 FD 拉拽前锋滞后（曾压到 0.826）归因结果是 pulled front 代数暂态
  （不是离散误差），用 Ebert–van Saarloos 渐近估计量修正——系数误差
  0.290 → 0.004，得分 0.826 → 0.998（2026-09-25，事件全程入测试档案）；
- Extrapolation 族目前只有 KPP 一题，更多域（湍流区制外推、跨域塔参数）
  在路线图上。

*ScaleBench 是 LSWM 世界模型的裁判权组件（白皮书 §6）。治理：基准与理论卡格式开源——用开放换标准地位。*
