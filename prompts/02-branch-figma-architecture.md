# 🌿 BRANCH 2 — Figma Architecture & UI Components

**Covers:** Phases 4 & 8 · **Inherits:** `LOCKED_CONSTRAINTS`, `COLOR_CONTRACT`, `TYPE_CONTRACT`
**Blocks:** Branch 3

2A must complete before 2B (you cannot organise a library of components that don't exist) and
before either Branch 3 node (you cannot lay out screens from undefined components).

---
---

# 🧩 SUB-PROMPT 2A — Atomic & Molecular Design Tokens

```yaml
NODE: 2A
PHASES: 4
INHERITS: [LOCKED_CONSTRAINTS, COLOR_CONTRACT, TYPE_CONTRACT]
EMITS: [COMPONENT_CONTRACT]
CONSUMED_BY: [2B, 3A, 3B]
DEFAULT_MODE: EXTEND
```

### Assemble the prompt

```
1. _shared/context-payload.md      (only if the model can't read the repo)
2. _shared/guardrails.md
3. HANDOFF blocks from 1A and 1B
4. Everything below the ── PROMPT ── line
5. _shared/output-contract.md
```

---

── PROMPT ──

**CONTEXT**

You are a Design Systems Architect building the Figma component library for **RideJaunm**, a
motorcycle riding companion app for Nepal. Phase 4 is **already complete and merged** — see
`docs/04-component-architecture.md`, which specifies ~26 atoms and ~18 molecules including
`Button/SOS`, `Control/RouteMode`, `Card/TelemetryHUD`, `Card/Rider`, `Card/Feed` and
`Overlay/MapControls`.

Locked from the root node: handlebar-mounted, gloved, direct sun. Targets are **48 px minimum /
56 px in-ride / 88 px SOS / 72 px PTT**. Glass is **mode-conditional** and disabled in Day-Glare.

**EXECUTION MODE: `EXTEND`.** The component inventory and behaviour are merged. What is missing
is the **Figma build layer** — the Auto-Layout mechanics, constraint values and property schemas
a designer needs to actually construct these, and the state machines an engineer needs to
implement them.

**TASK**

Convert the merged component descriptions into buildable Figma specifications.

**REQUIREMENTS**

---

**R1 · 🆕 NEW — Auto-Layout & constraint specification for Primary Action Buttons**

`docs/04` §4.1.1 defines `Button/Primary` visually (fill, label, height, radius, padding,
states). What it does not give is the Figma construction spec.

Deliver a complete build table for `Button/Primary` at all four sizes (sm 40 / md 48 /
**lg 56 default in-ride** / xl 64):

| Attribute | Spec needed |
|---|---|
| Auto-Layout direction | horizontal |
| Padding | top/right/bottom/left, per size, in px |
| Item spacing (icon↔label gap) | px, per size |
| Horizontal resizing | `Hug` / `Fill` / `Fixed` — and which is the default per usage context |
| Vertical resizing | `Fixed` (state the value) |
| Alignment | Both axes |
| Text layer resizing | `Hug` vs `Fill`, and the truncation rule |
| Icon frame | Fixed px, `Instance Swap` property name |
| Min width | px — so a 1-character label doesn't collapse |
| Max width | px — before the label truncates |
| Corner radius | Per size, from the radius scale |
| Stroke | Weight + alignment (inside/centre/outside), per theme mode |

Then specify the **map-interaction states** the brief calls out — a button rendered *over the
live map* behaves differently from one on a solid surface:

| State | Fill | Stroke | Shadow/glow | Scale | Duration |
|---|---|---|---|---|---|
| `default` (on surface) | | | | | |
| `default` (over map) | | | | | |
| `hover` | | | | | |
| `pressed/tapped` | | | | | |
| `focused` | | | | | |
| `loading` | | | | | |
| `disabled` | | | | | |
| `default` (Day-Glare, glass disabled) | | | | | |

State explicitly what changes when the same button sits over satellite imagery versus over
`graphite-900`, and how the Day-Glare mode substitution works.

---

**R2 · 🔁 EXTEND — `Button/SOS` safety micro-interaction, frame by frame**

`docs/04` §4.1.1 and `docs/07` §7.2 (MI-2) already specify the 3-second long-press ring, the
haptic ladder (light @0 s, medium @1 s, medium @2 s, heavy @3 s), the 10-second cancel window
and the unwind-on-early-release.

**Do not restate that.** Extend it into an implementable specification:

1. **Frame-by-frame timeline table** at 60 fps, sampled every 250 ms from 0 → 3,000 ms.
   Columns: `t (ms)` · `ring sweep angle (°)` · `ring stroke (px)` · `countdown label` ·
   `background wash opacity` · `button scale` · `haptic` · `audio`.
2. **The conic-gradient construction in Figma** — Figma cannot animate a conic sweep natively.
   Specify how to build it: how many variant frames, the ring geometry (diameter, stroke,
   start angle), and whether this is Smart Animate, a Rive embed, or a Lottie. Give the
   recommendation and the reason.
3. **The early-release unwind**: 240 ms, but specify the easing and whether the countdown label
   reverses, freezes or clears.
4. **Failure-mode analysis** — mandatory for safety-critical components. Answer both:
   - *What happens if it fires accidentally?* (pocket, glove brush, tank-bag pressure)
   - *What happens if it fails silently?* (touch not registered, app backgrounded mid-press,
     screen wet, device thermal-throttled)
   For each, state the guard and the recovery.
5. **The 24 px clearance rule** — `docs/04` mandates it. Specify how it is enforced structurally
   in Figma: is it padding on a wrapper frame, a spacer component, or a documented red-zone
   overlay? Choose one and make it enforceable.

---

**R3 · 🔁 EXTEND — `Card/TelemetryHUD` Auto-Layout stack**

`docs/04` §4.2.1 specifies the three variants (Compact 120 px / Expanded 320 px / Blackout) and
the data priority order (Speed → Distance remaining → ETA → Altitude → Fuel range).

Deliver the **nested Auto-Layout tree** for the Compact variant, as an indented structure with
every frame's direction, padding, spacing, resizing and alignment:

```
Card/TelemetryHUD/Compact  [V-stack, ...]
├── StatusStrip            [H-stack, ...]
│   ├── AltitudeChip       [...]
│   ├── ModeBadge          [...]
│   └── FuelRangeChip      [...]
└── ValueRow               [H-stack, ...]
    ├── ValueBlock/Speed   [V-stack, ...]
    │   ├── Value          [tel-lg, tabular]
    │   └── Unit           [tel-unit]
    ├── ValueBlock/Distance
    └── ValueBlock/ETA
```

Fill in every parameter. Then specify:

- **The width-reservation strategy** so a value changing `9` → `108` → `9` never reflows the
  layout. Give the exact reserved width per metric, derived from the max character count and
  the tabular-numeral advance width.
- **The long-press-to-swap-metric interaction** — which Component Property drives it, and how
  the 12 selectable metrics are modelled (Instance Swap? Variant? Text property?).
- **Sheet-detent transition**: what Auto-Layout changes between Compact (120 px) and Expanded
  (320 px), and whether that is one component with variants or two components.

---

**R4 · 🔁 EXTEND — Interaction state matrix for one-handed thumb navigation**

`docs/04` §4.0 lists the required states. Deliver them as a **complete matrix** across the
components a moving rider actually touches:

Rows: `Button/Primary` · `Button/Icon (glass)` · `Button/SOS` · `Control/RouteMode segment` ·
`Nav/BottomBar tab` · `Card/Rider (swipeable row)` · `Sheet drag handle`

Columns: `default` · `hover` · `focused` · `pressed` · `active/selected` · `loading` ·
`disabled` · `error`

Each cell: the visual delta (fill/stroke/scale/opacity), the transition duration, and the
haptic — or `N/A` where a state genuinely doesn't apply (e.g. `hover` on touch-only surfaces:
say so rather than inventing one).

