# Architecture Notes Across Two Project Lines

## What is connected, and what is not

The public repositories come from two project lines:

```text
Operational engineering                 AI / agent systems

TradingWEB                               LocalModelService / AutoRoute Gateway
├─ TradingWEB POS                        Statebar MCP (short-lived state)
└─ ShopifyDataBridge                     MR-Mem (source-backed semantic memory)
                                        └─ LCE (optional longitudinal projection)
                                        Mind Runtime (cognitive/affective harness)
                                        Agent-WorkBench (business events/tasks)
                                        MR Habitat (spatial prototype)
```

This is **not** a claim that the operational repositories evolved into the AI repositories,
that they share a package lineage, or that public commit timestamps reproduce the original
development chronology. Several older systems were cleaned and published later. This
classification also **does not** mean all AI components are presently wired together:
Agent-WorkBench and the new Host integrations are still on Draft branches.

The reason to discuss them together is narrower: independent projects kept exposing the
same class of systems question.

```text
What is temporary?
What is committed?
Who owns the authoritative state?
When is retry still semantically legal?
What may be derived without becoming source truth?
```

The sections below show where those questions appeared in each line. Similarity of design
concerns should not be read as evidence of code reuse.

## 1. Product systems: state matters before AI

The commerce projects are important because they show the engineering pattern before it became "AI architecture."

### TradingWEB

A storefront can be built as pages plus APIs. A business system cannot.

Products, variants, inventory, customers, orders, payments, refunds, permissions, audit, and fulfillment all create state transitions that several clients need to share.

The design therefore moved toward:

```text
many interfaces
    |
    v
one business authority
```

The storefront, admin system, POS, and migration path can look different while still converging on the same order and inventory semantics.

### TradingWEB POS

A physical store immediately makes reliability concrete.

A request can fail after the cashier has taken payment. The network can disappear. The client can retry. A scanner can behave differently from a camera. A printer can be Bluetooth, TCP, or a system service.

That produces several durable lessons:

- retry needs idempotency;
- offline work needs explicit pending / committed state;
- hardware should sit behind adapters;
- local state can be necessary without becoming business authority.

`client_ref` is not merely a convenient identifier. It represents a state boundary: replay the intent without duplicating the effect.

### ShopifyDataBridge

Migration exposed another version of the same distinction.

A CSV row can be syntactically parseable and still be unsafe or semantically invalid. Source data does not become target truth merely because a parser understood it.

That leads to:

```text
source
  -> parse
  -> sanitize
  -> validate
  -> check references
  -> admit
  -> target state
```

This pattern later reappears in the AI systems under different names.

## 2. LocalModelService: models became infrastructure

One of the earliest AI project lines began from a practical customer-service goal: run a useful model locally.

At first the problem looks small:

```text
application -> Ollama
```

But a real application needs streaming, text and vision models, tools, business functions, configuration, several clients, and a stable interface.

The important correction was to stop letting model details leak into the application.

```text
application
    |
    v
stable service boundary
    |
    v
replaceable inference backend
```

OpenAI-compatible endpoints became useful not because OpenAI itself was the architectural center, but because compatibility reduces coupling.

This was an early version of a principle that became much more important later:

> Replaceable capability should not own business or runtime authority.

## 3. AutoRoute Gateway: failure semantics became explicit

Once there are several model providers and credentials, "call a model" becomes a routing problem.

A naive gateway tends to collapse concepts:

```text
provider = model = credential = health
```

That is operationally false.

One credential can be rate-limited while another is healthy. One model can support vision while another cannot. One provider can fail before response commit and another can safely take over.

But streaming reveals a hard boundary.

Before the first client-visible chunk, fallback may be safe. After the client has consumed part of one generation, silently switching upstreams can create a response that never existed anywhere.

So:

```text
pre-commit failure  -> retry / fallback may continue
post-commit failure -> surface partial failure
```

The lesson is not "retry aggressively." It is:

> Recovery is only valid while the system can still preserve the semantics of one operation.

## 4. Statebar: language understanding was not state authority

Agent memory introduced a subtler version of the same problem.

Long-term memory is a poor place for temporary states such as:

- just woke up;
- feeling unwell;
- tentative afternoon plan;
- recently cancelled activity;
- unresolved short-lived concern.

Statebar began as a narrow current-state layer.

The first implementation used rule-based extraction plus optional LLM extraction. That worked until a failure exposed the abstraction leak: language *mentioning* sleep could be admitted as actual sleeping state.

Adding one more regex exception would repair the symptom. It would not repair the authority model.

The design moved toward:

```text
text / interaction
      |
      v
observation candidate
      |
      v
semantic admission
      |
      v
deterministic reconciliation
      |
      v
canonical state
```

The critical lesson was:

> Recognizing a statement is not the same as authorizing a state transition.

That idea becomes one of the central invariants in Mind Runtime.

## 5. Mind Runtime: memory became an authority problem

The original long-lived-agent question can be phrased as:

