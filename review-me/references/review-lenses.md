# Review Lenses

Use this checklist to construct and audit the decision tree. A lens is not a
scripted question: resolve it from evidence when possible, mark it not applicable
with a reason when it cannot affect the target, and interview only where a real
decision remains.

## Core lenses

These apply to every review target.

### Intent and success

- Desired outcome and the problem it solves
- Observable success and failure signals
- Priority when goals conflict
- Constraints that make an otherwise good answer invalid

### People and authority

- Actors, beneficiaries, operators, and affected non-users
- Who may decide, initiate, observe, change, cancel, and recover
- Ownership at handoff, failure, and deliberate deferral

### Scope and boundaries

- Entry trigger and preconditions
- Included behavior, exclusions, and non-goals
- Start, end, cancellation, and handoff boundaries
- Behavior before, during, and after the reviewed change

### Behavior and states

- Happy path and meaningful alternate paths
- Empty, minimum, maximum, duplicate, stale, and invalid cases
- States, transitions, invariants, and terminal states
- Partial progress, interruption, retry, rollback, and recovery

### Proof

- Concrete acceptance examples with inputs and observable outcomes
- A counterexample that must fail or be rejected
- Evidence source, test level, runtime check, and acceptance owner
- The distinction between verified fact, assumption, and chosen policy

## Contextual lenses

Instantiate each lens whose answers could change the decision or its acceptance
proof.

### Data and identity

- Source of truth, identifiers, validation, normalization, and defaults
- Storage, retention, deletion, migration, backfill, and compatibility
- Missing, duplicated, conflicting, late, or out-of-order data

### Dependencies and contracts

- Upstream and downstream systems, APIs, events, schemas, and versions
- Availability, timeout, degradation, rate limits, and fallback behavior
- Contract ownership and compatibility during mixed-version rollout

### Concurrency and repetition

- Ordering, races, simultaneous actors, and conflicting updates
- Idempotency, deduplication, replay, retries, and cancellation
- Atomicity and visible behavior after partial failure

### Security, privacy, and abuse

- Authentication, authorization, privilege boundaries, and auditability
- Sensitive inputs and outputs, minimization, retention, and disclosure
- Abuse cases, resource exhaustion, unsafe content, and trust boundaries

### Human interface

- Discoverability, feedback, confirmation, undo, and error recovery
- Keyboard and assistive-technology behavior, contrast, motion, and focus
- Responsive layouts, localization, time zones, units, and formatting
- Empty, loading, offline, slow, denied, and degraded states

### Capacity and operations

- Latency, throughput, volume, resource, and cost ceilings
- Observability, alerts, logs, metrics, tracing, support, and diagnostics
- Deployment, feature flags, staged rollout, rollback, and incident response

### Evolution

- Backward and forward compatibility
- Extension points and decisions intentionally kept reversible
- Deprecation, migration completion, cleanup, and long-term ownership

## Leaf-splitting signals

Split another child node whenever an answer introduces:

- an undefined adjective or quantifier;
- a new actor, permission, state, event, dependency, or source of truth;
- “usually”, “except”, “unless”, “later”, “automatic”, or a fallback;
- different behavior by platform, role, locale, connectivity, or time;
- a failure without a recovery owner;
- a success claim without an observable acceptance example; or
- a tradeoff whose priority has not been chosen.
