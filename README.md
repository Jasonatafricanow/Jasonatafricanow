# Jiasen Wang

I build **AI-native systems and operational software** by turning ambiguous product failures into explicit state, authority, recovery, and ownership boundaries.

The public repositories span full-stack commerce, offline mobile systems, local / multi-provider LLM infrastructure, agent state, runtime architecture, longitudinal cognition research, and spatial presence experiments.

The useful way to read the portfolio is not as a list of frameworks. It is as a sequence of increasingly abstract system questions.

## Portfolio map

```text
Operational systems
  TradingWEB
  ├─ TradingWEB POS
  └─ ShopifyDataBridge
          |
          | business systems exposed reliability,
          | state and integration boundaries
          v
LLM infrastructure
  LocalModelService
  └─ AutoRoute Gateway
          |
          | model access became an infrastructure problem
          v
Agent state
  Statebar MCP
          |
          | "current state" exposed an authority problem
          v
Long-lived agent runtime
  Mind Runtime
  ├─ Observation Window
  └─ MR Habitat
          |
          | longitudinal interpretation outgrew runtime authority
          v
Longitudinal cognition research
  LCE
```

This is a **conceptual development path**, not a literal ordering of public commit timestamps. Several older projects were published later as cleaned baselines.

## Selected systems

### Long-lived AI / cognition

**[Mind Runtime](https://github.com/Jasonatafricanow/Mind-Runtime)**  
A stateful runtime for long-lived agents. It separates evidence, memory, runtime identity, canonical state, persistence, policy, expression, telemetry, and observation so model inference can continue from authorized state rather than reconstructing everything from history.

**[LCE — Longitudinal Cognition Engine](https://github.com/Jasonatafricanow/LCE-Longitudinal-Cognition-Engine)**  
A research-engineering system for longitudinal understanding. Its current architecture was shaped through failed hypotheses, no-future evaluation, negative controls, adversarial regressions, immutable revisions, and an explicit rule that derived cognition cannot become factual authority by itself.

**[Statebar MCP](https://github.com/Jasonatafricanow/Statebar-mcp)**  
A lightweight current-state layer for agents. It maintains short-lived user state through evidence, semantic time, deterministic reconciliation, lifecycle rules, MCP / REST transports, and anti-self-pollution boundaries.

**[MR Habitat](https://github.com/Jasonatafricanow/MR-Habitat)**  
A bounded spatial-presence experiment. It asks whether persistent agent state is more legible when projected into an environment, while keeping cognition ownership outside the visualization.

### LLM infrastructure

**[AutoRoute Gateway](https://github.com/Jasonatafricanow/AutoRoute-Gateway)**  
A capability-aware multi-provider / multi-credential LLM gateway. Key concerns are provider-vs-credential identity, request capability gating, error-aware fallback, and a streaming commit boundary that prevents unsafe provider replay after output reaches the client.

**[LocalModelService / OpenClaw-CS](https://github.com/Jasonatafricanow/LocalModelService)**  
A local-first AI service built around Ollama and FastAPI. It exposes OpenAI-compatible endpoints, streaming, vision-capable models, tool modules, and business adapters behind a replaceable service boundary.

### Operational product systems

**[TradingWEB](https://github.com/Jasonatafricanow/TradingWEB)**  
A full-stack commerce and operations system that centralizes product, customer, order, payment, admin, POS, and migration domain logic.

**[TradingWEB POS](https://github.com/Jasonatafricanow/TradingWEB-POS)**  
An offline-first React Native / Expo POS client with idempotent resynchronization, hardware abstraction, shift / staff flows, local security boundaries, and explicit separation between pending local work and server-authoritative business state.

**[ShopifyDataBridge](https://github.com/Jasonatafricanow/ShopifyDataBridge)**  
A Shopify-to-TradingWEB migration boundary that parses, sanitizes, validates, maps, and audits imported data before it is admitted into the target business system.

## How I develop systems

### 1. Start from the failure mode

I prefer to begin with a concrete failure:

- a retry can duplicate an order;
- a model can reinforce its own inferred state;
- a credential failure can poison an entire provider;
- a migration can parse correctly and still corrupt references;
- a longitudinal pattern can look meaningful only because later evidence leaked into the evaluation.

The architecture is then organized around preventing or exposing that failure.

### 2. Separate discovery from authority

A recurring distinction across the portfolio is:

```text
candidate != accepted state
retrieval != authority
parsed input != admitted data
local pending work != committed remote effect
projection != cognition
```

Probabilistic or derived systems may propose. A narrower boundary decides what becomes authoritative.

### 3. Treat failure and recovery as part of the design

Retries, restart recovery, delayed evidence, partial streams, offline queues, idempotency, migration batches, and invalidated cognition are not edge cases added after the happy path. They determine the state model.

### 4. Preserve negative results

In research-heavy work, a failed hypothesis is useful if it changes the abstraction. LCE in particular preserves giant-component failures, semantic-boundary failures, weak structural signals, recovery faults, and adversarial regressions as part of the reasoning record.

### 5. Revalidate architecture when capability changes

Some code exists only because current models are weak at a task. Other boundaries remain necessary even if models become dramatically stronger.

I try to distinguish the two.

A component can therefore be kept, demoted, replaced, or deleted when evidence shows the abstraction no longer matches the problem.

## Engineering philosophy

> Build for the original problem, not for the maximum available capability.

Capability is not the same as usefulness. I prefer narrow contracts, explicit ownership, inspectable state, reproducible failure cases, and systems that can survive replacement of the model, provider, UI, or framework around them.

## Reading guide

- **[From Operational Software to Long-Lived AI Systems](ARCHITECTURE-JOURNEY.md)** — the development and abstraction path across the portfolio.
- **[Independent portfolio review](PORTFOLIO-INDEPENDENT-REVIEW.md)** — an earlier independent review of the MR / LCE / Habitat subset; useful as a conservative baseline, but narrower than the current public portfolio.
- Individual repository READMEs now use the same case-study structure: problem → development path → design decisions → architecture → verification → boundaries.

## Working stack

Python · TypeScript · FastAPI · Next.js · React · React Native / Expo · SQLite · MySQL · MCP · OpenAI-compatible APIs · Ollama · pytest · mypy · Ruff · Vitest · Playwright

## Publication note

Several projects predate their current public Git repositories. Where relevant, repository READMEs explicitly mark cleaned publication baselines so public commit dates are not mistaken for the complete development history.
