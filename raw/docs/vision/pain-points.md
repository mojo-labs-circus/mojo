# Pain Points — Why Companies Need Mojo

Observations from real AI transformation work. These are the problems Mojo is
positioned to solve.

---

## The Determinism Problem

Every company workflow is a mix of decisions that should be deterministic (rules,
compliance, routing, data transforms) and decisions that should be probabilistic
(summarisation, intent recognition, edge case handling). Current AI adoption treats
everything as probabilistic — you just "add AI" to a process.

The result: systems that can't be audited, can't be reasoned about, and fail in
ways that are hard to trace. Companies don't need AI everywhere. They need a way
to identify where probability earns its place and where it doesn't, and then wire
the right tool to the right step.

Mojo's framework — treating determinism as the default and probabilism as a
deliberate choice — is the missing design layer.

---

## The Migration Problem

Most companies already have systems. The question isn't "how do we build AI-native
infrastructure" — it's "how do we bring what we already have across correctly."

Mapping existing workflows onto a hybrid deterministic/probabilistic model without
breaking what works is the hard part. Consultancies sell process change. Nobody is
selling a system that introspects your current workflows and tells you where the
boundary should be.

---

## The Vendor Lock-in Problem

"Moving to the AI age" in practice means picking a vendor stack and hoping it
doesn't change. Companies end up dependent on design decisions made by someone
else, with no way to swap components or run anything privately.

Mojo's model — frontier models called as tools, local intelligence holding all
state and context — is directly applicable here. The company owns the intelligence
layer. External AI is a utility, not a dependency.

---

## The Understanding Problem

The people with budget and authority at most companies — CTOs, directors, senior
managers — don't understand this stuff and don't want to learn it. They want
outcomes, not education. The consultancies that try to upskill them are selling
to the wrong need.

Mojo can do it for them. The framework handles the design decisions; they just
describe what they want their business to do. That's the right abstraction level
for someone running a company.

---

## Notes

- These pain points are drawn from observation at internship (June 2026)
- The target is not startups building AI-native — it's established companies being
  sold "AI transformation" with no real infrastructure model underneath
