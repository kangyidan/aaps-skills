# 02 · 硬件组件与兼容性总览

> 本章概述 AAPS 兼容的泵、CGM、手机分类，以及虚拟泵与报告服务器的角色。具体泵的配置细节见 `chapters/10`，CGM 见 `chapters/11`。

## 2.1 兼容性总原则

- AAPS **仅与特定泵、CGM、手机兼容**，且部分技术因国家/地区不可用。
- 泵通信分两类：
  - **原生蓝牙**：手机直接连泵（DanaR/RS/i、部分 Insight、Medtrum、Omnipod DASH 等）。
  - **需 RileyLink 类桥接设备**：通过射频与泵通信（旧 Medtronic、Omnipod Eros、某些 Insight 场景）。
- 报告服务器（Nightscout）独立于其他模块；完成**目标 1** 必需，但非长期运行强制项。

## 2.2 胰岛素泵（闭环候选）

| 泵 | 通信方式 | 关键说明 |
|----|---------|---------|
| Accu-Chek Combo / Combo v2 | 原生蓝牙（屏幕解析） | 标准鲁尔接口、普通电池；速度较慢但可靠；仅用 1 号 Profile |
| Accu-Chek Insight | 原生蓝牙 | 连接按需建立以省电；支持 TBR 模拟突破 250% |
| DanaR / DanaRS / Dana-i | 原生蓝牙 | 基础率步进 0.01 U/h；RS/i 可配置静音 |
| Diaconn G8 | 原生蓝牙 | 支持记录储药器/针头/管路/电池更换 |
| EOPatch2（贴片） | 原生蓝牙 | 最小剂量 0.05 U；每贴建议 ≤84 小时 |
| Medtrum Nano / 贴片 | 原生蓝牙 | 自动 DST；不支持 0 U/h 基础率方案 |
| Omnipod DASH | 原生蓝牙（BLE） | 无 RileyLink；最低基础率 0.05 U/h |
| Omnipod Eros | RileyLink 桥接 | Pod 3 天/80h；通信中断时按预设基础率继续 |
| Medtronic（旧款） | RileyLink / GNARL | 需远程模式 + 添加 ID；泵序列号敏感，须强化 NS |

> 泵实现闭环的硬性要求：支持远程控制（蓝牙/射频）、可设置/取消 TBR、可获取（最好可设置）基础率 Profile。

## 2.3 CGM（血糖数据源）

- 经监管批准的 CGM 在商用混合闭环中被视为**可信血糖源**。
- 发送读数的应用须能提供**传感器信息**（sensor status）。
- **不受信任的数据源**：xDrip+ 伴侣应用（Companion）、跟随者模式（Follower，含 640G/Eversense）。
- 不推荐把 Nightscout 作为闭环 BG 源（依赖网络稳定）；本地广播的 CGM 更可靠。
- 支持列表（详见 `chapters/11`）：Dexcom G5/G6/G7、Libre 1/2/3、Eversense、MM640g、PocTech、xDrip+、BYODA、Juggluco 等。

## 2.4 手机

- 当前 AAPS 3.4 需 **Android 12.0+**；购新机（截至 2026-01）建议首选 **Android 16**，不推荐 15。
- 出于安全，强烈建议保持构建版本最新（每年约更新一次）。
- 旧 Android 设备可用兼容的早期 AAPS 版本。
- 部分机型（华为、Jelly 等）需特殊省电/后台配置，见 `chapters/03`。

## 2.5 虚拟泵（Virtual Pump）

- 在配置构建器 > 泵中选择"虚拟泵"，AAPS 仅给建议、不实际输注。
- 用途：
  - 完成**目标 1–5**（开环学习）而无需真实泵；
  - 真实泵驱动不可用时的体验/测试；
  - 仍可用其他胰岛素输送方式。
- 切换到真实泵并完成目标 5 验证后，方可进入后续目标。

## 2.6 报告服务器

- **Nightscout**：核心报告+远程+可视化中枢。除报告外还可远程下发 TT、未来碳水、Profile 切换。
- **Tidepool**：仅用于报告（非实时跟踪），不要额外用其上传器（会与 AAPS 重复）。如需其他关注者，仍须配 Nightscout。
- **Open Humans**：可选，用于科研数据捐赠（临时的 CGM/泵/NS 数据复制），随时可停止删除。

## 2.7 组件准备建议

- 准备时间因人而异；可先用虚拟泵 + 开环熟悉系统，同时校验 BR/IC/ISF。
- 别急着一口气配齐——开环阶段就能开始学习并积累数据（Autotune/DynISF 需要历史数据）。
- 每个组件都需单独验证，再组合成闭环。
