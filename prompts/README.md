<div align="center">

# 🌳 RideJaunm — Prompt Tree Artefact

**A dependency-ordered, context-injected prompt system for executing and extending
the RideJaunm design programme.**

`v1.0.0` · `Direction: Himalayan-Tactical Tech` · `Root node: ANSWERED & LOCKED`

</div>

---

## What this is

A **prompt tree** is not a list of prompts. It is a dependency graph where each node
(a) inherits locked decisions from its parents, (b) is runnable standalone via an injected
context payload, and (c) emits a defined handoff for its children.

This artefact exists because the naive way to use an LLM on a design programme — paste a
giant prompt, get a giant answer, paste another giant prompt — **produces contradictions**.
Node 3B invents a new red. Node 2A re-derives a type scale that node 1B already fixed. Every
run drifts a little further from the merged spec.

The tree solves that with four mechanisms:

| Mechanism | What it does |
|---|---|
| **Locked constraints** | Decisions already made are declared non-negotiable and restated in every node. The model cannot re-litigate them. |
| **Context payload** | A compact paste-in block ([`_shared/context-payload.md`](_shared/context-payload.md)) lets any node run in a fresh chat without the model hallucinating the project. |
| **Execution modes** | Every node runs as `GENERATE`, `EXTEND` or `AUDIT`. Since Phases 1–8 are **already merged**, the default is `EXTEND` — produce only the delta, never regenerate. |
| **Output contract + self-check** | Each node must verify its own output against a rubric before returning. Catches the drift that a human reviewer misses on line 400. |

---

## 🌳 The tree

```
[Master Prompt Configuration]
       │
       ├── 🛑 ROOT NODE — Pre-Flight Alignment & Scoping        ✅ ANSWERED & LOCKED
       │        └── emits: LOCKED_CONSTRAINTS → inherited by every node below
       │
       ├── 🌿 BRANCH 1 — Brand, Aesthetics & Design System      (Phases 1–3)
       │    ├── 1A · Visual Identity & Colour Tokens            → emits COLOR_CONTRACT
       │    └── 1B · Glanceable Typography & Scale              → emits TYPE_CONTRACT
       │
       ├── 🌿 BRANCH 2 — Figma Architecture & UI Components     (Phases 4 & 8)
       │    ├── 2A · Atomic & Molecular Design Tokens           ← needs 1A + 1B
       │    └── 2B · Canvas Structure & Design Operations       ← needs 2A
       │
       └── 🌿 BRANCH 3 — UX Mapping, Flows & Safety Controls    (Phases 5–7)
            ├── 3A · Navigation HUD & 3D Routing UI             ← needs 1A + 2A
            └── 3B · Dual-Mode SOS & Community Infrastructure   ← needs 1A + 2A + 3A
```

### Dependency rules

- **Root node blocks everything.** Its four answers are already locked in
  [`00-root-node.md`](00-root-node.md) — read them, don't re-ask them.
- **Branch 1 blocks Branches 2 and 3.** Colour and type contracts are the substrate.
  1A and 1B are independent of each other and **can run in parallel**.
- **2A blocks 2B and both of Branch 3.** You cannot lay out a screen from components
  that don't exist.
- **3A blocks 3B.** The SOS console inherits the HUD's map-chrome grammar.

### Critical path

```
ROOT → 1A ─┬→ 2A → 2B
           │      └→ 3A → 3B
       1B ─┘
```

---

## 📁 Files

| File | Purpose |
|---|---|
| [`00-root-node.md`](00-root-node.md) | The 4 pre-flight questions, **answered and locked**, with rationale and the design consequences each answer forces |
| [`01-branch-brand-design-system.md`](01-branch-brand-design-system.md) | Sub-prompts 1A + 1B |
| [`02-branch-figma-architecture.md`](02-branch-figma-architecture.md) | Sub-prompts 2A + 2B |
| [`03-branch-ux-flows-safety.md`](03-branch-ux-flows-safety.md) | Sub-prompts 3A + 3B |
| [`_shared/context-payload.md`](_shared/context-payload.md) | Paste-in project context — makes any node standalone-runnable |
| [`_shared/guardrails.md`](_shared/guardrails.md) | Non-negotiables, banned outputs, escalation triggers |
| [`_shared/output-contract.md`](_shared/output-contract.md) | Format spec, self-check gate, 10-point scoring rubric |
| [`ridejaunm.prompt-tree.json`](ridejaunm.prompt-tree.json) | Machine-readable tree — for automation, agents, or CI |

---

## ▶️ How to run it

### Step 0 — Read the root node (2 minutes, mandatory)

[`00-root-node.md`](00-root-node.md) contains all four answers. **Do not re-ask them.**
They were resolved during the Phase 1–8 programme and are encoded throughout the merged docs.
If a stakeholder wants to change one, that is a **root-node amendment** — it invalidates
downstream work and must be versioned (see *Amendments* below).

### Step 1 — Choose an execution mode

