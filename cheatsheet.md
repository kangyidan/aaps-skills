# 速查表（Cheatsheet）

> 高频查询的压缩速查。详细内容见对应章节。

## A. 目标系统时间表（Objectives 1–11）

| 目标 | 名称 | 最短等待 | 解锁 |
|------|------|---------|------|
| 1 | 设置可视化与监控 | 1h | NS 连接 |
| 2 | 学习控制 AAPS | 2h | TT/闭环对话框 |
| 3 | 证明知识 | ≤14d | 开环前置 |
| 4 | 开始开环 | 7d | 手动 TBR 建议 |
| 5 | 理解开环 | 7d | TBR 来源分析 |
| 6 | LGS 开始闭环 | 5d | maxIOB 强制 0 |
| 7 | 调整闭环 maxIOB>0 | 1d | 闭环 |
| 8 | 调基础率 + Autosens | 7d | Autosens 自动调 |
| 9 | 启用 SMB | 28d | 超微大剂量 |
| 10 | 自动化 | 28d | Automation |
| 11 | 动态 ISF | 28d | DynISF |

> 目标 1–8 ≈ 6 周；9–11 各 28d+。每完成一目标导出备份。

## B. 默认临时目标（TT）

| 预设 | 目标 (mg/dl / mmol/l) | 时长 |
|------|----------------------|------|
| 即将进食 Eating Soon | 72 / 4.0 | 45 min |
| 活动 Activity | 140 / 7.8 | 90 min |
| 低血糖 Hypo | 125 / 6.9 | 45 min |

TT 范围：最低 4/72，最高 15/225。约 30min+ 才达预期 BG。

## C. 常用 SMS 命令

```
BOLUS 2             → 注 2U 大剂量
CARBS 20            → 宣布 20g 碳水（须与 BOLUS 分开发！）
BOLUS 0.6 MEAL      → 注 0.6U + Eating Soon TT
BASAL 0.3           → 0.3 U/h 起 30min
BASAL 30% 50        → 30% 基础率 50min
BASAL STOP          → 停 TBR
LOOP STATUS         → 查状态
LOOP STOP           → 闭环禁用（泵回预设 BR）
LOOP START          → 启闭环
LOOP SUSPEND 20     → 暂停 20min
LOOP RESUME         → 恢复
TARGET MEAL         → 设 Eating Soon TT
TARGET HYPO         → 设 Hypo TT
TARGET STOP         → 取消 TT
PUMP                → 泵状态/IOB/余量/电池
```
- 远程大剂量**仅 SMS**；两次最小间隔 15min（缩短需两授权号）。
- 敏感指令需一次性密码 + PIN；两手机时间同步。

## D. 兼容泵速查

| 泵 | 通信 | 要点 |
|----|------|------|
| Combo / v2 | 蓝牙 | 仅 1 号 Profile；勿泵上手设 TBR；TBR 幅度 500% |
| Insight | 蓝牙 | 按需连接省电；TBR 模拟破 250% |
| DanaR/RS/i | 蓝牙 | BR 步进 0.01；换手机手动重配对 |
| Diaconn G8 | 蓝牙 | 扩展大剂量关闭环 |
| EOPatch2 | 蓝牙 | 最小 0.05U；贴 ≤84h |
| Medtrum Nano | 蓝牙 | 不支持 0U/h BR 方案；自动 DST |
| Omnipod DASH | BLE | 最低 BR 0.05；贴 ≤80h |
| Omnipod Eros | RileyLink | 超范围不停输；时间偏差>5min 须重设 |
| Medtronic | RileyLink/GNARL | 远程模式+ID；NS 序列号须保密 |

## E. 兼容 CGM 速查

- 可信源：Dexcom G5/G6/G7、Libre 2/3、Eversense、MM640g、PocTech、BYODA、Juggluco、xDrip+（作源/报警）。
- **不受信**：xDrip+ Companion、Follower（640G/Eversense）。
- 不推荐 NS 作闭环 BG 源（依赖网络）。
- xDrip+ 须**禁"上传治疗方案"**；校准在趋势水平时做；Libre 用户黄色三角常亮属正常。

## F. 故障排查清单

| 现象 | 原因 / 处理 |
|------|------------|
| 🔴 红色三角（重复 BG） | 多源上传同 BG 到 NS（如 BYODA+xDrip+）→ 删重复条目；闭环禁用直到解决 |
| 🟡 黄色三角 | BG 间隔不稳/缺失，多因 Libre 或换 G6 发射器；闭环仍运行；约 30h 消失 |
| 闭环已禁用（开发版） | 重命名日志为 `engineering_mode`（无扩展名） |
| 与 RileyLink 断连 | 查手机电池优化/蓝牙；泵不可达阈值设 120min |
| 卡在目标 | 文档关键词搜索 + Facebook/Discord；升级可能新增目标问题 |
| 升级后 IOB/COB 清零 | 大版本数据不迁移；无活性胰岛素/碳水时升级 |

## G. 安全红线（必记）

- ⛔ 只用 FDA/CE 认证泵与 CGM，禁改装/自制。
- ⛔ 禁服 SGLT-2 抑制剂（格列净类）。
- ⛔ 自行构建 APK，同 keystore 签名升级。
- ⛔ 每次目标/重大更改后导出备份；主密码必须备份。
- ⛔ 单一数据源：仅 AAPS 上传 NS。

## H. 关键公式

- **ISF 估算（1700 规则）**：`ISF ≈ 1700(mg/dl) / TDD` 或 `94(mmol/l) / TDD`
- **maxIOB 起点**：`平均餐时大剂量 + 3 × 最大每日基础率`
- **DynISF**：`ISF = 1800 / ((TDD × DynISF调整因子) × Ln((当前BG / 胰岛素除数) + 1))`
- **min_5m_carbimpact**：AMA=5，SMB=8
- **Android 要求**：AAPS 3.4 需 Android 12+（新机首选 16）
