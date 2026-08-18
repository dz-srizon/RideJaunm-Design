# 🌿 BRANCH 1 — Brand, Aesthetics & Design System

**Covers:** Phases 1–3 · **Inherits:** `LOCKED_CONSTRAINTS` (root) · **Blocks:** Branches 2 and 3

1A and 1B are **independent of each other** and can run in parallel. Both must complete before
any Branch 2 or Branch 3 node runs — colour and type are the substrate everything else sits on.

---
---

# 🎨 SUB-PROMPT 1A — Visual Identity & Colour Tokens

```yaml
NODE: 1A
PHASES: 1, 3
INHERITS: [LOCKED_CONSTRAINTS]
EMITS: [COLOR_CONTRACT, PERSONA_SET]
CONSUMED_BY: [2A, 3A, 3B]
DEFAULT_MODE: EXTEND
```

### Assemble the prompt

```
1. _shared/context-payload.md      (only if the model can't read the repo)
2. _shared/guardrails.md
3. Everything below the ── PROMPT ── line
4. _shared/output-contract.md
```

---

── PROMPT ──

**CONTEXT**

You are a Principal Mobile UI/UX Design Strategist working on **RideJaunm**, a motorcycle
riding companion app for Nepal. Phases 1–8 of the design specification are **already complete
and merged** — see `docs/01-brand-identity.md` and `docs/03-color-system.md`.

The root node is answered and locked:
- **Environment:** handlebar-mounted, direct Himalayan sun, gloved, 0.8 s glance budget
- **Vibe:** Himalayan-Tactical Tech (tactical base + one neon accent) — *not* pure tactical, *not* pure cyber
- **Map:** satellite-3D while riding, vector while planning
- **SOS disclosure:** progressive — plain language default, diagnostics one tap away

**EXECUTION MODE: `EXTEND`.** The colour system is merged and authoritative. Do **not**
regenerate it. Produce only the delta, and validate the rest.

**TASK**

Complete the brand identity architecture by delivering the missing persona layer, and
rigorously validate the existing colour system against conditions it has not yet been
tested for.

**REQUIREMENTS**

---

**R1 · 🆕 NEW — Three rider sub-culture personas**

`docs/01` defines brand *pillars* but no personas. This is the single biggest gap in the
brand layer. Build three, grounded in real Nepali riding sub-cultures:

1. **Highway Cruisers** — long-distance tarmac tourers (Prithvi/BP/Mahendra highways)
2. **Off-road Dirt Racers** — trail, gravel and enduro riders (Shivapuri, Kakani, Nagarjun, Sundarijal)
3. **Mountain Tourers** — high-altitude expedition riders (Mustang, Manang, Upper Dolpa, Rara)

For **each** persona deliver:

| Field | Requirement |
|---|---|
| Name, age, home district | Real Nepali naming, real geography |
| Bike | From the Nepal market: RE Himalayan 450 / Classic 350 / Hunter 350 / Pulsar NS200 / Xpulse 200 / KTM Duke / Honda CB / TVS Apache / CT100 |
| Riding pattern | Frequency, typical distance, solo vs group ratio, season |
| Phone + data reality | Device tier, storage headroom, monthly data budget in NPR, NTC vs Ncell |
| Primary job-to-be-done | One sentence, in their words |
| Top 3 anxieties | Concrete, not abstract ("will there be petrol at Chame", not "safety") |
| **Which of the 6 core features they actually use** | Ranked 1–6, with the ones they'd never open marked |
| **Which routing mode they default to** | Straight / Curvy / Supercurvy, and why |
| **Their SOS scenario** | The specific situation where they'd need it |
| **Design implication** | 2–3 concrete decisions this persona forces — a size, a default, a screen, a setting |
| **Anti-need** | One thing we should NOT build for them (persona discipline) |

Then produce a **persona × feature priority matrix** (3 personas × 6 features, marked
`Primary / Secondary / Rare / Never`) and state **which persona is the design default** when
their needs conflict — with reasoning.