Then add a **thumb-reach annotation**: for each component, state which reach zone it must live
in (`Easy` bottom 0–45 % / `Stretch` 45–72 % / `Hard` 72–100 %) and whether it is permitted in
Ride Mode at all.

---

**OUTPUT REQUIREMENTS**

- This node is about **buildability**. Every number must be a number.
- Use indented ASCII trees for Auto-Layout structures.
- Tag every requirement 🆕 / 🔁 / ✅.
- `Button/SOS` failure-mode analysis is mandatory and non-negotiable.
- Emit `HANDOFF` with `COMPONENT_CONTRACT`: component names, their Figma property schemas,
  their permitted reach zones, and the state matrix summary.

── END PROMPT ──

---
---

# 🗂️ SUB-PROMPT 2B — Canvas Structure & Design Operations

```yaml
NODE: 2B
PHASES: 8
INHERITS: [LOCKED_CONSTRAINTS, COMPONENT_CONTRACT]
EMITS: [FIGMA_OPS_CONTRACT]
CONSUMED_BY: []
DEFAULT_MODE: EXTEND
```

### Assemble the prompt

```
1. _shared/context-payload.md      (only if the model can't read the repo)
2. _shared/guardrails.md
3. HANDOFF block from 2A
4. Everything below the ── PROMPT ── line
5. _shared/output-contract.md
```

