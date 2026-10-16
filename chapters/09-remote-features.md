# 09 · 远程功能（Remote Features）

> 覆盖 SMS 命令、Nightscout 远程、AAPSClient（跟随者）、Android Auto、纯跟随（FollowingOnly）。远程控制须先有稳定同步与应急计划。

## 9.1 SMS 命令（短信远程）

### 安全前提
- 启用短信通信器后，远程手机可能被盗/被他人用——至少 PIN 锁，强烈建议强密码/生物识别，且**不同于 AAPS 主密码**。
- 建议授权**至少两个**电话号码：主号被盗可用第二号禁用短信通信器；确认短信也发两号防一机被盗。
- 敏感指令须用**带时间的一次性密码（authenticator）** + PIN。
- 两手机时间须同步（自动网络时间），否则验证失败。

### 关键规则
- **远程大剂量只能经 SMS**（Nightscout / AAPSClient 不能给 bolus）。
- 两次远程大剂量最小间隔默认 15 分钟；缩短须至少两个授权号。
- 经 SMS 注大剂量后，**碳水须单独发**（第二条 SMS / AAPSClient / NS）！否则 IOB 对而 COB 偏低→AAPS 认为 IOB 过多→不执行修正大剂量。
- 指令英文、不区分大小写；不能一条 SMS 同时发碳水+胰岛素，须分开。
- 保守起步：注比 IC 计算略少的胰岛素（不计当前 BG/趋势），防低血。

### 常用 SMS 命令速查
| 命令 | 示例 | 作用 |
|------|------|------|
| `BOLUS x` | `bolus 2` | 注 2U 大剂量 |
| `CARBS x` | `carbs 20` | 宣布 20g 碳水 |
| `BOLUS x MEAL` | `bolus 0.6 meal` | 注 0.6U + 设 Eating Soon TT |
| `BASAL x` / `BASAL x min` | `basal 0.3` / `basal 30% 50` | 起 0.3U/h 30min / 30% 50min |
| `BASAL STOP` | — | 停 TBR |
| `LOOP STATUS` | — | 查状态 |
| `LOOP STOP/DISABLE` | — | 泵回预设 BR，闭环禁用 |
| `LOOP START/ENABLE` | — | 启闭环 |
| `LOOP SUSPEND 20` | — | 暂停 20 分钟 |
| `LOOP RESUME` | — | 恢复 |
| `LOOP CLOSED` | — | 当前模式=闭环 |
| `TARGET MEAL/ACTIVITY/HYPO` | — | 设对应 TT |
| `TARGET STOP` | — | 取消 TT |
| `PUMP` | — | 返回泵状态/上次连接/TBR/IOB/余量/电池 |

## 9.2 Nightscout 远程控制

- NS 可下发 TT、未来碳水、Profile 切换、配置修改——AAPS 会据此操作。
- 若用 NSClient 输入 TT/碳水/Profile 切换，须确认同步正常；否则可能有"自我修改"的数据。
- **单一数据源原则**：仅 AAPS 上传，其他设备/应用只读，避免冲突。

## 9.3 AAPSClient（跟随者手机）

- 照护者手机装 **AAPSClient**（须自行构建，不能与 AAPS 同签名同机）。
- 两手机须都用 v1 或都用 v3 同步。
- 功能：设 TT、管大剂量（胰岛素+碳水）、查闭环状态、看治疗记录。
- 仅同步良好（无不需要的 TT/TBR 自改）才建议远程控制。
- 可经 NS 应用向 AAPS 发治疗（AAPS 中 AAPSClient 标签 → 同步选项）。

## 9.4 远程监控应急计划（Remote Monitoring）

- 远程失效（断网/蓝牙丢）时怎么办？AAPS 会用当前配置覆盖泵的 BR/ISF/IC。
- 切到更强配置时**只用临时 Profile 切换（设时长）**，以防远程断连后泵卡在强配置；时长到自动回原配置。

## 9.5 Android Auto

- 把当前状态作消息发到车机 Android Auto 屏。
- AAPS 须配置用系统通知报警；Android Auto 中须为"未知来源"启用通知（开发者模式）。

## 9.6 纯跟随（FollowingOnly）

- 用官方 Dexcom 应用发邀请后卸载官方应用。
- 有"Care"选项卡：设/删 TT、输碳水（不含基础率）。
- xDrip+ 作 NS 跟踪器：收 BG/治疗/基础率（若 AAPS 数据源是 xDrip+ 或能从伴侣应用取 BG）。

## 9.7 远程前提总结
1. AAPS + 跟随手机同步稳定（v1/v3 一致）。
2. 至少两个授权号 + 一次性密码 + PIN。
3. 明确应急计划（远程失联时泵行为）。
4. 远程大剂量只能 SMS；碳水必须单独发。
