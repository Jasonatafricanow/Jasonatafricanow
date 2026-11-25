# Jiasen Wang

**AI-native system architecture, agent infrastructure & foundational research** alongside **end-to-end operational software delivery**.

My work centers on a single question across both tracks: **how stateful systems preserve authority, provenance, and recovery across failures, retries, and time**. Rather than treating larger prompts or looser abstractions as substitutes for contracts, these repositories separate probabilistic generation from deterministic state transitions.

> **Benchmark Highlight — [MR-Mem](https://github.com/Jasonatafricanow/MR-Mem):** Achieved **59.78** overall (**#26** on the official AML 2026 Round 2 Full track leaderboard, published 2026-11-19; 2.50 points behind 5th place at 62.28) and **tied #4** in memory governance (**D1** override `77.6`, **D2** contradiction `76.4`) at ~¥135 total compiler + embedding cost across ~300M tokens.

---

## Core Invariants

Across commerce backends, offline clients, LLM gateways, and persistent agent memory, the same architectural boundaries recur:

```text
temporary work      != committed effect
parsed input        != admitted state
candidate           != accepted state
retrieval           != authority
projection          != source of truth
```

In the AI/agent architecture, execution and authority are explicitly partitioned across independent boundaries rather than collapsed into a single loop:

```text
External events ──▶ Agent-WorkBench SQL (tasks, events, timers, delegation)
                         │
                 durable wake event
                         ▼
                Existing Agent Host (Sessions, Runs, tool permissions)
                         │
          optional bounded context (read-only / gated)
                 ┌───────┴───────┐
                 ▼               ▼
           Mind Runtime        MR-Mem
       (cognitive/affective   (source-backed semantic
        state & wake intent)   memory, Lines & LCE)
```

This diagram defines **ownership boundaries**, not a claim that every repository runs as one monolithic production deployment. Cross-repository integrations (`MR-Mem` [PR #22](https://github.com/Jasonatafricanow/MR-Mem/pull/22), `Mind-Runtime` [PR #67](https://github.com/Jasonatafricanow/Mind-Runtime/pull/67)) and selected `Agent-WorkBench` surface extensions remain under active development. Several repositories also began prior to their public Git history and were released as cleaned baselines.

---

## Track 1 — MR & AI-Native Systems

### Memory, Cognition & State Authority

- **[MR-Mem](https://github.com/Jasonatafricanow/MR-Mem)** — Source-backed semantic memory system that keeps raw messages as factual authority and compiles conversations into traceable semantic blocks (**SBs**), **Lines**, and **LCE** stretches (`cause → who → when → what → outcome`). Every derived summary resolves back to exact source spans. Official AML 2026 Round 2 Full track score: **59.78** (**#26** overall, **tied #4** on D1/D2 governance) on [`feat/aml-20261013`](https://github.com/Jasonatafricanow/MR-Mem/tree/feat/aml-20261013); post-competition readout fixes on `main` raised local BEAM 100K and PersonaMem-v2 original-word evidence recall.
- **[Mind-Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)** — Cognitive and affective runtime harness separating **Soul** (evidence admission, appraisal, affect, relationships, cognitive wake intent, bounded `DecisionContext`) from the Host **Body** (planning, tool permissions, execution). Implements SQLite persistence, isolated runtime bindings, commit/abort turn recovery, and a read-only Observation Window.
- **[LCE — Longitudinal Cognition Engine](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine)** — Research-engineering engine for compiling and revising longitudinal trajectories and Baselines from accumulated semantic blocks without allowing derived structures to self-authorize as new evidence. Built around falsifiable research questions, no-future evaluation, negative controls, and retained negative results.
- **[Statebar-mcp](https://github.com/Jasonatafricanow/Statebar-mcp)** — Lightweight short-lived current-state service (MCP + REST) using source validation, semantic time, deterministic reconciliation, lifecycle rules, and anti-self-pollution boundaries so casual conversation mentions do not overwrite canonical state.

### Workflow Coordination & Model Infrastructure

- **[Agent-WorkBench](https://github.com/Jasonatafricanow/Agent-WorkBench)** — Standalone SQL-backed task, commitment, progressive-delegation (`ADR-0003`), and wake-event workbench for multi-agent hosts. Uses `BEGIN IMMEDIATE` transactions, `expected_version` optimistic locking, requester isolation, a five-view read-only work/life dashboard, and a `pi-workbot` host layer while keeping `MR-Mem` semantic context behind explicit authorization grants.
- **[AutoRoute-Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway)** — Capability-aware multi-provider and multi-credential LLM gateway enforcing `provider != credential != model`, capability filtering before scoring, credential-scoped health, and strict pre-commit vs. post-commit streaming fallback semantics.
- **[LocalModelService](https://github.com/Jasonatafricanow/LocalModelService)** — Local-first FastAPI inference service around Ollama exposing OpenAI-compatible chat/streaming/vision/tool APIs, bearer-authenticated owner-partitioned sessions, loopback-by-default binding, and WhatsApp/installer-USB appliance adapters.

### Applied Local-First Agent Workbenches & Prototypes

- **[facetide-mcp](https://github.com/Jasonatafricanow/facetide-mcp)** — Local-first MCP server (compact 3-tool core reflection contract) and 4-view web workbench (`facetide-web`) for evidence-backed periodic personal reflection, with an optional read-only `MR-Mem` AML adapter.
- **[Atlas](https://github.com/Jasonatafricanow/Atlas)** — Data-free personal text corpus framework with SQLite offline graph storage, entity resolution, pre-send leak-prevention hooks, static lens UI, and a read-only `MR-Mem` reader.
- **[Nutrition-Tracker-MCP](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP)** — Local-first, Agent-first household nutrition and health ledger backed by SQLite, supporting attributed estimates, audit-backed corrections, a compact 4-tool everyday MCP contract, and an explicit trusted-household NAS boundary.
- **[MR-Habitat](https://github.com/Jasonatafricanow/MR-Habitat)** — Bounded 3D spatial agent-presence experiment projecting runtime activity into a lightweight environment using procedural assets and mock events without granting the visualization cognition ownership.

---

## Track 2 — TradingWEB (Operational Delivery & Commerce)

End-to-end production-oriented engineering shaped by real store workflows, hardware constraints, payment callbacks, and first-user iteration:

- **[TradingWEB](https://github.com/Jasonatafricanow/TradingWEB)** — Full-stack storefront and operations backend unifying online orders, store sales, inventory row locks, integer minor-unit money calculation, RBAC/audit logs, POS contracts, and migration admission under a single transactional server authority.
- **[TradingWEB-POS](https://github.com/Jasonatafricanow/TradingWEB-POS)** — Offline-first mobile POS client using `client_ref` idempotency keys, hardware scanner/printer adapter boundaries, and explicit local-pending vs. server-committed state separation so network retries never duplicate orders.
- **[ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge)** — Shopify CSV migration sender that parses, sanitizes, maps stable Shopify source IDs, and submits authenticated envelopes to TradingWEB-owned validation and admission endpoints without direct target-database ownership.

---

## Maturity & Verification Matrix

Each repository maintains its own verification surface and explicit limitations. Maturity labels reflect observable repository evidence rather than uniform production readiness.

| Repository | Track | Stage | Public Evidence & Boundaries |
| --- | --- | --- | --- |
| **[MR-Mem](https://github.com/Jasonatafricanow/MR-Mem)** | Track 1 | Working implementation / research | AML 2026 Round 2 official score **59.78** (**#26** overall, **tied #4** D1/D2 governance); local BEAM/PersonaMem-v2 readout evaluations; `privacy` CI passing; WorkBench adapter [PR #22](https://github.com/Jasonatafricanow/MR-Mem/pull/22) remains Draft with Path-A offline smoke failure |
| **[Mind-Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)** | Track 1 | Research harness / pre-production | Persistent SQLite state, runtime bindings, restart/certification suites, read-only Observation Window; `historical-tools` & `semantic-delta-host` passing while `main` `quality` check currently fails; live causal experience-to-behavior validation and [PR #67](https://github.com/Jasonatafricanow/Mind-Runtime/pull/67) remain open |
| **[LCE](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine)** | Track 1 | Research engineering | `scripts/verify.py`, trajectory runtime Path B, no-future / negative controls, retained negative results, Public Verification CI; external empirical validation remains open |
| **[Statebar-mcp](https://github.com/Jasonatafricanow/Statebar-mcp)** | Track 1 | Active engineering / Beta | MCP + REST service with deterministic reconciliation and regression tests (`CI` green); bounded English rules in [Draft PR #6](https://github.com/Jasonatafricanow/Statebar-mcp/pull/6) |
| **[Agent-WorkBench](https://github.com/Jasonatafricanow/Agent-WorkBench)** | Track 1 | Modular backend / integration in progress | Modular `workdesk` SQLite backend, delegation engine, 5-view dashboard, and `pi-workbot` reference on `main` with **172 passing offline tests** (`quality` CI green); live Telegram bot token deployment and `MR-Mem` pointer consumer remain open |
| **[AutoRoute-Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway)** | Track 1 | Infrastructure project | Capability filtering, credential-scoped health, pre/post-commit stream fallback, `pytest` suite (`CI` green) |
| **[LocalModelService](https://github.com/Jasonatafricanow/LocalModelService)** | Track 1 | Infrastructure project | OpenAI-compatible + native chat/streaming endpoints, owner-partitioned sessions, auth/bind regressions (`CI` green); installer/WhatsApp modules remain prototype scope |
| **[facetide-mcp](https://github.com/Jasonatafricanow/facetide-mcp)** | Track 1 | Tested local-first MCP + workbench | Compact 3-tool MCP server, 4-view `facetide-web` workbench, synthetic-contract tests for optional read-only `MR-Mem` adapter (`Python CI` green); live production validation open |
| **[Atlas](https://github.com/Jasonatafricanow/Atlas)** | Track 1 | Data-free corpus framework | Offline M1/M2 SQLite graph storage, entity index, static lens UI, read-only `MR-Mem` reader, pre-commit leak scanner (`leak-scan` CI green) |
| **[Nutrition-Tracker-MCP](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP)** | Track 1 | Tested portfolio prototype | Compact 4-tool everyday MCP contract, SQLite dashboard, multi-timezone Node tests (`Node tests` green); live NAS/Agent usability validation open |
| **[MR-Habitat](https://github.com/Jasonatafricanow/MR-Habitat)** | Track 1 | Product prototype | Bounded 3D prototype driven by mock activity events (`CI` green); live `Mind-Runtime` wiring intentionally unbuilt |
| **[TradingWEB](https://github.com/Jasonatafricanow/TradingWEB)** | Track 2 | Operational engineering | Transactional order/payment/inventory backend, unit + browser + migration checks (`quality gate` & `verification` green); live payment integration & public demo remain outside public CI |
| **[TradingWEB-POS](https://github.com/Jasonatafricanow/TradingWEB-POS)** | Track 2 | Operational client | Offline-first POS sync with `client_ref` idempotency, hardware adapters, quality gate green; full field-hardware matrix unverified by public CI alone |
| **[ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge)** | Track 2 | Migration boundary | Authenticated CSV sender preserving Shopify source IDs with receiver-owned validation (`CI` green) |

---

## Focus & Engineering Philosophy

> **Build for the original problem, not for the maximum available capability.**

1. **Contracts before capability.** Generating another layer in AI-native software is cheap; proving that a state transition, admission gate, or recovery boundary is necessary is what makes a system dependable.
2. **Keep uncertain things uncertain.** Returning `unknown`, rejecting weak evidence, or surfacing a partial-stream failure is safer than letting a convenient guess persist across restarts.
3. **Recovery semantics define the design.** Retries, offline queues, interrupted runs, source revisions, and replay tests reveal what state truly means—across both physical-store POS checkouts and long-lived agents.
4. **Revalidate abstractions against stronger models.** If a foundation model doubles in capability tomorrow, brittle prompt scaffolding should disappear, while boundaries protecting factual authority, auditability, and rollback must hold.

For a deeper cross-track walkthrough of how these invariants evolved across both lines of work, see **[Architecture Notes Across Two Project Lines](ARCHITECTURE-JOURNEY.md)**.