---

**R2 · ✅ VALIDATE — High-glare colour palette, light and dark**

The palette is merged in `docs/03` §3.1 with full hex ramps for Volt, Cyan, Graphite, Snowline
and the semantic set, plus 4 theme modes.

**Do not regenerate it.** Instead:

1. Produce a **compact two-column reference table** (Night mode ↔ Day-Glare mode) for the
   **12 semantic roles only**: `bg/base`, `bg/surface`, `bg/raised`, `text/primary`,
   `text/secondary`, `text/tertiary`, `border/default`, `border/strong`, `action/primary`,
   `action/primary-text`, `focus/ring`, `overlay/scrim`. Give the token name and hex for each,
   in both modes. This is the table a developer actually needs and it does not currently exist
   in one place.
2. **Flag any role where the Day-Glare value is missing or under-specified** in `docs/03`.
3. Confirm or challenge: is `snow-050 #F7F9F8` light enough at 100,000 lux, or does Day-Glare
   need to go to `snow-000 #FFFFFF` despite the veiling-glare concern? Give a recommendation
   with reasoning, and flag it as a `TOKEN GAP` if you propose a change.

---

**R3 · 🔁 EXTEND — Route colours validated over satellite terrain**

`docs/03` §3.2 fixes the three route colours and mandates quad-coding. What does **not** exist
is a validation matrix proving they survive real Nepali terrain imagery.

Build a **4 × 3 legibility matrix**: the three route colours against four terrain backdrops
sampled from actual satellite imagery:

| Terrain | Approx. backdrop | Where |
|---|---|---|
| Monsoon green terraces | `#3E5B2E` | Mid-hills, Asar–Bhadra |
| Winter brown mid-hills | `#6B5A3E` | Mid-hills, Mangsir–Falgun |
| Mustang ochre desert | `#A8865C` | Trans-Himalaya |
| Glacier white-blue | `#DCE8EE` | Above snowline |

For each of the 12 cells: state the **contrast ratio**, a verdict (`PASS` / `MARGINAL` / `FAIL`),
and — where marginal or failing — **which secondary coding channel rescues it** (casing weight,
dash pattern, glow, icon, label).

Then specify the **adaptive casing rule**: the 2 px `graphite-900` casing is mandated, but state
precisely when it must widen to 3 px, and what triggers that (a luminance threshold on the
sampled underlying tile — give the number).

Also confirm: does `route-supercurvy #C25CFF` hold against the ochre Mustang backdrop, which is
the closest hue-neighbour of the four? If not, propose the fix **within** existing tokens.

---

**R4 · ✅ VALIDATE — SOS Red accessibility**

`docs/03` §3.1.5 specifies `sos-500 #FF1F3D` at 6.3:1 on `graphite-900`, with usage rules.

The brief asks for **WCAG AAA**. Address this honestly:

1. State the actual measured ratios: `sos-500` on `graphite-900`, and `snow-000` on `sos-500`.
2. **AAA for normal text is 7:1. `#FF1F3D` on `#0B0F0E` is 6.3:1 — it does not meet AAA as
   normal-size text.** Do not pretend otherwise. Explain precisely how the merged spec resolves
   this (large-text exemption at ≥17 px/700 where AAA is 4.5:1, plus a filled-surface variant),
   and state whether that resolution is sufficient.
3. If a true-AAA-at-any-size variant is needed, propose `sos-450` or similar as an explicit
   `TOKEN GAP` with a hex and its measured ratio — **do not silently change `sos-500`**, which
   is a locked constraint.
4. Restate the triple-coding requirement (colour + SOS glyph + haptic/audio) and explain why
   triple-coding matters more than the last 0.7 of a contrast point for a rider who may be
   concussed, in shock, or looking at the screen in peripheral vision.

---

**OUTPUT REQUIREMENTS**