> How can an agent remember across time?

A standard answer is:

```text
history -> retrieval -> prompt -> model
```

But retrieval quality alone does not answer:

- which interpretation is current;
- which state was superseded;
- what survives restart;
- which runtime identity owns it;
- what a model is permitted to change;
- whether retrieved content is evidence or accepted state.

The stronger problem is therefore:

> How can an agent continue from authorized state rather than reconstructing authority from historical fragments every turn?

This changed the architecture.

MR separates evidence, memory, state, binding identity, admission, persistence, policy, expression, telemetry, and observation.

The recurring rules are:

```text
evidence != cognition
retrieval != authority
model output != self-authorizing state
projection != source of truth
```

### Why fail-closed behavior matters

Long-lived state compounds errors. A convenient guess that survives restart becomes more dangerous than a one-turn hallucination.

MR therefore treats `UNKNOWN`, partial state, rejected admission, and unresolved binding as legitimate outcomes.

The system is allowed not to know.

### Why observation is separate

Once state becomes durable, debugging requires causal visibility. But an inspection surface with write authority becomes another hidden mutation path.

Observation Window remains read-only by design.

## 6. LCE: longitudinal cognition had to leave MR

As MR developed, another question emerged:

> What changed across months of evidence, and what durable structure can be learned from that history?

It was tempting to implement that directly inside the runtime.

That would have been architecturally convenient and epistemically dangerous.

Experimental clustering, embeddings, semantic trajectories, and model interpretation would sit beside canonical runtime state and could gradually acquire authority simply by proximity.

The response was separation:

```text
LCE asks:
"What structure is justified across this history?"

MR asks:
"What state is authorized to participate now?"
```

LCE became an independent research-engineering system.

### The research route was shaped by failures

Several plausible ideas failed:

- raw text similarity produced giant mixed regions;
- fixed time buckets produced bad semantic boundaries;
- model-led trend discovery risked circular validation;
- exclusive clustering lost legitimate multi-membership;
- mathematically attractive structure often lacked useful semantics;
- broad green test suites still allowed provenance and recovery errors.

Each failure changed an abstraction.

The research discipline became:

```text
hypothesis
  -> bounded experiment
  -> no-future / negative control where relevant
  -> preserve the miss
  -> classify the failure
  -> change the smallest abstraction
  -> encode the learned boundary as regression
```

This is more important to the portfolio than any one clustering algorithm.

### Derived cognition must not become its own evidence

The strongest invariant is recursive:

A model interpretation, vector region, higher-order candidate, or accepted cognition revision can be useful. It cannot silently become factual evidence supporting itself later.

Otherwise the system creates epistemic compound interest on its own mistakes.

## 7. MR-Mem: extracting reusable memory from cognition and execution

A persistent agent needs to retrieve more than disconnected excerpts, but
retrieved meaning must never become a fresh source of factual authority.

