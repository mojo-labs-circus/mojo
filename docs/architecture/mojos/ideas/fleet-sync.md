# Idea: Fleet Sync

> Deferred — performer-to-performer sync is explicitly out of scope for current Mk. All sync goes through the ringmaster.

---

## Problem

Currently local memory stays machine-scoped. Two performers in the same circus don't share local context with each other — only through the ringmaster.

A future Mk could allow performers to sync relevant local state directly (e.g. active tasks, pinned context) without a ringmaster round-trip, and to continue syncing when the ringmaster is offline.

---

## What Would Need Designing

- Multi-performer outbox protocol (currently outbox is performer → ringmaster only)
- Conflict resolution when two performers diverge while offline
- Scoping: what local state is worth syncing vs what stays machine-scoped by design
- Discovery: how performers find each other on the tailnet without ringmaster mediation
- Privacy: private memory must still never leave its device even in a performer-to-performer sync

---

## Dependency

Requires the single performer → ringmaster sync to be solid first. Don't design this until the base sync protocol is running and understood.
