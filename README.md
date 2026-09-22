# Jiasen Wang

I build **AI-native systems and operational software**, with a current research focus on long-lived AI systems around **continuity, authority, and longitudinal understanding rather than larger prompts**.

My public work spans operational products, LLM infrastructure, agent state, runtime architecture, longitudinal cognition research, and bounded product experiments.

## Portfolio map

```text
Operational systems
  TradingWEB
  ├─ TradingWEB POS
  └─ ShopifyDataBridge
          |
          v
LLM infrastructure
  LocalModelService
  └─ AutoRoute Gateway
          |
          v
Agent state
  Statebar MCP
          |
          v
Long-lived agent runtime
  Mind Runtime
  ├─ Observation Window
  └─ MR Habitat
          |
          v
Longitudinal cognition research
  LCE
```

This is a **conceptual development path**, not a literal ordering of public commit timestamps. Several older projects were published later as cleaned baselines.

## Current status

| Project | Current stage | Public evidence boundary |
| --- | --- | --- |
| **TradingWEB / POS / ShopifyDataBridge** | Engineering projects | Runnable repositories with type/lint/test surfaces and explicit domain, offline, migration, and integration boundaries |
| **LocalModelService** | Infrastructure project | Runnable local inference service with OpenAI-compatible API and modular tool/business seams |
| **AutoRoute Gateway** | Infrastructure project | Capability-aware routing implementation with pytest-based routing/fallback verification |
| **Statebar MCP** | Active engineering / Beta | MCP + REST state layer with deterministic reconciliation and regression-tested state semantics |
| **Mind Runtime** | Research engineering / pre-production | Deterministic certification, persistence/restart validation, strict static/runtime checks; live shadow validation remains incomplete |
| **LCE V1** | Research engineering | Public verification gate, synthetic boundary experiments, negative controls, recovery tests, and replication surface; external empirical validation remains open |
| **MR Habitat** | Product prototype | Runnable bounded 3D prototype; current inputs are authored/mock and real MR integration remains open |

These labels describe the evidence currently present in the repositories. They are not claims of equivalent production maturity.

## Selected work

### Long-lived AI systems

**[Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)** — A stateful runtime for long-lived agents. It separates evidence, memory, runtime identity, canonical state, persistence, policy, expression, telemetry, and observation so inference can continue from authorized state rather than reconstructing authority from history.

**[LCE — Longitudinal Cognition Engine](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine)** — A research-engineering system for longitudinal understanding. Its architecture was shaped through failed hypotheses, no-future evaluation, negative controls, adversarial regressions, immutable revisions, and an explicit rule that derived cognition cannot become factual authority by itself.

**[Statebar MCP](https://github.com/Jasonatafricanow/Statebar-mcp)** — A lightweight current-state layer for agents using evidence, semantic time, deterministic reconciliation, lifecycle rules, and anti-self-pollution boundaries.

**[MR Habitat](https://github.com/Jasonatafricanow/MR-Habitat)** — A spatial-presence experiment that projects agent state into a lightweight environment without giving the visualization ownership of cognition.

### LLM infrastructure

**[AutoRoute Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway)** — A capability-aware multi-provider / multi-credential LLM gateway with explicit credential state, fallback semantics, and a streaming commit boundary.

**[LocalModelService / OpenClaw-CS](https://github.com/Jasonatafricanow/LocalModelService)** — A local-first Ollama / FastAPI model service exposing OpenAI-compatible APIs, streaming, vision-capable models, tools, and business adapters behind a replaceable service boundary.

### Operational systems

**[TradingWEB](https://github.com/Jasonatafricanow/TradingWEB)** — A full-stack commerce and operations system covering products, customers, orders, payments, administration, POS contracts, and migration integration.

**[TradingWEB POS](https://github.com/Jasonatafricanow/TradingWEB-POS)** — An offline-first React Native / Expo POS client with idempotent resynchronization, hardware abstraction, staff/shift workflows, and explicit local-vs-server state boundaries.

**[ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge)** — A Shopify-to-TradingWEB migration boundary that parses, sanitizes, validates, maps, and audits data before target admission.

## Verifiable outputs

The portfolio is intended to be inspected rather than accepted from description alone.

| Project | Where to verify |
| --- | --- |
| **Mind Runtime** | Repository tests, strict mypy/Ruff configuration, deterministic certification artifacts, restart/recovery validation, and explicit delivery-gate status |
| **LCE V1** | `python scripts/verify.py`, public research experiments, retained negative results, recovery matrices, and the provider-agnostic replication surface |
| **Statebar MCP** | Regression tests around evidence priority, delayed observations, assistant-message exclusion, semantic admission, and sleep/wake state transitions |
| **AutoRoute Gateway** | pytest / pytest-asyncio routing tests around capability filtering, credential isolation, fallback, and streaming commit behavior |
| **TradingWEB** | TypeScript checks, ESLint, Vitest, Playwright, i18n checks, and migration-reconciliation scripts |
| **TradingWEB POS** | Type checking, Vitest/Jest suites, linting, native bundling, and Android build helpers |
| **ShopifyDataBridge** | TypeScript validation, linting, Vitest, and explicit migration validation/admission paths |
| **MR Habitat** | Local prototype, automated tests/build, verification record, and product-experiment notes |

Each repository README states its own limitations and distinguishes implemented behavior from future work or unverified deployment claims.

## Development and architecture journey

The detailed reasoning is kept outside this landing page:

**[From Operational Software to Long-Lived AI Systems](ARCHITECTURE-JOURNEY.md)** traces the path from commerce, offline recovery, migration, and model infrastructure into agent state, runtime authority, and longitudinal cognition.

Across those projects, several distinctions recur:

```text
candidate != accepted state
retrieval != authority
parsed input != admitted data
local pending work != committed remote effect
projection != cognition
```

The vocabulary changes by domain. The underlying question is the same:

> **What is allowed to become true for the next layer?**

An earlier **[Independent Portfolio Review](PORTFOLIO-INDEPENDENT-REVIEW.md)** covers the MR / LCE / Habitat subset and remains as a conservative historical review.

## Engineering philosophy

> **Build for the original problem, not for the maximum available capability.**

Capability is not the same as usefulness. Architecture should preserve the target function before expanding the feature surface.

## Working stack

Python · TypeScript · FastAPI · Next.js · React · React Native / Expo · SQLite · MySQL · MCP · OpenAI-compatible APIs · Ollama · pytest · mypy · Ruff · Vitest · Playwright

## Publication note

Several projects predate their current public Git repositories. Where relevant, repository READMEs mark cleaned publication baselines so public commit dates are not mistaken for the complete development history.
