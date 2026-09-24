# Jiasen Wang

I build **AI-native systems and operational software**, with a current research focus on
long-lived AI systems around **continuity, authority, recovery, and longitudinal
understanding rather than larger prompts**.

The public repositories come from two project lines. They are related by recurring
engineering problems, not by a claim that one codebase literally evolved into the other.

## Portfolio map

```text
Operational engineering                  AI / agent systems

TradingWEB                               LocalModelService
├─ TradingWEB POS                        └─ AutoRoute Gateway
└─ ShopifyDataBridge                            |
                                                v
                                          Statebar MCP
                                                |
                                                v
                                           Mind Runtime
                                           ├─ MR Habitat
                                           └─ LCE
```

The operational line deals with shared business state, retries, offline work, migration
and transaction boundaries. The AI/agent line deals with model-serving boundaries,
routing, current state, runtime authority and longitudinal cognition.

The connection between the two lines is architectural rather than code reuse:

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
| **Mind Runtime** | Research engineering / pre-production | Deterministic certification, persistence/restart validation, test/coverage gates and Ruff/mypy no-regression baselines; live shadow validation remains incomplete |
| **LCE** | Research engineering | Public verification gate, retained negative results, controlled/synthetic research fixtures, recovery tests and replication surfaces; external empirical validation remains open |
| **MR Habitat** | Product prototype | Runnable bounded 3D prototype; current inputs are authored/mock and real MR integration remains open |

These labels describe the evidence currently present in the repositories. They are not
claims of equivalent production maturity.

## Selected work

### Long-lived AI systems

**[Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)** — A stateful runtime
for long-lived agents. It separates evidence, memory, runtime identity, canonical state,
persistence, policy, expression, telemetry, and observation so inference can continue from
authorized state rather than reconstructing authority from history.

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
| **Mind Runtime** | Repository tests/coverage, Ruff/mypy no-regression gates, deterministic certification artifacts, restart/recovery validation, and explicit incomplete live-validation status |
| **LCE** | `python scripts/verify.py`, public research experiments, retained negative results, recovery matrices, and the provider-agnostic replication surface |
| **Statebar MCP** | Regression tests around evidence priority, delayed observations, assistant-message exclusion, semantic admission, replay and sleep/wake state transitions |
| **AutoRoute Gateway** | pytest routing tests around capability filtering, credential isolation, fallback and streaming commit behavior |
| **LocalModelService** | API/session authentication regressions, owner-partitioned sessions, local-bind defaults and CI |
| **TradingWEB** | TypeScript checks, ESLint, Vitest, Playwright, i18n checks, migration replay and integration checks |
| **TradingWEB POS** | Type checking, Vitest/Jest suites, linting, native bundling and Android build helpers |
| **ShopifyDataBridge** | TypeScript validation, linting, Vitest and tests preventing direct target-database ownership |
| **MR Habitat** | Local prototype, automated tests/build, verification record and product-experiment notes |

Each repository README states its own limitations and distinguishes implemented behavior
from future work or unverified deployment claims.

## Development and architecture notes

**[Architecture Notes Across Two Project Lines](ARCHITECTURE-JOURNEY.md)** explains where
the same state/authority/recovery questions appeared in otherwise independent projects.
It is not a claim of shared code lineage.

## Engineering philosophy

> **Build for the original problem, not for the maximum available capability.**

Capability is not the same as usefulness. Architecture should preserve the target function
before expanding the feature surface. In AI-native development, generating another layer is
cheap; proving that the layer is necessary is not.

## Working stack

Python · TypeScript · FastAPI · Next.js · React · React Native / Expo · SQLite · MySQL · MCP · OpenAI-compatible APIs · Ollama · pytest · mypy · Ruff · Vitest · Playwright