| Mode | When | Behaviour |
|---|---|---|
| `GENERATE` | Greenfield / new feature area with no existing spec | Full output from scratch |
| **`EXTEND`** ← default | The spec exists in `docs/` and you want the missing delta | Output **only** what's new; cite what already covers the rest |
| `AUDIT` | You suspect drift, or a stakeholder challenged a decision | No new design — return a findings table with severity and file:line references |

> **Because Phases 1–8 are merged, almost every run should be `EXTEND` or `AUDIT`.**
> Running `GENERATE` on a node that already has merged output is the single most common
> way to waste a day and introduce contradictions.

### Step 2 — Assemble the prompt

```
┌─────────────────────────────────────┐
│ 1. _shared/context-payload.md       │  (only if the model can't read the repo)
│ 2. _shared/guardrails.md            │  (always)
│ 3. The sub-prompt, e.g. 1A          │  (always)
│ 4. _shared/output-contract.md       │  (always)
│ 5. Parent handoffs, if any          │  (see each node's INHERITS block)
└─────────────────────────────────────┘
```

If your model **can** read the repo (Claude Code, Cursor, an agent with file access),
skip the context payload and let the `@`-references in each node resolve naturally.
That is strictly better — the payload is a lossy summary.

### Step 3 — Run, verify, capture the handoff

Every node ends with a `HANDOFF` block. **Copy it into a scratch file.** Child nodes
declare exactly which handoffs they inherit. Skipping this is why trees drift.

### Step 4 — Score it

Every output gets scored against the 10-point rubric in
[`_shared/output-contract.md`](_shared/output-contract.md). **Below 8/10 → re-run with the
failures quoted back at the model.** Do not hand-fix a bad generation; fix the prompt.

---

## 🎯 What each node should actually produce right now

Because Phases 1–8 are merged, most requirements in the original blueprint are already
satisfied. Each node tags its requirements so you don't burn tokens:

| Tag | Meaning |
|---|---|
| 🆕 **NEW** | Genuinely missing. This is where the value is. |
| 🔁 **EXTEND** | Partially covered; needs depth, a variant, or a missing case. |
| ✅ **VALIDATE** | Already specified. Verify against `docs/`, do not regenerate. |

### The real delta across the whole tree

| Node | 🆕 What's actually missing today |
|---|---|
| **1A** | **3 rider sub-culture personas** (Highway Cruisers / Dirt Racers / Mountain Tourers) — the docs have brand pillars but no personas. Plus a route-colour-over-satellite validation matrix. |
| **1B** | An explicit **two-pairing comparison with a recommendation**, and a letterform-confusion analysis (`1/l/I`, `0/O`, `5/S`, `6/8`) at speed. |
| **2A** | Figma **Auto-Layout constraint values** and the interaction state machine as a table — the docs describe components, not their layout mechanics. |
| **2B** | **4-sprint remap** (docs are structured as a 3-stage / 10-week plan) + the exact **Component Property schema for the 3-Way Route Switcher**. |
| **3A** | The **Kathmandu → Pokhara** flow. Docs cover Kathmandu → Daman (Rajpath). Pokhara is a different problem: Prithvi Highway is a *straight* corridor, so Supercurvy must divert via Bandipur/Nuwakot — the flow must handle "the curvy route is +2h". |
| **3B** | The **online → offline transition choreography** as a timed sequence, and the **responder-side** screens (half the SOS feature is currently unspecified). |

---

## 🔒 Amendments to locked decisions

If a root-node answer or a locked constraint must change:

1. Open an issue titled `root-amendment: <what>`.
2. State the **blast radius** — which nodes and which merged docs are invalidated.
3. Bump the tree version (`v1.0.0` → `v2.0.0`; root changes are always major).
4. Re-run every affected node in `AUDIT` mode **before** re-running in `EXTEND`.
5. Update [`00-root-node.md`](00-root-node.md) with the new answer, the date, and the reason.

**Never silently edit a locked constraint.** The whole point of the tree is that downstream
nodes can trust their inheritance.

---

## 🧪 Anti-patterns

| ❌ Don't | ✅ Do |
|---|---|
| Run a node without its parent handoffs | Copy the `HANDOFF` block forward every time |
| Run `GENERATE` on merged phases | Run `EXTEND` and demand the delta only |
| Ask the model to "be creative" about colour or type | Those are locked; creativity belongs in personas, flows and micro-interactions |
| Accept output that introduces a new hex value | Every colour must resolve to `tokens/ridejaunm.tokens.json` |
| Skip the self-check because the output "looks good" | Long outputs hide contradictions on line 400 |
| Run all six nodes in one mega-prompt | You get shallow output on all six instead of deep output on one |
| Let a node invent a second red | 🚨 See [`_shared/guardrails.md`](_shared/guardrails.md) |

---

## 📚 Source of truth

This tree **serves** the merged documentation; it does not supersede it.

| Layer | Authority |
|---|---|
| `tokens/ridejaunm.tokens.json` | **Highest** — machine-readable, wins every conflict |
| `docs/01–09` | Merged specification |
| `prompts/` (this tree) | Execution layer — how to produce more spec consistently |
| Any LLM output | **Lowest** — a draft until reviewed and merged |
