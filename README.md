# Jiasen Wang

I build **AI-native systems and operational software** across three layers: long-lived agent runtime, LLM infrastructure, and full-stack product systems.

My projects usually start from a concrete failure mode — state gets lost across conversations, model calls become unreliable, offline checkout breaks, migrations corrupt data, or a system cannot explain why a piece of state is trusted. I turn those problems into explicit boundaries, deterministic contracts, recoverable state, and testable failure semantics.

The common thread is not a single framework or model. It is the attempt to make systems remain understandable when the surrounding capability becomes probabilistic, distributed, or long-lived.

## Selected Work

### Long-lived AI systems

#### [Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)

A stateful runtime for long-lived AI agents. MR separates evidence, memory, runtime identity, canonical state, persistence, policy, telemetry, and observation so that model inference can continue from authorized state instead of reconstructing everything from history on every turn.

Current work focuses on authority boundaries, restart-safe persistence, multi-binding isolation, bounded context compilation, causal tracing, and fail-closed admission.

#### [LCE — Longitudinal Cognition Engine](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine)

A standalone research-engineering project for longitudinal understanding across evidence over time.

LCE treats similarity, vectors, clusters, higher-order structure, and model interpretation as derived evidence rather than truth. Its architecture was shaped through failed hypotheses, no-future evaluation, negative controls, adversarial audits, immutable revision history, and recovery testing.

#### [Statebar MCP](https://github.com/Jasonatafricanow/Statebar-mcp)

A lightweight current-state layer for AI agents, positioned between long-term memory and the active conversation.

It maintains short-lived user state through explicit evidence, deterministic reconciliation, semantic expiration, provenance, MCP stdio / REST transports, and anti-self-pollution rules. Recent work extends it from text extraction toward an evidence-driven state engine.

#### [MR Habitat](https://github.com/Jasonatafricanow/MR-Habitat)

A spatial presence experiment for Mind Runtime.

Habitat projects agent state and activity into a lightweight 3D environment while remaining read-oriented and projection-only: it does not own memory, cognition, or runtime authority.

### LLM infrastructure

#### [AutoRoute Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway)

A capability-aware, multi-provider, multi-credential LLM gateway with OpenAI-compatible APIs.

It separates provider identity from credentials, filters candidates by request capabilities, performs dynamic routing and fallback, and prevents unsafe provider replay after a streamed response has already been committed to the client.

#### [LocalModelService / OpenClaw-CS](https://github.com/Jasonatafricanow/LocalModelService)

A local AI service built around Ollama and FastAPI.

It exposes OpenAI-compatible endpoints, streaming responses, vision-capable local models, tool extensions, and business adapters while keeping deployment lightweight and local-first.

### Product systems

#### [TradingWEB](https://github.com/Jasonatafricanow/TradingWEB)

A full-stack commerce and cross-border trading platform built with Next.js, React, TypeScript, Drizzle, and MySQL.

The repository covers storefront flows, admin operations, product and order domains, payments, i18n, RBAC, audit logging, POS-facing APIs, and migration integration.

#### [TradingWEB POS](https://github.com/Jasonatafricanow/TradingWEB-POS)

An offline-first React Native / Expo POS client connected to TradingWEB.

It includes cart and checkout flows, split payments, returns, inventory operations, staff / shift workflows, hardware abstraction for scanners and receipt printers, secure credential storage, local persistence, and idempotent resynchronization after network recovery.

#### [ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge)

A migration and validation bridge from Shopify exports into TradingWEB.

It handles CSV parsing, sanitization, product / variant / customer / order validation, referential checks, import progress, and migration audit history.

## Portfolio Map

```text
Operational products
  ├─ TradingWEB
  ├─ TradingWEB POS
  └─ ShopifyDataBridge

LLM infrastructure
  ├─ LocalModelService
  └─ AutoRoute Gateway

Long-lived agent systems
  ├─ Statebar MCP
  ├─ Mind Runtime
  │    └─ MR Habitat
  └─ LCE
```

These are different systems, but they repeatedly return to the same engineering questions:

- What is the authoritative state?
- What can be derived but must not become truth automatically?
- What happens after restart, retry, partial failure, delayed evidence, or network loss?
- Which layer owns a decision, and which layer should remain replaceable?
- How much machinery is actually required to solve the original problem?

## Working Stack

Python · TypeScript · FastAPI · Next.js · React · React Native / Expo · SQLite · MySQL · MCP · OpenAI-compatible APIs · Ollama · pytest · Vitest · Playwright

## Engineering Philosophy

> Build for the original problem, not for the maximum available capability.

Capability is not the same as usefulness. I prefer narrow contracts, explicit ownership, inspectable state, reproducible failure cases, and architectures that can survive replacement of the model or framework around them.
