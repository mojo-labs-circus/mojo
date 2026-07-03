# Tailscale for Circus Networking

* Status: accepted (provisional — subject to revision post-framework)
* Deciders: Clarke Hines
* Date: 2026-06-08

## Context and Problem Statement

Circus machines need to communicate securely across a private network — a home server,
laptops, and potentially machines in different locations. Should we build our own
networking layer or use an existing solution?

## Considered Options

* Build proprietary networking — full control, no external dependency
* WireGuard directly — open source VPN, more manual configuration
* Tailscale — managed WireGuard with automatic key exchange and device management

## Decision Outcome

Chosen option: **Tailscale**, because it solves the secure mesh networking problem
correctly and is well-maintained. No ego in the plumbing — if someone else solves a
piece of this better, use their solution.

### Consequences

* Good: zero-config secure mesh networking between all Circus machines; devices join
  and leave without manual key management.
* Good: works across NAT and different network locations — a laptop away from home
  stays in the Circus.
* Bad: external dependency on Tailscale's coordination server for device discovery
  (data stays on-device; coordination metadata goes through Tailscale).
* Bad: Tailscale's continued existence and pricing is outside our control.