[MR-Mem](https://github.com/Jasonatafricanow/MR-Mem) isolates the
semantic-memory problem from the surrounding Agent Host and from Mind Runtime's
affect, decision, and execution-adjacent concerns. The design has two modes that
must not be conflated:

- **Host-sidecar:** the native Host retains the authoritative raw conversation.
  Versioned SourceRefs let MR-Mem admit accepted semantics, invalidate stale
  support and selectively hydrate current source text.
- **Standalone benchmark adapter:** an AML-specific runtime can store raw
  messages because the benchmark host does not provide an equivalent native
  store. This is not a mandate for all production integrations to duplicate
  histories.

Canonical semantic records support bounded online Threads and optional LCE
longitudinal projections. Thread/LCE are derived structures, not parallel
authorities for original user statements. The distinction supports rebuild,
source-revision handling and precision-oriented retrieval.

### What can be verified

The [project repository](https://github.com/Jasonatafricanow/MR-Mem)
reports an AML 2026 Round 2 official-run average of **54.37** and publishes
research/verification material. The [Draft WorkBench adapter PR #22](https://github.com/Jasonatafricanow/MR-Mem/pull/22)
adds scoped source and grant checks; real Host admission and cross-Agent
integration are not yet implemented. The pre-existing Path-A offline smoke
compatibility failure remains a real gate, despite the passing quality checks.

## 8. Agent-WorkBench: business continuity is not agent cognition

The memory and runtime projects exposed a separate, everyday problem:
what happens to actual work when an Agent Session ends or a different
Agent takes over?

A task can be due, waiting on somebody, or under external commitment even
when no agent is generating tokens. Such work requires a deterministic,
shared business authority rather than another prompt, an LCE Line, or the
Host's transient Run state.

[Agent-WorkBench](https://github.com/Jasonatafricanow/Agent-WorkBench)
therefore separates:

```text
external event / business timer
             |
             v
WorkBench SQL: task / event / dependency / approval / delivery intent
             |
             v
durable wake event --> existing Agent Host: Session / Run / tools
                                       |
                       optional context from MR and MR-Mem
```

The roles are strict:

- **WorkBench:** tasks, versions, business facts, dependencies, timed follow-ups,
  authorized commitments, outbox and audit.
- **Existing Agent Host:** Session/Run lifecycle, checkpoint, deferred tool call,
  tool permission and execution. It is not a new fourth repository here.
- **Mind Runtime:** cognitive and affective state, experience-dependent
  intent tendency and WakeSignal; it cannot grant Host tool permission.
- **MR-Mem:** source-backed, authorized semantic history, Thread/LCE; retrieval
  cannot silently override current task SQL.

### Why these boundaries matter

A Host Run can finish without the task being DONE. A WorkBench task can
remain WAITING when no Host tool call is suspended. An MR WakeSignal can
exist without the Body authorizing the corresponding external action.
A sent message is an operational receipt, not evidence that the recipient
read, approved or liked it.

This project is an **in-progress engineering slice**, not a fully functioning
always-on Agent deployment. [Draft PR #1](https://github.com/Jasonatafricanow/Agent-WorkBench/pull/1)
contains an initial schema, scoped tool contracts, wake-event dedupe/lease
behavior, negative SQL isolation tests and a
[system blueprint](https://github.com/Jasonatafricanow/Agent-WorkBench/blob/feat/modular-backend-20261116/docs/PROJECT_BLUEPRINT.md).
The concrete transactional TaskService, real Host Run bridge, Telegram,
scheduler-to-event integration, dashboard and NAS deployment remain to be built.

## 9. MR Habitat: architecture should permit disposable experiments

Observation Window answered:

> Can the runtime be inspected?

Habitat asks:

> Can persistent state become perceptible as presence?

That question could expand into a virtual-world platform very quickly.

The project deliberately stops earlier:

- one environment;
- one stylized character;
- authored/mock events;
- local ambient behavior;
- no cognition ownership;
- no primary chat surface.

This is a different kind of architecture discipline.

Sometimes the right decision is not adding another authority boundary. It is refusing to turn an experiment into infrastructure before the product hypothesis survives.

Habitat is designed to be disposable without damaging MR.

## The common design pattern

The projects look different, but the same distinctions recur:

| Domain | Candidate / temporary | Authority / committed |
| --- | --- | --- |
| Migration | parsed source row | admitted target data |
| POS | queued local order | idempotently accepted server order |
| LLM routing | available candidate | committed response stream |
| Statebar | extracted observation | reconciled canonical state |
| MR | cognitive proposal / retrieval | accepted runtime cognition, not Host tool authority |
| MR-Mem | source-backed semantic candidates | canonical semantic memory; raw Host source remains authority |
| LCE | derived structure | accepted derived cognition, still not factual Memory |
| WorkBench | incoming message / task-operation proposal | SQL task version, append-only event and authorized business outcome |
| Agent Host | planned call / deferred call | Host-owned Run checkpoint and external tool receipt |
| Habitat | visual projection | no cognition authority at all |

The vocabulary changes. The engineering question does not:

> What is allowed to become true for the next layer?

## Design philosophy

### Build for the original problem

More capability creates more ways to lose the boundary.

A local service does not need to become a distributed scheduler. A state layer does not need to become an agent runtime. A runtime does not need to absorb research. A visualization does not need to become a world platform.

### Keep uncertain things uncertain

A system that says "unknown" at the correct boundary is often safer and more useful than one that produces a confident but recursive explanation.

### Recovery semantics define architecture

Restart, retry, offline operation, delayed events, partial streams, invalidation, and replay are not implementation details. They reveal what the state actually means.

### Revalidate abstractions

For model-centric systems, one question matters repeatedly:

> If the model became dramatically more capable tomorrow, which parts of this component would still be required for correctness, recovery, authority, or auditability?

Some mechanisms should disappear as capability improves.

The boundaries that protect state and meaning usually should not.

## What the portfolio is intended to demonstrate

Not that every project is production-complete, and not that persistent cognition has been solved.

The intended signal is narrower:

- translating messy product failures into explicit system contracts;
- separating probabilistic capability from deterministic authority;
- designing for restart, retry, replay, and partial failure;
- preserving negative research results;
- changing abstractions when evidence contradicts them;
- keeping scope small enough that a project can still say what it does **not** own.

## Current integration evidence boundary (2026-11)

The cross-project architecture is documented, but the implementation is intentionally
not described as production-integrated. The three review points are:
[Agent-WorkBench #1](https://github.com/Jasonatafricanow/Agent-WorkBench/pull/1)
for business state and event contracts,
[MR-Mem #22](https://github.com/Jasonatafricanow/MR-Mem/pull/22)
for source/authorization bindings, and
[Mind Runtime #67](https://github.com/Jasonatafricanow/Mind-Runtime/pull/67)
for the frozen Soul/Body authority clarification. The existing Host is
responsible for Session/Run and tool execution; it has not been claimed as a
working production connection to all three.
