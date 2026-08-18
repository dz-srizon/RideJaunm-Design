# PR Draft — Prompt Tree Artefact

> **Status:** opened as PR #2 from `arena/01a014ec-ridejaunm-design`.
> Tree `v1.0.0`. Documentation only.

---

## Suggested title

```
feat(prompts): RideJaunm Prompt Tree artefact v1.0.0
```

**Alternative, if you prefer scope-first:**
```
feat: add dependency-ordered Prompt Tree for extending the design spec
```

## Labels

`documentation` · `design-ops` · `enhancement`

---
---

# ▼ PR BODY — copy everything below this line

## 🌳 Prompt Tree Artefact v1.0.0

Adds `prompts/` — a **dependency-ordered prompt system** for extending the merged Phase 1–8
design specification without drift.

> A prompt tree is not a list of prompts. It is a dependency graph where each node inherits
> locked decisions from its parents, is runnable standalone via an injected context payload,
> and emits a defined handoff for its children.

**9 new files · ~2,400 lines · documentation only · no application code paths touched.**

---

### The problem this solves

The naive way to use an LLM across a design programme is to paste a large prompt, get a large
answer, then paste another. On a spec this size that **reliably produces contradictions**:

- A node invents a second red because it never saw the "red = SOS only" rule
- A node re-derives a type scale that an earlier node already fixed
- A node regenerates 3,000 lines of merged colour system instead of producing the actual delta
- Each run drifts a little further from `docs/`

Four mechanisms address that:

| Mechanism | What it does |
|---|---|
| **Locked constraints** | Root-node decisions are declared non-negotiable and restated in every node. The model cannot re-litigate them. |
| **Context payload** | A compact paste-in block lets any node run in a fresh chat without the model hallucinating the project. |
| **Execution modes** | `GENERATE` / `EXTEND` / `AUDIT`. Since Phases 1–8 are merged, the default is **`EXTEND`** — produce only the delta. |
| **Output contract + self-check** | A 14-point gate and a weighted 10-point rubric the model scores itself against before returning. Catches drift a human reviewer misses on line 400. |

---

### The tree

```
[Master Prompt Configuration]
       │
       ├── 🛑 ROOT NODE — Pre-Flight Alignment & Scoping     ✅ ANSWERED & LOCKED
       │        └── emits LOCKED_CONSTRAINTS → inherited by every node below
       │
       ├── 🌿 BRANCH 1 — Brand, Aesthetics & Design System   (Phases 1–3)
       │    ├── 1A · Visual Identity & Colour Tokens          → COLOR_CONTRACT, PERSONA_SET
       │    └── 1B · Glanceable Typography & Scale            → TYPE_CONTRACT
       │
       ├── 🌿 BRANCH 2 — Figma Architecture & UI Components  (Phases 4 & 8)
       │    ├── 2A · Atomic & Molecular Design Tokens         → COMPONENT_CONTRACT
       │    └── 2B · Canvas Structure & Design Operations     → FIGMA_OPS_CONTRACT
       │
       └── 🌿 BRANCH 3 — UX Mapping, Flows & Safety Controls (Phases 5–7)
            ├── 3A · Navigation HUD & 3D Routing UI           → HUD_CONTRACT
            └── 3B · Dual-Mode SOS & Community Infrastructure → SAFETY_CONTRACT, FEED_CONTRACT
```

**Critical path:** `root → 1A → 2A → 3A → 3B`
**Parallelisable:** 1A ∥ 1B

---

### Files

| File | Purpose |
|---|---|
| `prompts/README.md` | Tree map, dependency rules, execution modes, how to run it, anti-patterns |
| `prompts/00-root-node.md` | The 4 pre-flight questions — **answered and locked** — with rationale, forced consequences, and a blast-radius table per question |
| `prompts/01-branch-brand-design-system.md` | Sub-prompts 1A + 1B |
| `prompts/02-branch-figma-architecture.md` | Sub-prompts 2A + 2B |
| `prompts/03-branch-ux-flows-safety.md` | Sub-prompts 3A + 3B |
| `prompts/_shared/context-payload.md` | Paste-in project context for models without repo access |
| `prompts/_shared/guardrails.md` | 5 inviolable rules, banned outputs, Nepal reality checks, escalation triggers |
| `prompts/_shared/output-contract.md` | Response shape, 14-point self-check gate, weighted scoring rubric |
| `prompts/ridejaunm.prompt-tree.json` | Machine-readable tree — 7 nodes, 11 edges, 3 gates |
| `README.md` | Links the tree from the root README |

---

### Four decisions worth reviewing

**1. The root node is answered, not left open.**

The original blueprint framed the four scoping questions as things to ask before running the
sub-prompts. But all four were resolved during Phases 1–8 and are already encoded throughout
`docs/`. Leaving them open invites a fresh model to answer them *differently* and contradict
merged work.

They are now locked, each with its rationale, the design consequences it forces, and the cost
of reopening it:

| Question | Locked answer | Blast radius if reopened |
|---|---|---|
| Environment | Handlebar-mounted, direct sun, gloved — tank bag is a subset | **Catastrophic** — full re-spec |
| Vibe | Himalayan-Tactical Tech hybrid, allocated by surface class | **High** — docs/01, 03, 07 + all hi-fi |
| Map | Satellite-3D riding / vector planning / vector offline & day-glare | **Medium** — map style, tile budget |
| SOS disclosure | Progressive — plain default, diagnostics one tap away | **Low–medium** — contained to SOS |