---

── PROMPT ──

**CONTEXT**

You are a Design Operations Lead for **RideJaunm**. Phase 8 is **already complete and merged** —
see `docs/08-figma-setup-roadmap.md`, which specifies the team/project hierarchy, the 10-page
sidebar, naming conventions, section colours, status stamps, branching policy, permissions
(including the isolated SOS file), the plugin set, and a **3-stage / 10-week gated roadmap**.

**EXECUTION MODE: `EXTEND`.** Do not regenerate the page structure or the conventions.

**TASK**

Deliver the three things the merged doc does not have: a developer-handoff-optimised naming
layer, a 4-sprint remap of the roadmap, and the exact Component Property schema for the
3-Way Route Switcher.

**REQUIREMENTS**

---

**R1 · ✅ VALIDATE + 🔁 EXTEND — Page structure, handoff-optimised**

The 10-page sidebar in `docs/08` §8.1.2 is locked:

```
ℹ️ Cover & Specs · 🎨 Design System (Tokens) · 🧩 Atoms & Components · 🎛️ Complex Modules ·
🖼️ Moodboard & Assets · ✏️ Lo-Fi Wireframes · ✨ Hi-Fi Screens · 📱 Prototypes ·
🚧 WIP / Scratch · 🗄️ Archive
```

**Do not change it.** Extend it for developer handoff specifically:

1. For each of the 10 pages: state whether it is **`Dev Mode: Ready`**, **`Reference only`** or
   **`Hidden from developers`**, and why. Engineers should not be reading the moodboard.
2. Specify the **section-level structure inside `✨ Hi-Fi Screens`** — the merged doc says
   "one section per feature area" but doesn't enumerate. Give the exact section list, in order,
   with section colours applied per the status convention.
3. Define the **`Doc/Spec` annotation block schema** — the merged doc mandates one per screen
   but doesn't specify its fields. Deliver the exact field list an engineer needs (behaviour,
   edge cases, offline behaviour, analytics events, API dependency, permission requirements,
   safety-critical flag).
4. Specify **variable-name-to-code-token mapping rules**. `docs/08` says names must match
   exactly. Give the transformation: Figma variable `color/action/primary` →
   CSS `--color-action-primary` → Swift `Color.actionPrimary` → Kotlin `RideJaunmTheme.colors.actionPrimary`.
   Include the collision rules and the casing convention.

---

**R2 · 🆕 NEW — Remap the roadmap into 4 sprints**

