# Jiasen Wang

I build **AI-native systems and operational software**, with a current research focus on
long-lived AI systems around **continuity, authority, recovery, and longitudinal
understanding rather than larger prompts**.

The portfolio spans operational software, AI/agent infrastructure, semantic-memory research, a cognitive/affective harness, and emerging multi-agent applications. The projects share recurring engineering questions about authority and recovery; this is **not** a claim that they share code lineage or are all deployed together.

## Portfolio map

```text
Operational engineering                   AI / agent engineering

TradingWEB                                LocalModelService / AutoRoute Gateway
├─ TradingWEB POS                         Statebar MCP (current-state semantics)
└─ ShopifyDataBridge                      MR-Mem (source-backed semantic memory)
                                         ├─ Thread / retrieval
                                         └─ LCE (optional longitudinal projection)
                                         Mind Runtime (cognitive / affective harness)
                                         Agent-WorkBench (business workflow, in progress)
                                         MR Habitat (bounded spatial experiment)
```

A separate **Agent Host** (existing infrastructure, not a new project in this portfolio)
owns Sessions, Runs, tools, permissions, and actual execution. In the *proposed* integration,
[Agent-WorkBench](https://github.com/Jasonatafricanow/Agent-WorkBench/pull/1) owns
business tasks/events/timers, [Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)
owns cognitive and affective state plus cognitive wake intent, and
[MR-Mem](https://github.com/Jasonatafricanow/MR-Mem) owns semantic memory,
provenance and Thread/LCE projections.

```text
External events --> WorkBench SQL -- business wake event --> existing Agent Host
                         ^                                      |
                         |                                  agent tools
                         +--------------------------------------+
                                                   |
                                   optional cognitive / memory context
                                   Mind Runtime        MR-Mem
```

**This is an ownership diagram, not a deployed integration diagram.** WorkBench's
[design branch](https://github.com/Jasonatafricanow/Agent-WorkBench/tree/feat/modular-backend-20261116),
MR-Mem's [host-adapter PR](https://github.com/Jasonatafricanow/MR-Mem/pull/22)
and MR's [architecture PR](https://github.com/Jasonatafricanow/Mind-Runtime/pull/67)
remain in progress. The operational line covers business state, retries, offline
work and migration; the AI/agent line explores the same questions in model serving,
memory, cognition and execution coordination.

**Applied Agent product:** [Nutrition Tracker MCP](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP) — private family NAS health/nutrition tracking where low-effort input, source-aware corrections and an explicit shared-household trust boundary matter more than maximal feature count.

The connection between the projects is architectural rather than code reuse:

```text
temporary work      != committed effect
parsed input        != admitted state
candidate           != accepted state
retrieval           != authority
projection          != source of truth
```

Several projects predate their current public Git repositories and were later published as
cleaned baselines. Public repository creation dates therefore describe publication, not the
full development chronology.

## Current status

| Project | Current stage | Public evidence boundary |
| --- | --- | --- |
| **TradingWEB / POS / ShopifyDataBridge** | Engineering projects | Runnable repositories with type/lint/test surfaces and explicit transaction, offline, migration and integration boundaries |
| **LocalModelService** | Infrastructure project | Runnable local inference service with OpenAI-compatible API, authentication/session boundaries and modular adapters |
| **AutoRoute Gateway** | Infrastructure project | Capability-aware routing implementation with credential isolation, fallback semantics and pytest verification |
| **Statebar MCP** | Active engineering / Beta | MCP + REST state layer with deterministic reconciliation and regression-tested state semantics |
| **MR-Mem** | Standalone memory research / working implementation | AML 2026 Round 2 official run (project-reported average **54.37**), synthetic and public benchmark runs, source-backed semantic memory and Thread/LCE research; WorkBench host integration remains a Draft PR |
| **Mind Runtime** | Cognitive/affective harness research / pre-production | Persistent state and runtime binding, certification and restart tests; causal/experiential research and live Host integration remain incomplete |
| **Agent-WorkBench** | Early implementation / integration design | Draft PR #1 has SQLite bootstrap, scoped tool contracts, business wake-event ledger and 23 passing offline contract/SQL tests; no concrete production TaskService, live Host/Telegram, or NAS deployment |
| **LCE** | Research engineering | Public verification gate, retained negative results, controlled/synthetic research fixtures, recovery tests and replication surfaces; external empirical validation remains open |
| **MR Habitat** | Product prototype | Runnable bounded 3D prototype; current inputs are authored/mock and real MR integration remains open |
| **Nutrition Tracker MCP** | Tested portfolio prototype | Compact Agent MCP, private NAS household context, editable nutrition records, batch health ingestion, 7/30-day reports; real NAS/Agent usability validation remains open |

These labels describe the evidence currently present in the repositories. They are not
claims of equivalent production maturity.

## Selected work

### Agent workflow and execution continuity

**[Agent-WorkBench](https://github.com/Jasonatafricanow/Agent-WorkBench)** —
An independent, event-driven business workbench for several agents. Its design
uses SQL as the authority for tasks, commitments, dependencies, approval and
reminder history; a durable wake-event inbox/outbox bridges to an existing Agent
Host without implementing a second Agent loop. Semantic context is obtained,
when authorized, from MR-Mem, not copied into the task database.

**Evidence and maturity:** [Draft PR #1](https://github.com/Jasonatafricanow/Agent-WorkBench/pull/1)
includes a [system blueprint](https://github.com/Jasonatafricanow/Agent-WorkBench/blob/feat/modular-backend-20261116/docs/PROJECT_BLUEPRINT.md),
a [cross-repository authority matrix](https://github.com/Jasonatafricanow/Agent-WorkBench/blob/feat/modular-backend-20261116/docs/CROSS_REPO_INTERFACE_MATRIX.md),
a scoped Agent tool façade, SQLite integrity tests and an event ledger.
The feature branch's offline Quality workflow passed **23 tests** at the reviewed
commit. The actual transactional TaskService, authenticated Host/Telegram
integration, deferred-run resumption, dashboard and deployment are **not complete**.
The default branch is intentionally a minimal baseline.

### Applied Agent product

**[Nutrition Tracker MCP](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP)** — A local-first, Agent-first health and nutrition tracker for individuals and a trusted family NAS. Instead of enforcing perfect measurements, it records attributed estimates with a short conversation and allows audit-backed correction. Photo entries receive a single identity/portion confirmation. It exposes two compact default MCP tools, reuses deterministic SQLite calculations, and provides a professional-first dashboard that can be simplified without losing records.

The important engineering decision was to **deliberately remove SaaS-like per-member credential and consent machinery** from a trusted household instance, while retaining remote authentication and documenting the resulting privacy tradeoff. [Product decisions and evidence](https://github.com/Jasonatafricanow/Nutrition-Tracker-MCP#portfolio-overview). CI covers multiple Node versions and time zones; production NAS/Tailscale/Hermes integration and real Agent usability are not yet validated.

### Long-lived AI systems

**[MR-Mem](https://github.com/Jasonatafricanow/MR-Mem)** —
A standalone, provenance-aware agent-memory system. It compiles source evidence
into semantic memory, maintains bounded Thread context and supports longitudinal
LCE projections without promoting retrieval results into factual authority.
The [project README](https://github.com/Jasonatafricanow/MR-Mem#readme) reports
an AML 2026 Round 2 official-run average of **54.37** (2026-11-02) and later local
retrieval/readout evaluations. These are identified benchmark measurements,
not a claim of production memory quality across arbitrary hosts. The
[WorkBench user-context adapter](https://github.com/Jasonatafricanow/MR-Mem/pull/22)
is a Draft integration; its quality/privacy checks pass, while a pre-existing
Path-A offline smoke incompatibility remains.

**[Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)** —
A research-engineering **cognitive and affective harness** for long-lived
agent identity and the effects of experience. Its Soul/Body boundary separates
evidence, appraisal, affect, relationship, internalization, cognitive wake
semantics and bounded DecisionContext from the Agent Host's planning, tool
permissions and real-world execution. Persistence/restart and deterministic
verification are implemented; a complete live causal validation of the
experience-to-behavior chain is **not claimed**. A
[Draft integration design](https://github.com/Jasonatafricanow/Mind-Runtime/pull/67)
clarifies how it relates to WorkBench without making MR a generic Agent OS.

**[LCE — Longitudinal Cognition Engine](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine)** —
A research-engineering system for longitudinal understanding. Its architecture was shaped
through failed hypotheses, no-future evaluation, negative controls, adversarial regressions,
immutable revisions, and an explicit rule that derived cognition cannot become factual
authority by itself.

**[Statebar MCP](https://github.com/Jasonatafricanow/Statebar-mcp)** — A lightweight
current-state layer for agents using evidence, semantic time, deterministic reconciliation,
lifecycle rules, and anti-self-pollution boundaries.

**[MR Habitat](https://github.com/Jasonatafricanow/MR-Habitat)** — A spatial-presence
experiment that projects agent state into a lightweight environment without giving the
visualization ownership of cognition.

### LLM infrastructure

**[AutoRoute Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway)** — A
capability-aware multi-provider / multi-credential LLM gateway with explicit credential
state, fallback semantics, and a streaming commit boundary.

**[LocalModelService / OpenClaw-CS](https://github.com/Jasonatafricanow/LocalModelService)** —
A local-first Ollama / FastAPI model service exposing OpenAI-compatible APIs, streaming,
vision-capable models, tools, and business adapters behind a replaceable service boundary.

### Operational systems

**[TradingWEB](https://github.com/Jasonatafricanow/TradingWEB)** — A full-stack commerce
and operations system covering products, customers, orders, payments, administration, POS
contracts, and migration integration.

**[TradingWEB POS](https://github.com/Jasonatafricanow/TradingWEB-POS)** — An offline-first
React Native / Expo POS client with idempotent resynchronization, hardware abstraction,
staff/shift workflows, and explicit local-vs-server state boundaries.

**[ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge)** — A
Shopify-to-TradingWEB migration boundary that parses, sanitizes, maps and audits source
data before TradingWEB performs target admission.

## How to review the smaller repositories

Repository size is not used here as a maturity metric. Several components are intentionally
narrow because responsibilities that do not belong to them stay outside the repository.

For a compact project, the useful questions are:

```text
What contract does it own?
What state / authority is it allowed to mutate?
What failure semantics are implemented?
What responsibilities are explicitly excluded?
Which regressions prove those boundaries?
```

A small repository with a complete bounded contract is different from an incomplete larger
system. The READMEs therefore state both implemented surfaces and explicit non-goals so the
difference can be inspected rather than inferred from file count or repository size.

## Verifiable outputs

The portfolio is intended to be inspected rather than accepted from description alone.
Each project keeps its own implementation, verification surface, limitations, and current
evidence boundary; reviewers should treat repository code, tests, CI, and current
architecture records as primary evidence.

| Project | Where to verify |
| --- | --- |
| **MR-Mem** | [Main README](https://github.com/Jasonatafricanow/MR-Mem#readme), [technical report](https://github.com/Jasonatafricanow/MR-Mem/blob/main/docs/TECHNICAL_REPORT.md), public/synthetic evaluations and [host adapter Draft PR #22](https://github.com/Jasonatafricanow/MR-Mem/pull/22); note the still-failing Path-A smoke gate |
| **Mind Runtime** | Repository tests/coverage, deterministic certification and restart validation, [Soul/Body architecture freeze](https://github.com/Jasonatafricanow/Mind-Runtime/blob/main/docs/architecture/MR_ARCHITECTURE_LOCK_v1_1.md), [Draft PR #67](https://github.com/Jasonatafricanow/Mind-Runtime/pull/67) and explicit incomplete live causal-validation status |
| **Agent-WorkBench** | [Draft PR #1](https://github.com/Jasonatafricanow/Agent-WorkBench/pull/1): initial schema, workspace-FK and grant tests, wake-event leasing/idempotence/unknown state, [blueprint](https://github.com/Jasonatafricanow/Agent-WorkBench/blob/feat/modular-backend-20261116/docs/PROJECT_BLUEPRINT.md), and known unimplemented production components |
| **LCE** | `python scripts/verify.py`, public research experiments, retained negative results, recovery matrices, and the provider-agnostic replication surface |
| **Statebar MCP** | Regression tests around evidence priority, delayed observations, assistant-message exclusion, semantic admission, replay and sleep/wake state transitions |
| **AutoRoute Gateway** | pytest routing tests around capability filtering, credential isolation, fallback and streaming commit behavior |
| **LocalModelService** | API/session authentication regressions, owner-partitioned sessions, local-bind defaults and CI |
| **TradingWEB** | TypeScript checks, ESLint, Vitest, Playwright, i18n checks, migration replay and integration checks |
| **TradingWEB POS** | Type checking, Vitest/Jest suites, linting, native bundling and Android build helpers |
| **ShopifyDataBridge** | TypeScript validation, linting, Vitest and tests preventing direct target-database ownership |
| **MR Habitat** | Local prototype, automated tests/build, verification record and product-experiment notes |
| **Nutrition Tracker MCP** | Node.js tests and Actions matrix, documented correction/idempotency semantics, family NAS permission tradeoff, product strategy decision record and live-Agent limitations |

Each repository README states its own limitations and distinguishes implemented behavior
from future work or unverified deployment claims.

## Development and architecture notes

**[Architecture Notes Across Two Project Lines](ARCHITECTURE-JOURNEY.md)** explains where
the same state/authority/recovery questions appeared in otherwise independent projects,
including the distinct responsibilities of MR-Mem, Mind Runtime and Agent-WorkBench.
It is not a claim of shared code lineage.

## Engineering philosophy

> **Build for the original problem, not for the maximum available capability.**

Capability is not the same as usefulness. Architecture should preserve the target function
before expanding the feature surface. In AI-native development, generating another layer is
cheap; proving that the layer is necessary is not.

## Working stack

Python · TypeScript · Node.js · FastAPI · Next.js · React · React Native / Expo · SQLite · MySQL · MCP · OpenAI-compatible APIs · Ollama · pytest · mypy · Ruff · Vitest · Playwright