**2. Default execution mode is `EXTEND`, not `GENERATE`.**

This is the highest-impact choice in the artefact. Run these prompts naively against merged
docs and you burn a day regenerating a colour system that already exists. Every requirement
is tagged:

- 🆕 **NEW** — genuinely missing; this is where the value is
- 🔁 **EXTEND** — partially covered; needs depth or a missing case
- ✅ **VALIDATE** — already specified; verify, do not regenerate

**3. The nodes are harder than the blueprint asked for.**

Three examples where the brief was sharpened rather than transcribed:

- **3A** asked generically for a "Supercurvy Group Trip." Specified as **Kathmandu → Pokhara**
  instead of reusing the merged Kathmandu → Daman flow — because Prithvi Highway *is* the
  straight corridor, so a real Supercurvy route must divert via Nuwakot or Bandipur at roughly
  **+90 km and +2.5 h**. The node's core question becomes: *how do you sell that as a reward
  rather than an error — and where is the line between persuasive and manipulative?*
- **1A** asked for WCAG AAA on SOS Red. `#FF1F3D` on `#0B0F0E` measures **6.3:1, which is not
  AAA for normal text.** The node forces the model to state that plainly and explain how the
  large-text exemption and filled-surface variant resolve it — rather than quietly asserting
  compliance.
- **3B** makes the **responder side** the highest-value requirement in the whole tree. It is
  currently one line in `docs/05`, yet it is half of the SOS feature: alert receipt while
  moving at 60 km/h, the ACK interaction, offline navigate-to-victim, multi-responder
  coordination, the mesh-relay role, and the all-clear.

**4. Handoffs are machine-readable YAML.**

Each node emits a `HANDOFF` block that children consume **by name**. This is the mechanism that
stops Branch 3 from inventing a colour Branch 1 already fixed. If a node proposes a new token,
its handoff is marked **provisional** and a human must triage the token into
`tokens/ridejaunm.tokens.json` before children run.

---

### The delta the tree targets

The tree is only worth merging if it points at real gaps. It does:

| Node | 🆕 What's genuinely missing from `docs/` today |
|---|---|
| **1A** | **Three rider sub-culture personas** (Highway Cruisers / Dirt Racers / Mountain Tourers) — `docs/01` has brand pillars but no personas. Plus a route-colour-over-satellite validation matrix. |
| **1B** | An explicit two-pairing head-to-head, and a **letterform-confusion audit** (`1/l/I`, `0/O`, `5/S`, `6/8`) at riding speed. |
| **2A** | Figma **Auto-Layout constraint values** and the interaction state machine — `docs/04` describes components behaviourally, not their construction mechanics. |
| **2B** | The **4-sprint remap** (docs are a 3-stage / 10-week plan) and the **Component Property schema for `Control/RouteMode`**, with variant-count control. |
| **3A** | The **Kathmandu → Pokhara** flow, the **occlusion budget arithmetic**, and the speed-based chrome-disclosure ladder. |
| **3B** | The **online → offline transition choreography** as a timed sequence, and the **entire responder experience**. |

---

### Guardrails encoded

The five inviolable rules every node inherits:

1. **Red belongs to SOS.** `#FF1F3D` is emergency-only. Destructive actions use `danger-400
   #F2603C`. Any output introducing a second red is rejected.
2. **No new colours, fonts or type sizes.** Everything resolves to
   `tokens/ridejaunm.tokens.json`. Gaps must be raised as an explicit `⚠️ TOKEN GAP`, never
   invented silently.
3. **Never colour alone.** Every colour-coded state carries a second channel. Route modes are
   quad-coded (hue + pattern + icon + label).
4. **Every interactive element states its tap target.** 48 / 56 in-ride / 88 SOS / 72 PTT.
5. **Every degradable system shows honest state** — state + magnitude + age. No fake progress,
   no bare "Loading…", no success implied before it occurs.

Plus a Nepal-reality checklist (dead zones, monsoon closures, metered data, UTC+05:45, permit
zones, bandhs) and escalation triggers that require the model to **stop and flag rather than
guess** — mandatory on anything safety-critical.

---

### Verification

- ✅ `prompts/ridejaunm.prompt-tree.json` parses; **7 nodes, 11 edges, 3 gates**, no dangling
  edge references
- ✅ Critical path resolves: `root → 1A → 2A → 3A → 3B`
- ✅ All internal cross-file links resolve
- ✅ Every colour referenced in the prompts traces to `tokens/ridejaunm.tokens.json`
- ✅ Documentation only — no application code paths touched

### Suggested review order

1. `prompts/README.md` — the map, and *why* a tree rather than a prompt list
2. `prompts/00-root-node.md` — confirm the four locked answers match your intent
3. `prompts/_shared/guardrails.md` — the rules every future generation inherits
4. `prompts/03-branch-ux-flows-safety.md` — node 3B is the highest-value gap in the programme

### Follow-ups (not in this PR)

- Repo About-box description and topics — still pending from PR #1, captured in
  `.github/REPO_DESCRIPTION.md` (needs an admin-scoped `gh` login)
- First node run. Recommended: **3B**, since the responder gap is the one that would actually
  hurt in the field

# ▲ END PR BODY
