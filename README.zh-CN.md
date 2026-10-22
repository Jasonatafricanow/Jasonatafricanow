# Jiasen Wang

[English](README.md) | [简体中文](README.zh-CN.md)

我开发门店经营软件，以及能够长期保留有用状态的 Agent 系统。作品包括线上商城、POS 与 Shopify 迁移端，家庭营养记录工具，以及本地推理和记忆系统。

## 主要作品

| 项目 | 具体成果 |
| --- | --- |
| [TradingWEB](https://github.com/Jasonatafricanow/TradingWEB) | 线上商城、支付和库存，支付最终确认在事务内执行。 |
| [TradingWEB POS](https://github.com/Jasonatafricanow/TradingWEB-POS) | 离线收银客户端，支持幂等补单与硬件适配。 |
| [ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge) | CSV 迁移发送端，由 TradingWEB 校验并提交导入记录。 |
| [Nutrition Tracker MCP](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP) | Agent 饮食记录、可追溯修正及 SQLite 家庭仪表盘。 |
| [LocalModelService](https://github.com/Jasonatafricanow/LocalModelService) | 安装 U 盘与本地推理盒子原型，带 WhatsApp 客服模块。 |
| [AutoRoute Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway) | 按配置顺序回退供应商，隔离凭据并约束流式提交。 |
| [Statebar MCP](https://github.com/Jasonatafricanow/Statebar-mcp) | 通过中文观察规则和确定性对账维护用户当前状态。 |
| [Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime) | 持久 Agent 状态，包含准入、提交／中止和重启恢复。 |
| [MR-Mem](https://github.com/Jasonatafricanow/MR-Mem) | 语义记忆 sidecar，保留指向宿主原始历史的来源关系。 |
| [LCE](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine) | 长期结构研究，保留负结果并验证重放。 |
| [MR Habitat](https://github.com/Jasonatafricanow/MR-Habitat) | 用程序化资产展示模拟 Agent 活动的交互式 3D 小屋。 |

## 界面预览

[![Nutrition Tracker demo dashboard](https://raw.githubusercontent.com/Jasonatafricanow/Nutrition-Tracker-MCP/638ce240cb69680cacdb20673e8e60650595e863/assets/dashboard-preview.jpg)](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP)

[![MR Habitat public cabin prototype](https://raw.githubusercontent.com/Jasonatafricanow/MR-Habitat/247dea6e2bd06427681f2acd7c8a44aa2ff601f4/docs/screenshots/desktop.png)](https://github.com/Jasonatafricanow/MR-Habitat)

Nutrition Tracker 使用种子演示数据；Habitat 展示公开的模拟事件原型。各仓库提供本地运行说明和验证入口。

## 贯穿项目的工程问题

重试不应重复销售；旧观察不应覆盖新状态；派生记忆应能追溯来源。这些问题反复出现在不同项目中，[架构笔记](ARCHITECTURE-JOURNEY.md)记录了相应取舍。

## 证据与范围

这些仓库包含经过测试的原型、基础设施和研究项目，部署成熟度各不相同。公开测试和截图不代表真实门店验收或外部研究收益已经验证。部分项目以清理后的基线发布，仓库时间反映公开时间，不是完整开发历史。

Python · TypeScript · React / React Native · FastAPI · Next.js · SQLite · MySQL · MCP
