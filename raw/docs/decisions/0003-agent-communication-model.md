# Agent Communication Model

* Status: accepted
* Deciders: Clarke Hines
* Date: 2026-06-08

## Context and Problem Statement

Two distinct communication patterns exist: UI clients talking to agents, and agents
talking to each other. Should we use one protocol for both, or different ones
optimised for each?

## Considered Options

* REST for everything — uniform, simple, well-understood
* MCP for everything — agent-native contracts, but heavy for UI clients
* REST for UI-to-agent, MCP for agent-to-agent

## Decision Outcome

Chosen option: **REST for UI-to-agent, MCP for agent-to-agent**, because the two
patterns have fundamentally different requirements. UI clients need simplicity and
broad compatibility; agent-to-agent communication needs structured tool contracts
with known schemas.

### Consequences

* Good: all client-facing surfaces use REST — any client (browser, TUI, mobile)
  works without understanding MCP.
* Good: clear boundary at the Ringmaster — REST inward from clients, MCP outward
  to sub-agents and frontier models.
* Bad: two protocols to implement and maintain.
* Bad: MCP infrastructure is a non-trivial implementation dependency.
