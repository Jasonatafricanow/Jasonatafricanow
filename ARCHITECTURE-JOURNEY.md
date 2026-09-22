# Development Notes

This file records how the public projects changed scope over time. It is not intended as one unified theory of the portfolio.

## TradingWEB

The project started from storefront work and grew into a shared backend for products, inventory, customers, orders, payments, administration, POS, and migration.

The main technical consequences were:

- order and inventory rules moved behind shared server APIs;
- POS retries required stable client references and idempotent writes;
- Stripe/PayPal callbacks are handled independently from browser redirects;
- migration traffic uses a separate validation/import path;
- RBAC and audit logging are part of normal operations rather than admin-side exceptions.

## TradingWEB POS

The mobile client made intermittent connectivity and hardware differences unavoidable.

The implementation therefore separates:

```text
local pending order
        |
        v
sync / retry with client_ref
        |
        v
server-accepted order
```

Scanner and printer integrations sit behind adapters so checkout logic does not depend on one transport or device model.

## ShopifyDataBridge

Historical Shopify imports are handled outside the normal storefront request path.

```text
CSV
 -> parse
 -> sanitize
 -> validate fields and references
 -> map
 -> batch import
 -> TradingWEB
```

The bridge also checks formula injection, unsafe HTML, missing references, and inconsistent inventory before upload.

## LocalModelService

This began as a local AI customer-service service around Ollama.

It later exposed a stable HTTP/OpenAI-compatible surface so applications did not need Ollama-specific integration. Streaming, text/vision model selection, tools, and business adapters became separate modules rather than one chat application.

## AutoRoute Gateway

Routing across multiple upstreams required separating provider, credential, model capability, health, and quota.

The important streaming case is simple:

```text
failure before client-visible output -> another candidate may be tried
failure after output is committed     -> surface the partial failure
```

The gateway does not splice responses from unrelated generations after a stream has started.

## Statebar MCP

Statebar started as a small current-state service with rule-based and optional model extraction.

A concrete bug changed the design: the extractor matched sleep-related text inside a discussion about sleep and created a false sleeping state.

The fix was to separate extraction from the state update:

```text
user message
 -> extracted observation
 -> evidence/time checks
 -> proposed state change
 -> deterministic reconciliation
 -> SQLite state
```

The original user text remains the source for sleep/wake admission checks, and assistant output is not treated as user-state evidence.

## Mind Runtime

MR grew from the broader question of keeping agent state across turns and restarts.

Current implementation areas include:

- SQLite-backed facts, runtime state, intents, delivery records, and checkpoints;
- runtime bindings and per-binding storage isolation;
- bounded memory retrieval with optional Qdrant/FastEmbed adapters;
- process-local turn admission and commit/abort handling;
- restart validation;
- telemetry and causal tracing;
- a read-only Observation Window;
- an optional one-way adapter to LCE.

Several research ideas that were explored inside MR were later moved out when they did not belong in the runtime.

## LCE

LCE became a separate repository for experiments over longitudinal evidence.

The research route changed several times:

- raw text similarity produced large mixed regions;
- fixed time buckets split/merged semantic material poorly;
- model-led trend discovery made evaluation circular;
- exclusive clustering lost legitimate multi-membership;
- some structural signals were stable mathematically but weak semantically;
- green tests initially missed provenance/recovery failures.

The current implementation uses semantic blocks, rebuildable vectors, local overlapping structures, candidate interpretations, immutable accepted revisions, temporal cutoffs, invalidation, and deterministic reads.

## MR Habitat

Habitat is a deliberately small UI experiment: one environment, one stylized character, mock/authored events, and read-oriented interactions.

The current question is simply whether persistent agent activity is useful when rendered spatially. It is not a second runtime and does not own agent state.
