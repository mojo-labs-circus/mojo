# Private Memory Stays on Performer

* Status: accepted (provisional — subject to revision post-framework)
* Deciders: Clarke Hines
* Date: 2026-06-08

## Context and Problem Statement

A performer agent has access to personal context about its user — files, patterns,
private notes, personal history. When a performer proxies a request to the Ringmaster,
how much of that context should travel with it?

## Considered Options

* Send full context — Ringmaster gets everything it needs for best results
* Send no context — Ringmaster handles requests cold
* Send only what's needed for non-private requests — private context never leaves
  the performer

## Decision Outcome

Chosen option: **private memory stays on performer**, because sovereignty is a founding
axiom of the framework — individuals own their own data and it does not leave their
machine without explicit consent. Only non-private requests are forwarded; the
performer strips private context before proxying.

### Consequences

* Good: private data never reaches the Ringmaster or any frontier model; sovereignty
  is enforced structurally, not by convention.
* Good: even if the Ringmaster is compromised, personal context is not exposed.
* Bad: the Ringmaster produces lower-quality responses on forwarded requests because
  it lacks personal context.
* Bad: the performer must make the privacy classification decision — which requests
  are private vs. shareable. This logic needs to be correct and auditable.
