# 术语表（Glossary）

> AAPS 文档中常用缩写与概念。查词先看这里。

| 术语 | 全称 / 含义 |
|------|------------|
| **AAPS** | AndroidAPS —— 安卓开源人工胰腺 / 混合闭环系统 |
| **APS** | Artificial Pancreas System，人工胰腺系统（自动把血糖维持在健康范围） |
| **HCL / FCL** | Hybrid / Full Closed Loop，混合闭环（需餐前大剂量）/ 全闭环（无需） |
| **OpenAPS** | AAPS 采用的底层开源算法与功能集（oref0/oref1） |
| **AMA** | Advanced Meal Assist，高级膳食助手（旧算法，2017，不支持 SMB 纠正高血） |
| **SMB** | Super Micro Bolus，超微大剂量。用小剂量快速纠正血糖，不靠提 TBR；须目标 9 |
| **Zero-temp** | 零临时基础率（TBR=0%），SMB 后常触发的安全设计 |
| **UAM** | Un-announced meals，未声明餐食。未输碳水却检测到血糖显著升高（进餐/肾上腺素等） |
| **TBR** | Temporary Basal Rate，临时基础率 |
| **LGS** | Low Glucose Suspend，低血糖暂停。降时减基础；升时仅 IOB 为负才加量（目标 6） |
| **BR** | Basal Rate，基础率（U/h） |
| **ISF** | Insulin Sensitivity Factor，胰岛素敏感系数（1U 降多少血糖） |
| **IC / I:C** | Insulin-to-Carb ratio，碳水系数（1U 覆盖多少克碳水） |
| **DIA** | Duration of Insulin Action，胰岛素作用时长（默认 ≥5h，常 5–9h） |
| **Profile** | 用户基础治疗设置（BR/DIA/IC/ISF/BG 目标） |
| **TT** | Temporary Target，临时目标（短期改 BG 目标，如运动/即将进食/低血） |
| **COB** | Carbs On Board，活性碳水化合物（尚未吸收） |
| **IOB** | Insulin On Board，活性胰岛素（含 bolus 与 TBR，不含 Profile 基础率） |
| **CSF** | Carb Sensitivity Factor，碳水系数（吸收 1g 碳水 BG 升多少） |
| **Autosens** | 自动敏感度：据偏差在 24h/8h 窗口算胰岛素敏感性，调基础率+ISF |
| **DynISF / DynamicISF** | 动态 ISF：据 TDD 等动态调整 ISF（与 Autosens 互斥，整合进 SMB 插件） |
| **maxIOB** | 最大活性胰岛素总量上限（安全限制，防超量） |
| **max-basal** | 最大临时基础率上限（U/h，安全限制） |
| **min_5m_carbimpact** | 5 分钟碳水影响最小值（故障安全默认衰减；AMA=5，SMB=8） |
| **eCarbs** | extended Carbs，扩展碳水（分配到数小时，匹配缓吸收餐/药物） |
| **Virtual Pump** | 虚拟泵（AAPS 仅建议不实际输注，用于学习/无驱动泵） |
| **Objectives** | 目标系统（1–11），渐进解锁能力，含强制等待期 |
| **NS / Nightscout** | 报告/远程/可视化服务器，目标 1 必需 |
| **AAPSClient** | 跟随者手机 App（远程监控/控制，须自行构建） |
| **RileyLink** | 桥接设备，让手机经射频与某些泵（Eros/旧 Medtronic）通信 |
| **TDD** | Total Daily Dose，每日总胰岛素剂量 |
| **TBB** | Total Basal per day，24h 基础率总和 |
| **Oref0 / Oref1** | 参考设计实现版本 0 / 1（OpenAPS 核心算法） |
| **Peak time** | 胰岛素最大效应时间 |
| **Automation** | 自动化：条件→操作自动执行（目标 10 解锁） |
| **Autotune** | 基于历史数据的 Profile 自调（OpenAPS 算法实现） |
| **Careportal** | AAPS/NS 中的护理记录区（治疗/事件/备注） |
| **BG** | Blood Glucose，血糖 |