- Tag every requirement 🆕 / 🔁 / ✅ in section 0 before answering it.
- Personas get the most depth — that is the real delta here.
- Every colour cited as `token-name #HEX`.
- Every contrast ratio to one decimal place.
- No new tokens without an explicit `⚠️ TOKEN GAP` block.
- Emit `HANDOFF` with `COLOR_CONTRACT` (the 12 semantic roles, both modes) and `PERSONA_SET`
  (the three personas condensed to name / bike / default-route-mode / top-feature /
  design-implication).

── END PROMPT ──

---
---

# ✍️ SUB-PROMPT 1B — Glanceable Typography & Scale

```yaml
NODE: 1B
PHASES: 2
INHERITS: [LOCKED_CONSTRAINTS]
EMITS: [TYPE_CONTRACT]
CONSUMED_BY: [2A, 3A, 3B]
DEFAULT_MODE: EXTEND
PARALLEL_WITH: 1A
```

### Assemble the prompt

```
1. _shared/context-payload.md      (only if the model can't read the repo)
2. _shared/guardrails.md
3. Everything below the ── PROMPT ── line
4. _shared/output-contract.md
```

---

── PROMPT ──

**CONTEXT**

You are a Principal Mobile UI/UX Design Strategist working on **RideJaunm**, a motorcycle
riding companion app for Nepal. Phase 2 typography is **already complete and merged** — see
`docs/02-typography.md`.

Locked from the root node:
- Reading conditions: **60 km/h, vibrating handlebar mount, direct high-altitude sun,
  through a visor, 0.6–0.8 s glance budget**
- Current stack: **Space Grotesk** (display/telemetry) + **Inter** (body, `tnum`) +
  **Mukta** (Devanagari) + **JetBrains Mono** (coordinates)
- Floors: no weight < 400 anywhere, ≥ 500 in-ride, ≥ 14 px in-ride, 11 px absolute minimum

**EXECUTION MODE: `EXTEND`.** The scale is merged and authoritative — 17 Latin steps, 7
telemetry steps, 7 Devanagari overrides. Do **not** regenerate it.

**TASK**

Stress-test the typographic system against letterform confusion at speed, and specify the
micro-typography for telemetry that the merged docs define structurally but not yet
optically.

**REQUIREMENTS**

---

**R1 · 🔁 EXTEND — Two font pairings, compared, with a defended recommendation**

`docs/02` §2.1.2 lists runners-up but never runs a head-to-head. Do that properly.

**Pairing A (incumbent):** Space Grotesk + Inter
**Pairing B (challenger):** propose **one** genuine alternative from Google Fonts. Candidates
worth considering: Barlow + IBM Plex Sans, Archivo + Inter, Roboto Flex + Roboto, Public Sans +
Source Sans 3. Justify your challenger choice.

Compare them across a **letterform-confusion audit** — the failure mode that actually matters
when a rider glances at a speed readout on a vibrating mount:

| Confusion pair | Why it's dangerous here | Pairing A verdict | Pairing B verdict |
|---|---|---|---|
| `1` / `l` / `I` | Distance, ETA, altitude misreads | | |
| `0` / `O` | Speed, coordinates | | |
| `5` / `S` | Speed at a glance | | |
| `6` / `8` / `9` | Distance remaining | | |
| `3` / `8` | Speed | | |
| `rn` / `m` | Unit labels (`km` vs `krn`) | | |
| `2` / `Z` | Coordinates, grid refs | | |

Then evaluate both on: x-height ratio, aperture openness, stroke contrast under vibration
blur, weight availability, **Devanagari companion quality**, variable-font support, and
rendering at 11 px on a low-DPI Android panel.

**Deliver a verdict.** If Pairing A wins, say so and state the one condition under which you'd
switch. If B wins, that is a `LOCKED_CONSTRAINT` challenge — raise a `🛑 ESCALATION`, do not
just recommend it.

---

**R2 · ✅ VALIDATE + 🔁 EXTEND — The complete scale table**