`docs/08` §8.2 structures the work as 3 stages over 10 weeks with 3 gates. The brief asks for
**4 design sprints**. Remap it — do not invent new work, **redistribute the existing 38 tasks**.

Deliver:

| Sprint | Weeks | Theme | Entry criteria | Exit criteria / gate | Demo artefact |
|---|---|---|---|---|---|

For each sprint also give:
- The **task list** (referencing the merged task numbers 1–38 so nothing is lost)
- The **critical path** through that sprint
- The **one thing that, if it slips, slips the whole sprint**
- **Parallelisation**: what the second designer works on while the lead is on the critical path

Then state explicitly **how the 4-sprint remap maps onto the 3 merged gates** — do the gates
move, split, or stay? If a gate now falls mid-sprint, say so and justify it.

Constraint to respect: `docs/08` is emphatic that **no hi-fi screen is designed before the
token layer publishes**. Your Sprint 1 must honour that.

---

**R3 · 🆕 NEW — Component Property schema for `Control/RouteMode`**

This is the signature component (`docs/04` §4.1.3) and the highest-value part of this node.
It is a 3-way switch: Straight / Curvy / Supercurvy, 64 px tall, animated sliding pill
indicator, swipeable, with a live stat strip beneath.

Deliver the **complete Figma Component Property schema**:

| Property name | Type | Values / default | Drives what | Exposed on instance? |
|---|---|---|---|---|

Use all four Figma property types deliberately:
- **Variant** — for mutually exclusive states (which segment is selected; which size)
- **Boolean** — for optional elements (stat strip visible? hazard chip visible? labels visible in compact?)
- **Instance Swap** — for the three mode icons
- **Text** — for the live stat values

Then:

1. **Justify the variant explosion.** Compute the total variant count for your schema
   (selected-state × size × theme-mode × …). If it exceeds ~40, **restructure it** — state
   which axes you moved from Variant to Boolean/Instance-Swap and why. A component with 200
   variants is unmaintainable; show the reasoning that avoids that.
2. Specify the three **sizes** from `docs/04`: `full` (planner, 64 px), `compact` (HUD chip,
   40 px, icon-only), `list` (stacked rows with descriptions, onboarding). State whether these
   are variants of one component or separate components — and defend the choice.
3. Specify the **animated pill indicator** construction: how the 320 ms spring slide is built
   given Figma's Smart Animate constraints, what must be named identically across variants for
   it to interpolate, and where it will break.
4. Specify the **swipe gesture** — Figma prototypes support drag; state how to prototype a
   3-position swipe and what the fallback is if it can't be faithfully represented.
5. Specify **how the selected mode's colour propagates** to the pill, the label and the glow
   without hardcoding — i.e. which variable aliases the layers bind to, given that the colour
   changes per segment (`#22C9EE` / `#B4FF39` / `#C25CFF`).
6. Give the **accessibility annotation**: the radiogroup semantics and the exact VoiceOver
   string per segment.

---

**OUTPUT REQUIREMENTS**

- R3 is the highest-value requirement. Go deepest there.
- Property names must be lowercase-hyphenated and stable — engineers will reference them.
- The variant-count computation must be shown, not asserted.
- Tag every requirement 🆕 / 🔁 / ✅.
- Emit `HANDOFF` with `FIGMA_OPS_CONTRACT`: page dev-mode status, the sprint map, and the
  `Control/RouteMode` property schema.

── END PROMPT ──

---
---

## Branch 2 completion gate

- [ ] 2A returned `COMPONENT_CONTRACT` with Auto-Layout specs that a designer can build from
- [ ] `Button/SOS` failure-mode analysis covers both accidental-fire and silent-failure
- [ ] Telemetry width-reservation values are numeric, not descriptive
- [ ] 2B's `Control/RouteMode` variant count is computed and under ~40
- [ ] The 4-sprint remap preserves all 38 merged tasks and honours the tokens-before-hi-fi rule
- [ ] Both self-scored ≥ 8/10
