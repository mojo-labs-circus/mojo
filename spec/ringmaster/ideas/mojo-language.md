# Mojo Language

A scripting language for running your AI as a company. You write a `.mojo` file — a machine-readable, executable spec — and when you run it, an entire AI engineering organisation spins up to build your project under your direction.

This is not vibe-coding. That is the overarching principle. The mojo language exists to give skilled developers more power, not to replace them.

## What a .mojo File Is

A `.mojo` file is a spec, not a program. You are not writing instructions for a computer — you are writing a brief for a company. The constructs are things a technical lead actually thinks about:

- **Goals** — what the product needs to do
- **Components** — how the system breaks down, what each part is responsible for, how they relate
- **Guardrails** — what the system must never do
- **Success criteria** — how you know when a component or the whole product is done

The syntax is block-based and declarative — closer to Terraform HCL than Python or YAML. You describe structure, not procedures.

```mojo
product "task-manager-api" {
  goal: "REST API with full CRUD for tasks"

  component "database" {
    type: postgres
    schema {
      tasks: { id: uuid, title: string, done: bool, created_at: timestamp }
    }
  }

  component "api" {
    depends_on: database
    interface {
      GET    /tasks      -> "list all tasks"
      POST   /tasks      -> "create a task"
      PUT    /tasks/:id  -> "update a task"
      DELETE /tasks/:id  -> "delete a task"
    }
    must_not {
      - "return raw SQL errors to the client"
    }
    done_when {
      - "all endpoints respond correctly to happy-path requests"
      - "test suite passes"
    }
  }

  agent main {
    model: auto
    builds: [database, api]
  }
}
```

`model: auto` resolves at runtime from `.mojo_config` — the same file runs on any machine in the circus, scaling to available hardware.

## The Company

Running a `.mojo` file spins up a standard AI engineering company. The spec is the brief; the runtime provides the org. You do not configure the company — it assembles itself from the spec.

Standard roles that spin up for every run:

| Role | Responsibility |
|---|---|
| **Architect** | Reads the full spec, breaks it into tasks, assigns work |
| **Coders** | One per component — write the implementation |
| **Reviewer** | Pushes back on quality, requests changes |
| **QA** | Owns `done_when` criteria, signs off on completion |
| **Ethics/Guardrails** | Monitors all output against `must_not`, can veto |
| **Watcher** | Observes the whole process, flags drift from the goal |

The company runs internal debate. The coder proposes, the reviewer pushes back, the ethics team flags issues. Structured disagreement is what produces quality — not parallel execution alone.

## The Hierarchy

The Architect's job is not just to assign tasks — it is to decide what is complex enough to warrant its own company. Simple component: assign directly to a coder. Complex component: write a `.mojo` file and spin up a subsidiary. This is the same decision a CTO makes when choosing what to keep in-house vs. contract out.

The result is a tree. One `project.mojo` at the root. A set of generated component `.mojo` files underneath it, each running their own full company — Architect, Coders, Reviewer, QA, Ethics, Watcher — independently. Those companies can themselves generate further subsidiaries if a component is complex enough to warrant it. The tree is as deep as the problem requires.

This unlocks four things:

**Fractal visibility.** Every `.mojo` file is independently watchable using the same interface. As CTO you can sit at the top level and track high-level progress, or zoom into any component's company and see exactly what that team is doing, or zoom further into individual agent decisions within that team. Same dashboard at every layer of abstraction.

**Outsourcing but transparent.** In the real world, outsourcing costs you visibility — you get deliverables, not insight into how they were produced. Here the subsidiary `.mojo` is fully open. You can walk in at any time, at any level, and see the work in progress.

**Sub-.mojo files as versioned artifacts.** The system does not just produce code — it produces a complete auditable tree of specs that produced the code. Every generated `.mojo` lives in the repo, inspectable and re-runnable. If something went wrong in a component, you go look at that component's `.mojo` and its run history.

**Context isolation for free.** Each subsidiary company only knows about its own `.mojo` — not the whole system. The context window problem is solved architecturally: no component team is carrying context it does not need.

## You Are the CTO

You are not a client who submits a spec and waits for a product. You are a live participant — the technical lead the whole company is working under.

What that means in practice:
- Agents surface decisions to you rather than making them. You get options and tradeoffs; you make the call.
- You can check in at any time, at any depth. See what every team is working on right now. Pull someone into a review. Redirect a component mid-build.
- Nothing ships, merges, or gets marked done without your explicit sign-off.
- Multiple projects can run simultaneously. With enough compute on the Ringmaster, multiple companies are active at once — different projects, different teams, all under your direction. Check in from your phone, your browser, your terminal.

## Anti-Vibe-Coding

Vibe-coding is: describe something loosely, accept whatever comes out, ship it without understanding what you got.

This is the opposite of that. The mojo language exists to give technical people more power — not to replace their judgment with the AI's.

The target operating level is **Boris Cherny** with a full engineering org: someone who could walk into any part of the system, understand what's happening and why, and have a strong opinion about whether it's right. You cannot necessarily recite every line the AI team wrote — no technical lead on a large codebase can do that, and that is not the bar. The bar is conceptual ownership. You can explain the architecture, the tradeoffs, the approach. You made every major decision. You can defend every concept.

**Tony Stark** probably could not tell you the exact resistance value of every circuit in the Iron Man suit. But he could tell you why every subsystem exists, whether it was working correctly, and exactly what to change if it was not. That is the operating level this targets.

The AI handles execution detail. You handle direction, judgment, and conceptual ownership. The product is still entirely yours.

## Write Once, Run Anywhere

The same `.mojo` file runs on any machine in the circus. The runtime resolves `model: auto` from the machine's `.mojo_config`. ringbaker uses larger models and produces higher quality output faster. pearlybaker runs smaller local models and is slower. The spec does not change — the execution context does.

This is not a "works on my machine" tool. A spec written for the Ringmaster runs on a performer machine at reduced quality but identical structure.

## Open Questions

- Can the company structure be customised per-project (e.g. skip reviewer for a fast prototype run)?
- What does the syntax look like for inter-component communication or shared state?
- How does a `.mojo` run map onto LangGraph nodes at the Ringmaster level — does the Architect generate the graph dynamically?
- How does the live check-in interface surface per-agent state to the developer?
