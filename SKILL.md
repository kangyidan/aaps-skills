---
name: aaps-skills
description: AndroidAPS（AAPS）开源人工胰腺/混合闭环系统的结构化学习笔记与运维参考。涵盖 AAPS 的心智模型（DIY APS = CGM + 泵 + OpenAPS 算法闭环）、核心概念（HCL/FCL、SMB、TBR、ISF/IC/DIA/COB/IOB/UAM、Autosens/DynamicISF）、目标系统（Objectives 1-11）、设置向导与配置构建器、日常使用、兼容泵与 CGM 速查、远程功能（SMS/AAPSClient/Nightscout）、Autotune 与自动化（Automation）、WearOS、维护与旅行场景。当用户需要查询 AAPS 的安装构建、参数调校、故障排查、泵/CGM 兼容性、闭环算法原理或 DIY 闭环糖尿病管理时使用。
domain: healthcare
version: '1.0'
license: 衍生学习笔记（基于 AndroidAPS 官方中文文档 androidaps.readthedocs.io/zh-cn/latest，仅供个人学习参考，请遵守原文档版权与开源许可）
source: AndroidAPS 官方说明文档（中文版）https://androidaps.readthedocs.io/zh-cn/latest/
disclaimer: 本技能为技术学习笔记，不替代专业医疗建议。AAPS 是未获监管机构批准的 DIY 医疗实验，所有胰岛素治疗决策须与糖尿病医疗团队共同制定。使用后果由用户自行承担。
---

# AndroidAPS（AAPS）知识技能 · aaps-skills

> 本技能把 AAPS 官方中文说明文档**提炼为结构化学习笔记**：保留框架、原则、算法逻辑、参数定义与反模式，**不复制原文段落**。适合 1 型糖尿病患者、照护者、糖尿病教育者、临床医生及 DIY 闭环爱好者按需查询与排障。

## 核心心智模型（Mental Model）

AAPS = **DIY 人工胰腺（APS）= 持续葡萄糖监测（CGM）+ 胰岛素泵 + OpenAPS 算法闭环**

```
        ┌──────────── CGM（血糖数据源）────────────┐
        │  Dexcom / Libre / Eversense / xDrip+ ...  │
        └───────────────────┬───────────────────────┘
                            │ 每 5 分钟血糖读数
                            ▼
        ┌──────────── AAPS（中枢 = 你构建的 App）────┐
        │  OpenAPS 算法：预测未来 BG、计算剂量调整     │
        │  ├─ 目标 Objectives 1→11（渐进解锁能力）     │
        │  ├─ Profile（BR / ISF / IC / DIA / 目标）    │
        │  ├─ SMB / TBR / 零临时基础率 / UAM           │
        │  └─ Autosens / DynamicISF / Automation      │
        └───────────────┬───────────────────────────┘
                        │ 临时基础率 / 大剂量 / SMB 指令
                        ▼
        ┌──────────── 胰岛素泵（执行机构）────────────┐
        │  Combo / Insight / DanaRS / Omnipod / ...    │
        └──────────────────────────────────────────────┘
                        ↕ 双向同步
                  Nightscout（报告/远程/可视化）
```

**三条公理（贯穿全系统）：**

1. **AAPS 不是"自动驾驶"而是"辅助驾驶"** —— 如同飞行自动驾驶不能取代飞行员，AAPS 不能取代你对糖尿病的管理责任。你需要主动排查问题、做决策、与社群协作。
2. **闭环质量完全取决于 Profile 正确性** —— BR（基础率）、ISF（胰岛素敏感系数）、IC（碳水系数）、DIA（胰岛素作用时长）、目标值这五个参数若不准确，再好的算法也会出错。先调好 Profile，再谈优化。
3. **安全通过"目标系统"分阶段解锁** —— 从开环 → 低血糖暂停（LGS）→ 混合闭环（HCL）→ 全闭环（FCL），每个目标有强制等待期（不可绕过），目的是让你在启用更强能力前已掌握底层设置。

## 关键原则（Principles）

