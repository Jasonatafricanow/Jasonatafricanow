# Jiasen Wang

[English](README.md) | [简体中文](README.zh-CN.md)

I build software for store operations and agents that keep useful state over time. My projects include an online store with a POS and Shopify migration sender, a family nutrition tracker, and local inference and memory systems.

## Selected work

| Project | Concrete result |
| --- | --- |
| [TradingWEB](https://github.com/Jasonatafricanow/TradingWEB) | Online storefront, payments and inventory with transactional payment finalization. |
| [TradingWEB POS](https://github.com/Jasonatafricanow/TradingWEB-POS) | Offline checkout client with idempotent order resubmission and hardware adapters. |
| [ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge) | CSV migration sender that lets TradingWEB validate and commit imported records. |
| [Nutrition Tracker MCP](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP) | Agent meal logging, traceable corrections and a household dashboard backed by SQLite. |
| [LocalModelService](https://github.com/Jasonatafricanow/LocalModelService) | Installer-USB/local-inference appliance prototype with a WhatsApp support module. |
| [AutoRoute Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway) | Configured-order provider fallback with credential isolation and streaming commit rules. |
| [Statebar MCP](https://github.com/Jasonatafricanow/Statebar-mcp) | Chinese observation rules and deterministic reconciliation for current user state. |
| [Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime) | Persistent agent state with admission, commit/abort and restart recovery. |
| [MR-Mem](https://github.com/Jasonatafricanow/MR-Mem) | Semantic-memory sidecar that retains provenance to the host’s original history. |
| [LCE](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine) | Longitudinal-structure research with retained negative results and replay checks. |
| [MR Habitat](https://github.com/Jasonatafricanow/MR-Habitat) | Interactive 3D cabin that presents mock agent activities using procedural assets. |

## Interface previews

[![Nutrition Tracker demo dashboard](https://raw.githubusercontent.com/Jasonatafricanow/Nutrition-Tracker-MCP/638ce240cb69680cacdb20673e8e60650595e863/assets/dashboard-preview.jpg)](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP)

[![MR Habitat public cabin prototype](https://raw.githubusercontent.com/Jasonatafricanow/MR-Habitat/247dea6e2bd06427681f2acd7c8a44aa2ff601f4/docs/screenshots/desktop.png)](https://github.com/Jasonatafricanow/MR-Habitat)

Nutrition Tracker uses seeded demo data; Habitat shows the public mock-driven prototype. Open each repository for local run instructions and its verification surface.

## Engineering thread

Retries should not duplicate sales. An older observation should not overwrite a newer one. A derived memory should stay traceable to its source. These are the recurring problems behind the projects; [architecture notes](ARCHITECTURE-JOURNEY.md) connect the decisions.

## Evidence and scope

The repositories span tested prototypes, infrastructure and research; deployment maturity varies. Public tests and screenshots are linked evidence, not claims of real-store acceptance or externally validated research gains. Several projects were published as cleaned baselines, so repository dates are publication dates rather than full development history.

Python · TypeScript · React / React Native · FastAPI · Next.js · SQLite · MySQL · MCP
