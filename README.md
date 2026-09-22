# Jiasen Wang

I build software across commerce, AI infrastructure, and long-lived agent systems.

Most of my recent work is around persistent agent state, restart/recovery behavior, and longitudinal processing. The projects below are intentionally different in scope; some are operational software, some are infrastructure, and some are research prototypes.

## Projects

### Long-lived agents

**[Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)** — Python runtime for persistent AI agents. It includes SQLite-backed facts/state/intents/checkpoints, explicit runtime bindings, restart recovery, bounded memory retrieval, telemetry, and a read-only Observation Window. Optional Qdrant/FastEmbed adapters sit behind the retrieval interface.

**[LCE — Longitudinal Cognition Engine](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine)** — Python research system for building longitudinal structures from historical evidence. The current pipeline compiles semantic blocks, builds vector projections and local overlapping structures, creates revisable candidate interpretations, and exposes accepted revisions through a deterministic read API. The repository keeps failed experiments, temporal cutoffs, negative controls, invalidation, and recovery tests.

**[Statebar MCP](https://github.com/Jasonatafricanow/Statebar-mcp)** — Small MCP/REST service for short-lived user state. Extracted observations are checked against evidence priority and semantic time before deterministic reconciliation updates SQLite state. A false-positive sleep bug is kept as a regression case: mentioning sleep must not by itself mark the user as sleeping.

**[MR Habitat](https://github.com/Jasonatafricanow/MR-Habitat)** — React/Three.js prototype that renders agent activity and state in a small 3D environment. The current prototype is read-oriented and mock/authored-event driven; it does not write agent state back to MR.

### AI infrastructure

**[AutoRoute Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway)** — OpenAI-compatible multi-provider gateway. Provider, credential, model capability, health, and quota are tracked separately. Requests are filtered by capability before scoring, and streaming fallback stops once output has been committed to the caller.

**[LocalModelService / OpenClaw-CS](https://github.com/Jasonatafricanow/LocalModelService)** — FastAPI/Ollama model service with OpenAI-compatible endpoints, SSE streaming, text/vision configuration, tools, and optional business adapters.

### Commerce and operations

**[TradingWEB](https://github.com/Jasonatafricanow/TradingWEB)** — Next.js/TypeScript commerce backend covering products, customers, orders, payments, admin/RBAC, audit, POS APIs, and Shopify migration. Payment webhooks are reconciled as asynchronous state changes; POS writes use stable client references for retry safety.

**[TradingWEB POS](https://github.com/Jasonatafricanow/TradingWEB-POS)** — Expo/React Native offline-first POS. It keeps local pending work separate from server-accepted orders, resynchronizes with idempotent client references, and isolates scanner/printer implementations behind hardware adapters.

**[ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge)** — Shopify CSV migration tool for TradingWEB. It parses and sanitizes source files, validates products/customers/orders/inventory together, checks references, and only then submits batches to the TradingWEB import API.

## Development notes

The projects were built for different problems, but several implementation concerns recur: idempotent retries, restart recovery, explicit state transitions, and keeping derived/model-produced data separate from committed application state.

A short project-by-project history is in **[Development Notes](ARCHITECTURE-JOURNEY.md)**.

## Stack

Python · TypeScript · FastAPI · Next.js · React · React Native / Expo · SQLite · MySQL · MCP · OpenAI-compatible APIs · Ollama · pytest · mypy · Ruff · Vitest · Playwright

## Publication note

Some older projects were published as cleaned repositories after substantial local development. Their public commit dates therefore do not represent the complete development timeline.
