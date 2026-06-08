# Framework-First Approach

* Status: accepted
* Deciders: Clarke Hines
* Date: 2026-06-08

## Context and Problem Statement

Mojo Circus (the infrastructure layer) and the component repos could be built
directly from existing specs. But Circus must serve the Mojo framework — a general
model for collective intelligence. If Circus is built before the framework is
designed, it will encode assumptions that may need to be undone. Build now, or
design the framework first?

## Considered Options

* Build now — implement from current specs, extract framework patterns later
* Framework first — complete framework design before any implementation

## Decision Outcome

Chosen option: **framework first**, because Circus must serve the framework, not
the other way around. Building before the framework is done risks encoding the wrong
abstractions at the infrastructure level.

### Consequences

* Good: implementation is correct by construction; no retrofitting framework decisions
  onto existing code.
* Good: all component specs are written after the framework, applying established
  decisions rather than re-deriving them.
* Bad: implementation is delayed until ten framework sessions are complete.
* Bad: framework sessions must be strictly timeboxed — a session that expands without
  producing output blocks everything downstream.
