# 🛑 ROOT NODE — Pre-Flight Alignment & Scoping

> **Status: ANSWERED & LOCKED** · Resolved 2026-08-18 · Tree `v1.0.0`
>
> These four answers are inherited by **every** node in the tree. They are not open questions.
> Each answer below states the decision, the reasoning, the **design consequences it forces**,
> and where it is already encoded in the merged specification.
>
> To change one, follow the amendment process in [`README.md`](README.md#-amendments-to-locked-decisions).

---

## Q1 · Environmental Context

> *Will the user base primarily use this app mounted on a vibrating handlebar in direct
> Himalayan sunlight, or inside an intercom-controlled tank bag?*

### ✅ ANSWER — **Handlebar-mounted, direct sunlight, gloved. Primary and non-negotiable.**

Tank-bag and pocket use are **secondary contexts**, supported but never optimised for. Design
for the worst case: a phone clamped to vibrating bars at 3,000 m, 100,000 lux of high-altitude
sun, a wet or dusty screen, and a rider wearing winter gloves who can look away from the road
for **0.6–0.8 seconds**.

**Why this and not the softer option:** the tank-bag scenario is a strict subset. A UI that
works on vibrating bars in glare also works in a tank bag. The reverse is catastrophically
false — and the failure mode is a rider staring at a screen on a blind mountain corner.

### Consequences forced on every downstream node

| Consequence | Value | Where encoded |
|---|---|---|
| Minimum tap target | 48 px · **56 px in-ride** · 88 px SOS | `docs/04` §4.0 |
| Minimum in-ride type | 14 px, weight ≥ 500 | `docs/02` §2.1.3 |
| No font weight below | 400, anywhere | `docs/02` §2.1.3 |
| Telemetry contrast floor | **7:1** (body 4.5:1, SOS 10:1) | `docs/03` §3.4 |
| Glanceability gate | Parseable at 4 px Gaussian blur in < 1 s | `docs/06` §6.6 |
| Thumb-zone law | All in-ride controls in the bottom 45 % | `docs/05` §5.1.1 |
| Theme modes required | Night · **Day-Glare** · Dusk · Blackout | `docs/03` §3.1.6 |
| Glass behaviour in sun | **Disabled** in Day-Glare — solid fills + borders instead | `docs/07` §7.1.3 |
| Tabular numerals | Mandatory on every runtime-changing value | `docs/02` §2.1.3 |
| Testing protocol | Outdoors, midday, gloved, on the tester's own phone | `README.md` |

> ⚠️ **The trap this answer avoids:** glassmorphism is the signature treatment *and* the thing
> most likely to fail in sunlight. That tension is resolved by making glass **mode-conditional**
> rather than universal. Any node that applies glass unconditionally has violated the root.

---

## Q2 · Visual Vibe Preference

> *Rugged tactical outdoor (military matte, survivalist texture) or high-contrast cyber-tech
> (neon accents, dark glassmorphism)?*

### ✅ ANSWER — **Hybrid: "Himalayan-Tactical Tech."** Neither pure option.

A tactical, expedition-grade base carrying **exactly one** high-energy neon accent.

```
Rugged tactical base          +          One neon accent          =   Himalayan-Tactical Tech
Graphite #0B0F0E                         Volt #B4FF39
Instrument-panel typography              Glassmorphism (mode-conditional)
Honest, dense data                       Cinematic 3D map moments
Hard edges, no ornament                  Glow reserved for interactive + emergency
```

**The allocation rule that makes the hybrid work:**

| Surface class | Treatment | Rationale |
|---|---|---|
| Safety-critical (SOS, mesh, signal, telemetry) | **Tactical.** Hard edges, maximum contrast, zero decoration, no glass | Trust comes from looking like an instrument, not an app |
| Navigational (map, HUD chrome, controls) | **Tactical Glass.** Dark translucent panels over live terrain | The map is the hero; chrome is glass on top of it |
| Aspirational (route preview, feed, post-ride, onboarding) | **Neon-tech.** Glow, gradients, cinematic camera, motion | This is where desire and shareability are manufactured |

**Why not pure tactical:** military-matte alone reads as dated and joyless. It undersells
Supercurvy — the feature that makes the product worth choosing.
**Why not pure cyber-neon:** neon alone reads as a toy. Nobody trusts a toy with an SOS.

**Explicitly rejected:** skeuomorphic leather/carbon-fibre textures, camo patterns,
stencil-military typography, iridescent/holographic gradients, "extreme sports" bro imagery.

📍 Encoded in `docs/01` §1.1–1.2 (5 pillars, 7 keywords, visual constitution), `docs/03` (palette),
`docs/07` §7.1 (depth, glass, gradients).

---

## Q3 · Hardware & Mapping Realities

> *Vector-styled map (Mapbox/Google) or heavily textured satellite-terrain interface?*

### ✅ ANSWER — **Both, switchable, with a hard default: 3D satellite-terrain while riding,
### vector while planning.**

This is not fence-sitting. The two contexts have opposite requirements.

| Context | Style | Why |
|---|---|---|
| **Riding (default)** | **Satellite-3D** — imagery + DEM, 1.4× terrain exaggeration, 60° pitch, 35 % hillshade | A rider navigating a Himalayan valley needs to recognise *the actual landform ahead*. Vector abstraction destroys the single most useful cue: what the mountain looks like. |
| **Planning** | **Terrain-Dark vector** — contours, hypsometric tint, route geometry | Comparing three routes needs legible geometry, not photographic noise. Vector renders route colour cleanly. |
| **Offline** | **Minimal-Offline vector** | Vector tiles are ~10× smaller. Satellite offline is an opt-in premium download. |
| **Day-Glare** | **Terrain-Day vector** | Satellite imagery in direct sun is an unreadable brown smear. |

**Engine:** MapLibre GL (open, self-hostable, no per-tile billing surprise at Nepali scale) with
Mapbox as the commercial fallback. **Four custom styles** built from the `map-*` tokens.

### The hard constraint this forces on Branch 3

Route lines must remain identifiable **on photographic satellite imagery over green terraces,
brown mid-hills, ochre Mustang desert and white glacier** — simultaneously, in sun.

Colour alone cannot survive that. Hence **quad-coding**, which is now a locked invariant:

```
Every route mode = hue + line pattern + icon + text label
Every route line = 2 px graphite-900 casing (so it survives on snow AND on forest)
```

📍 Encoded in `docs/03` §3.1.7 + §3.2, `docs/07` §7.1.7.

---

## Q4 · SOS Hardware Limitations

> *Should the offline mesh UI display technical signal data (dBm, node hops) or stay strictly
> consumer-friendly?*

### ✅ ANSWER — **Progressive disclosure. Plain language by default; technical data one tap away, never hidden.**

Rejecting both extremes, deliberately:

- **Pure consumer-friendly is dishonest.** "Connecting…" tells an injured rider nothing about
  whether help is actually coming. The product's core promise is *degrade, never fail* — that
  promise is only credible if the degradation is legible.
- **Pure technical is unusable.** `-87 dBm / TTL 4 / RSSI variance 12` is noise to a person
  with a broken collarbone.

### The two-layer contract

**Layer 1 — Default. Plain language, always visible, no interaction required.**

```
📶 Cellular    ✖  NO SERVICE        last seen 41 min ago
🛰 GPS         ✔  LOCKED  ±6 m      28.7823°N, 83.6402°E
📡 Mesh        ✔  3 PEERS           nearest 1.5 km (2 hops)
🛰 Satellite   ✖  NOT PAIRED        [Pair inReach/Zoleo]
```

Every row carries a **state + a magnitude + an age**. "3 peers, nearest 1.5 km, 2 hops" is
plain language *and* real information. That is the target register.

**Layer 2 — Diagnostics. Tap any row to expand.**

```
RSSI −74 dBm · link quality 82% · last packet 4 s ago
Route: you → Bibek (1 hop, −68 dBm) → Prakash (2 hops, −81 dBm)
Broadcast interval 30 s · TTL 6 · 512-byte payload · 14 sent / 11 ACK
```

Used by: the rider who wants to know if walking 50 m uphill will help; a rescue coordinator;
and our own field debugging. Also copyable as text for a support ticket.

### The rules this locks

| Rule | Detail |
|---|---|
| **Never show a bare technical value as the primary state** | `−87 dBm` alone is banned; it must be accompanied by "weak / 1 peer" |
| **Never show a fake state** | No "Connecting…" spinner that means nothing. If it's searching, say what it's searching for and for how long |
| **Always show age** | Every status carries a timestamp or a relative age. Stale data is worse than no data |
| **Actionability over precision** | Prefer "Move uphill — no peers in range" to any number |
| **Diagnostics are never behind a settings menu** | One tap from the SOS console. In an emergency, nothing is buried |
| **Both layers work with zero network** | Obviously, and worth stating |

📍 Encoded in `docs/05` Flow B (Signal Matrix, transmission ladder), `docs/06` §6.4,
`docs/04` §4.2.5 (`Widget/SignalMatrix`, `Widget/MeshTopology`).

---

## 📤 ROOT HANDOFF — `LOCKED_CONSTRAINTS`

**Every child node inherits this block verbatim. Paste it, or reference this file.**

```yaml
LOCKED_CONSTRAINTS:
  project: RideJaunm — motorcycle riding companion for Nepal
  direction: Himalayan-Tactical Tech (rugged tactical base + one neon accent)

  environment:
    primary: handlebar-mounted, direct high-altitude sun, gloved hands, vibration
    glance_budget_seconds: 0.8
    tap_target_min_px: 48
    tap_target_inride_px: 56
    tap_target_sos_px: 88
    min_type_px_inride: 14
    min_font_weight: 400
    min_font_weight_inride: 500

  contrast_floors:
    body: 4.5
    telemetry: 7.0
    sos: 10.0

  theme_modes: [night (default), day-glare, dusk, blackout]
  glass: mode-conditional — DISABLED in day-glare, replaced with solid + border

  map:
    engine: MapLibre GL (Mapbox fallback)
    riding_default: satellite-3D (pitch 60°, terrain exaggeration 1.4×)
    planning_default: terrain-dark vector
    offline_default: minimal-offline vector
    route_encoding: QUAD-CODED — hue + line pattern + icon + label
    route_casing: 2px graphite-900 under every route line

  sos:
    disclosure: progressive — plain language default, diagnostics one tap away
    banned: bare technical values as primary state; fake/indeterminate states
    required: every status shows state + magnitude + age
    arming: 3s long-press to arm, 10s cancel window, 3s long-press to stand down
    degradation_ladder: cellular → BLE mesh (TTL 6) → walkie-talkie PTT → breadcrumb + queued SMS

  colour_invariants:
    primary_accent: "#B4FF39"   # volt-400
    secondary: "#22C9EE"        # cyan-400
    dark_base: "#0B0F0E"        # graphite-900
    light_base: "#F7F9F8"       # snow-050
    sos_red: "#FF1F3D"          # RESERVED — emergency only, never decorative
    route_straight: "#22C9EE"
    route_curvy: "#B4FF39"
    route_supercurvy: "#C25CFF"

  type_invariants:
    display: Space Grotesk 400–700
    body: Inter 400–700 with tabular-nums on all changing values
    devanagari: Mukta (+ Noto Sans Devanagari fallback)
    mono: JetBrains Mono

  source_of_truth_order:
    1: tokens/ridejaunm.tokens.json
    2: docs/01–09
    3: prompts/ (this tree)
    4: LLM output (draft until merged)
```

---

## 🔁 If a stakeholder reopens one of these

| Question reopened | Blast radius | Cost |
|---|---|---|
| **Q1 Environment** | Everything. Sizing, type, contrast, modes, testing. | Catastrophic — full re-spec |
| **Q2 Vibe** | `docs/01`, `03`, `07` + all hi-fi. Structure survives. | High — ~3 weeks of surface work |
| **Q3 Map** | `docs/03` §3.1.7, `docs/07` §7.1.7, Branch 3 layouts, offline sizing model | Medium — map style + tile budget |
| **Q4 SOS disclosure** | `docs/04` §4.2.5, `docs/05` Flow B, `docs/06` §6.4 | Low–medium — contained to the SOS subsystem |

Q4 is the only one that is cheap to revisit. Treat Q1 as effectively immutable.
