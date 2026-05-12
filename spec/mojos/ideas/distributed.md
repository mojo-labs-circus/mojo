# Idea: Distributed Stack / Resource Pooling

> Deferred — current design has one ringmaster per circus. Everything below is a future Mk.

---

## Resource Pooling

Currently the ringmaster runs everything. A future Mk could distribute the stack across circus machines by capability:

- Inference routed to the best available GPU in the circus
- Storage routed to the node with capacity
- The ringmaster's registry already tracks connected node hardware tiers — the data is there

Implementation would be a routing logic change, not an architectural redesign. `AI_SERVER` would generalise to a topology config rather than a single hostname.

---

## Multiple Ringmasters

Not currently designed. Open questions:

- Which ringmaster is source of truth for shared memory?
- How do ringmasters sync with each other?
- Consensus / conflict resolution
- Likely requires a leader election or primary/replica model

---

## Dependency

Resource pooling is achievable as a routing change once the single-ringmaster design is solid. Multiple ringmasters is a deeper problem — don't approach it until resource pooling is running.