`docs/02` §2.2.1 has the 17-step Latin scale with token, style name, family, weight, size,
line-height, tracking, case and usage. **Do not regenerate it.**

Instead, add the **two columns it is missing**, which are the ones that matter for this
environment:

| Existing token | 🆕 Min. glance distance (cm) | 🆕 Vibration verdict |
|---|---|---|

- **Min. glance distance**: the furthest distance at which this style is reliably legible at
  arm's length on a 393 px-wide device. Calculate from the size and weight; state your formula.
- **Vibration verdict**: `IN-RIDE SAFE` / `STATIONARY ONLY` / `NEVER IN-RIDE` — and for
  anything not in-ride-safe, name the surfaces where it is therefore banned.

Then produce a single consolidated **"in-ride permitted styles"** shortlist — the subset a
designer may use on any surface visible while moving. This list does not currently exist and
it is the most useful artefact this node can produce.

---

**R3 · 🆕 NEW — Telemetry micro-typography**

`docs/02` §2.2.2 defines the 7-step telemetry scale. What it does **not** specify is the
optical detail. Deliver, for **speedometer**, **altimeter** and **satellite/connection status**:

For each of the three:

| Spec | Requirement |
|---|---|
| Style token + exact size | From the existing `tel-*` scale |
| **Digit reservation** | Max character count, and the fixed width reserved so the layout never reflows (`88` → `188` must not shift anything) |
| **Value/unit relationship** | Exact baseline offset in px, gap in px, unit opacity |
| **Leading-zero policy** | `08` vs `8` — decide and justify per metric |
| **Decimal policy** | When decimals appear, when they're dropped, rounding rule |
| **Transition behaviour** | Does the digit roll, cross-fade or snap? Duration in ms. What happens at 1 Hz update vs 10 Hz |
| **Stale-data treatment** | Exact opacity and what badge appears, at what age threshold |
| **No-data treatment** | What renders instead of a number (`--`), and its style |
| **Negative/edge values** | Altitude below sea level, speed 0, ETA past-due |
| **Devanagari variant** | If units localise (`किमी/घण्टा`), what changes |

Specifically for **satellite/connection status**: it is not a number — specify the exact
`caption-caps` / `micro-caps` treatment, the state vocabulary (`SEARCHING` / `±6 M` /
`STALE 12S` / `NO FIX`), and how it reconciles with the root-node rule that every status shows
**state + magnitude + age**.

---

**R4 · 🆕 NEW — The blur-test specification**

`docs/06` §6.6 mandates a 4 px Gaussian blur test but never defines the pass criteria for type.

Specify the test formally: blur radius, viewing distance, exposure time, what counts as a pass
for each of the three tiers (telemetry / heading / body), and the **remediation ladder** when a
style fails — in order: increase weight → increase size → increase tracking → change case →
reduce information density. State when each step is the right one.

---

**OUTPUT REQUIREMENTS**

- Tag every requirement 🆕 / 🔁 / ✅ before answering.
- R1's confusion audit and R3's telemetry spec carry the most value — go deep there.
- Every size in px, every duration in ms, every opacity as a decimal.
- Do not introduce a new type size. If one is genuinely needed, raise `⚠️ TOKEN GAP`.
- Emit `HANDOFF` with `TYPE_CONTRACT`: the in-ride permitted shortlist, the telemetry
  micro-spec summary, and the blur-test pass criteria.

── END PROMPT ──

---
---

## Branch 1 completion gate

Before running any Branch 2 or Branch 3 node, confirm:

- [ ] 1A returned `COLOR_CONTRACT` with all 12 semantic roles in both modes
- [ ] 1A returned `PERSONA_SET` with a declared design-default persona
- [ ] 1A's route-over-terrain matrix has no unresolved `FAIL` cells
- [ ] 1B returned `TYPE_CONTRACT` with the in-ride permitted shortlist
- [ ] Neither node proposed a token that hasn't been triaged into
      `tokens/ridejaunm.tokens.json`
- [ ] Both self-scored ≥ 8/10
