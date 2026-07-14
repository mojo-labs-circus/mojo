# Seams

The output of walking [seam-method.md](seam-method.md) against the piece
list. Replaces the eighteen-letter seam list that used to live in
research-plan.md, which assumed every seam was the same shape (a flat,
POSIX-style table) before the shape work showed that was wrong. Existing
research in research-plan.md isn't discarded, it's cited from here where a
seam entry actually draws on it, rather than duplicated.

One entry per seam. Not a table, because the entries aren't uniform, that's
the whole point.

Status values, matching the procedure's steps: `existence` (found in the
matrix, not yet classified), `classified` (step 3 done), `shaped` (step 5
done, a real precedent matched), `stress-tested` (step 6 run, held or sent
back), `settled` (recorded here for real, still revisable if a later seam's
walk exposes a hole in it, the same way the piece list stays revisable).

Nothing below is filled in yet. Next step: run the existence pass (step 1)
across [piece-matrix.md](piece-matrix.md), from scratch, not checked against the old
eighteen-letter list while working. That list (with anatomy.md's old lettered
map) is preserved as of commit `d35483b` (`git show d35483b:anatomy.md`) —
worth diffing against once the pass is done, to see what reappears on its own
and what doesn't.

---

## Cross-cutting objects

piece-matrix.md is a pairwise pass, one entry per piece pair. That shape
can't represent an object that's really one thing with several bilateral
faces, a Grant issued by one piece, checked by a second, revoked by a third,
would just show up as three separate cells, each able to pass its own
substitutability test (step 6) in isolation while the underlying schema
quietly drifts three different ways. This registry is the hand-kept check
against that failure mode: every seam entry's "What crosses" field gets
checked against it, not just against m's data model.

Empty on purpose, same discipline piece-matrix.md holds: nothing gets a
row here from a guess at what the objects probably are, only from an actual
seam's "What crosses" field naming one for real. A row starts as a name
only, no schema, added the first time some seam's walk needs one. The
second seam that touches the same object checks against what's already here
instead of inventing its own.

| Object | Status | Seams that touch it | Notes |
|---|---|---|---|

Status values: `candidate` (named once, not yet seen on a second crossing),
`confirmed` (at least two seam entries' "What crosses" matched it),
`schema-drift` (two seams touch it but describe it differently, needs
reconciling before either can go `settled`).

---

## Shape and object families

Two rollups worth building once enough entries exist, neither exists yet
since no seam has reached `shaped` status:

- **By matched shape** (step 5's six shapes): which seams turned out to be
  the same precedent wearing different piece names.
- **By shared object** (the registry above): which seams turned out to be
  different faces of the same object rather than genuinely separate
  relationships.

A seam can belong to both groupings at once, and they're not the same
question, two seams can share an object while matching different shapes
(one side's write versus another side's read of the same Grant), or match
the same shape while sharing no object at all.

---

## Entry template

```
### <name> — <piece> ↔ <piece>

**Status:** existence | classified | shaped | stress-tested | settled

**Crossing:** one-way (A → B) / two-way / peer

**Structural classification:**
- Locality: same machine/process | across machines
- Cardinality: <side A> is singular/plural ↔ <side B> is singular/plural
- Authority: <side> owns the fact | peers
- Persistence: a fact survives past this crossing | nothing remains once it
  returns

**Load-bearing problems:** (subset of shape, vocabulary, discovery,
versioning, trust, liveness — say which, and why the others don't bite)

**What crosses:** plain description of the actual fact/object, checked
against whether it's a new shape, an existing one from m's data model, or an
entry in the cross-cutting object registry above

**Shared object:** name from the registry above, or "none, owned solely by
this seam"

**Matched shape:** one of the six from seam-method.md, with the real
precedent it's drawn from

**Open questions:**

**Notes / prior art carried from research-plan.md:**
```
