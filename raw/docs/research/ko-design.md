# Knowledge Object Design — Baseline

> Pre-session-05 design thinking. Provisional. Session 05 finalises the schema and
> resolves the open questions listed here. Do not treat anything below as decided.

---

## The Problem

Clarke needs to communicate complex ideas — projects, systems, designs — to other people
so they can contribute meaningfully. Getting good input on the Mojo framework requires
that collaborators actually understand it. Understanding requires shared mental models.

This is not a communication skills problem. It is a framework design problem. The
knowledge communication primitive needs to be designed correctly once, and every
rendering format (brief, presentation, future formats) builds on top of it.

---

## The Model

**Knowledge Objects (KOs)** are portable, semantically-tagged representations of what
an intelligence understands about something. Tagged to the producing member (human or AI).

The seam in any knowledge transfer is between **Member A's understanding and Member B's
understanding**. The KO lives at that seam.

**Current state:** Member A (Clarke) captures their mental model as a KO. Member A also
handles the adaptation — who is receiving this, what do they know, what needs emphasis.
Both run locally. The rendering is generic: "adapted for an engineer audience."

**Future state (when local AI + multi-member framework is in place):** Member A produces
a KO. Member B's personal AI — which has deep context of B specifically: their knowledge
model, how they think, what they've worked on — receives the KO and computes the diff
between A's understanding and B's. It renders a view tailored not just to B's role or
domain, but to B as a specific intelligence. The rendering is personalised in a way no
human adapter can achieve. This requires the KO to be designed for diff-readiness from
the start.

AI members receive KOs natively — no rendering needed.

This means:

- The brief and presentation are **human-readable renderers for KOs**, nothing more
- AI members consume KOs directly
- KOs are a first-class framework primitive, not a brief-specific concept
- Designing this correctly now means receiver-side adaptation can move to the receiver's
  personal AI without redesigning anything

---

## Skill Decomposition

```
capture  →  KnowledgeObject JSON

brief        →  [capture]  →  brief-adapt  →  brief-renderer  →  brief.html
presentation →  [capture]  →  presentation-adapt  →  presentation-renderer  →  deck.html
                                              →  presentation-script    →  script.md
```

### `capture`

Conversational, one question at a time. Socratic — draws out what Clarke knows but
hasn't named. Takes existing source material (docs, notes, code) and extracts from it,
only asks for what it can't get from what's there. Produces a KnowledgeObject JSON.

### `brief-adapt`

Takes a KO + runs audience intake. Produces a BriefDocument JSON. Currently runs
locally on Clarke's machine. Designed as a clean seam so it can move to receiver-side AI.

### `brief-renderer`

Pure function. BriefDocument JSON in, self-contained HTML out. Alpine.js + Mermaid.js
inlined. Dispatches on layout type.

### `brief`

Thin orchestrator. Calls capture → brief-adapt → brief-renderer.

### `presentation-adapt`, `presentation-renderer`, `presentation-script`, `presentation`

Same pattern as brief, linear slides + speaker script.

---

## Provisional KO Schema

```typescript
interface KnowledgeObject {
  id: string
  created: string
  author_id: string           // tagged to a member intelligence (human or AI)
  subject: string
  thesis: string              // the organising claim
  nodes: KnowledgeNode[]      // the mental model, structured
  source_material?: string[]  // docs/files this was derived from
}

interface KnowledgeNode {
  id: string
  content: string
  semantic: SemanticRole
  depends_on?: string[]       // ids of nodes this builds on
}

type SemanticRole =
  | "problem-statement" | "key-insight" | "mental-model"
  | "supporting-evidence" | "analogy" | "concrete-example"
  | "open-question" | "decision" | "action"
```

---

## Provisional BriefDocument Schema

```typescript
interface BriefDocument {
  title: string
  subtitle?: string
  date: string
  author: string
  status: "draft" | "review" | "approved"
  audience: string
  thesis: string
  sections: BriefSection[]
}

interface BriefSection {
  id: string
  title: string
  layout: BriefLayoutType
  surface: "dark" | "paper"
  semantic: SemanticRole
  content: object             // shape varies by layout type
  subsections?: BriefSection[]
}
```

---

## Layout Types

Shared (brief + presentation):
`prose`, `bullets`, `two-col`, `three-col`, `stat-row`, `comparison`, `steps`,
`definitions`, `callout`, `code-block`, `diagram` (Mermaid), `table`, `image`

Brief-only interactive:
`expandable`, `tabs`, `toggle-compare`, `annotated-diagram`

---

## Aesthetic Tokens

```
--aged-paper: #F2ECD8      --dark-panel: #1C1A10      --phosphor-green: #4AF626
--amber: #C47820           --text-primary: #2C2A1C    --text-secondary: #6B6545
--text-muted: #9B9275      --border: #C8C0A0
font-family: 'JetBrains Mono', monospace
```

Surface rhythm: `dark` = high rhetorical weight. Never two consecutive `dark` unless
intentional.

---

## Open Questions for Session 05

- What fields does a KO need? What is essential vs. optional?
- What semantic roles can a knowledge node play? Is the current list right?
- How do KOs reference other KOs — what does a dependency between KOs mean?
- What does diff-readiness require structurally — what fields does a receiver-side AI
  need to compute the gap between its model and a KO?
- How do KOs evolve as understanding deepens? Versioning / superseding model?
- Storage convention — where do KOs live in the filesystem, how are they named?
- When is a KO produced vs. updated vs. superseded?
- How does personal AI rendering change the schema requirements vs. generic adaptation?