- **P1 只用经 FDA/CE 认证的泵与 CGM，禁止改装/自制/损坏设备。** 非官方改装配件会导致数据误差与输注错误，构成重大风险。
- **P2 绝对禁止服用 SGLT-2 抑制剂（格列净类）。** 此类药物不可预测地降低血糖，与"通过降低基础率升糖"的闭环逻辑冲突，可致酮症酸中毒与严重低血糖。
- **P3 自行构建 APK，从不下载。** AAPS 未上架 Google Play，出于法律要求必须自编译。同一密钥库（keystore）签名才能无感升级。
- **P4 每次完成目标/重大更改后导出设置并备份。** 设置文件用主密码加密；忘记主密码 = 无法恢复 Profile 与目标进度。
- **P5 闭环是"旅程"而非"一次性设置"。** 从构建到全闭环约需 4–6 个月；部分目标有强制天数等待（如目标 9/10/11 各 28 天）。
- **P6 单一数据源原则。** 推荐仅由 AAPS 向 Nightscout 上传全部数据，其他设备/应用仅读取不写入，避免重复数据与同步冲突。
- **P7 先开环验证、再上闭环。** 开环模式下 AAPS 给建议、你手动执行，是熟悉系统、验证 Profile 的安全方式。
- **P8 自动化（Automation）不能补偿糟糕的 Profile。** 它用来减少决策疲劳，而非掩盖 ISF/IC/BR 设置错误。启用前应先有稳定的血糖控制。

## 文档导航（章节索引）

| 章节 | 内容 |
|------|------|
| `chapters/01-what-is-aaps.md` | 概念心智模型、组件总览、安全理念、DIY 责任、临床沟通 |
| `chapters/02-components-hardware.md` | 硬件兼容性总览（泵/CGM/手机分类）、虚拟泵、Nightscout/Tidepool 报告 |
| `chapters/03-build-install.md` | 构建 APK、安装、手机省电/后台配置、传输 |
| `chapters/04-setup-config-profile.md` | 设置向导、配置构建器、首选项、Profile 五参数详解 |
| `chapters/05-objectives.md` | 目标系统 1–11 详解、时间要求、解锁能力、回顾 |
| `chapters/06-daily-use.md` | 日常使用：屏幕/IOB/COB/TBR/预测线/闭环状态/BG 警告、大剂量向导、eCarbs、TT、泵断开、治疗修正、儿童 |
| `chapters/07-algorithms-smb-dynisf-autosens.md` | 闭环算法（AMA/SMB/UAM/零临时基础率/Autosens/min_5m_carbimpact/安全限制）+ 动态 ISF |
| `chapters/08-autotune-automation.md` | Autotune 调参 + 自动化（Automation）规则与示例 |
| `chapters/09-remote-features.md` | 远程功能：SMS 命令、Nightscout 远程、AAPSClient、Android Auto、Following |
| `chapters/10-compatible-pumps.md` | 兼容泵详解（Combo/Insight/DanaRS/DiaconnG8/EOPatch2/MedtrumNano/Omnipod DASH·Eros/Medtronic+RileyLink） |
| `chapters/11-compatible-cgms-wearos.md` | 兼容 CGM + WearOS 与自定义表盘 |
| `chapters/12-maintenance-travel-support.md` | 维护（备份/升级）、旅行与特殊场景（时区/运动/饮酒/疾病）、支持与帮助 |
| `glossary.md` | 术语表（AAPS/AMA/Autosens/BR/COB/CSF/DynISF/IC/ISF/LGS/maxIOB/SMB/TBR/TT/UAM…） |
| `patterns.md` | 模式与反模式（剂量计算、闭环算法、Autotune、Automation、故障排查） |
| `cheatsheet.md` | 速查表（泵/CGM 兼容矩阵、SMS 命令、目标时间表、默认 TT、故障排查清单） |

## 如何使用本技能

- **查询概念/术语** → 先查 `glossary.md`。
- **安装构建/硬件选型** → `chapters/02`、`03`、`10`、`11`。
- **参数调校/算法原理** → `chapters/04`、`07`、`08`。
- **日常操作/排障** → `chapters/06`、`12` + `cheatsheet.md`。
- **远程照护** → `chapters/09`。
- **快速参考** → `cheatsheet.md`。

> 注意：AAPS 文档持续更新，本笔记对应 2026 年初的 3.4 版框架；具体数值（如 Android 版本要求、DynISF 行为）以官方文档最新版为准。
