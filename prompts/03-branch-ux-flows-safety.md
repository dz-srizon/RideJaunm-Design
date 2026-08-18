# 🌿 BRANCH 3 — UX Mapping, Flows & Safety Controls

**Covers:** Phases 5–7 · **Inherits:** `LOCKED_CONSTRAINTS`, `COLOR_CONTRACT`, `TYPE_CONTRACT`,
`COMPONENT_CONTRACT`

3A must complete before 3B — the SOS console inherits the HUD's map-chrome grammar, and the
offline transition choreography depends on knowing what the online HUD looks like.

---
---

# 🗺️ SUB-PROMPT 3A — Navigation HUD & 3D Routing UI

```yaml
NODE: 3A
PHASES: 5, 6 (Screens 1 & 2)
INHERITS: [LOCKED_CONSTRAINTS, COLOR_CONTRACT, TYPE_CONTRACT, COMPONENT_CONTRACT]
EMITS: [HUD_CONTRACT]
CONSUMED_BY: [3B]
DEFAULT_MODE: EXTEND
```

### Assemble the prompt

```
1. _shared/context-payload.md      (only if the model can't read the repo)
2. _shared/guardrails.md
3. HANDOFF blocks from 1A, 1B, 2A
4. Everything below the ── PROMPT ── line
5. _shared/output-contract.md
```

---

── PROMPT ──

**CONTEXT**

You are a Principal UX Architect working on **RideJaunm**, a motorcycle riding companion app for
Nepal. Phases 5 and 6 are **already complete and merged**:

- `docs/05` — 5-tab bottom nav (Ride / Plan / **SOS centre** / Squad / Profile), full sitemap,
  **Flow A: Kathmandu → Daman via the Tribhuvan Rajpath** as a group Supercurvy trip
- `docs/06` — zone-by-zone pixel blueprints for Screen 1 (Map Home / HUD) and Screen 2
  (Trip Planner), with 7 and 8 states respectively

Locked from the root node: satellite-3D while riding (pitch 60°, terrain exaggeration 1.4×),
vector while planning, route lines **quad-coded** with a 2 px `graphite-900` casing.

**EXECUTION MODE: `EXTEND`.** The layouts and Flow A are merged. Do **not** regenerate them.

**TASK**

Deliver the Kathmandu → Pokhara flow — which is a genuinely different routing problem from the
merged Daman flow — plus the map-chrome scaling rules and the ride-mode nav behaviour that the
merged docs describe but do not parameterise.

**REQUIREMENTS**

---

**R1 · 🆕 NEW — "Supercurvy Group Trip: Kathmandu → Pokhara", step by step**

`docs/05` Flow A covers Kathmandu → Daman. **Pokhara is a different problem and that is exactly
why it is worth specifying:**

> The Prithvi Highway (Kathmandu → Mugling → Pokhara, ~200 km) is the *straight* corridor —
> it is the default, it is what every rider knows, and it has relatively few bends for its
> length. A genuine Supercurvy route must **divert** — via Nuwakot/Devighat, or Bandipur, or
> the Dumre–Besisahar spur — adding substantial distance and time.
>
> **The core UX problem this flow must solve: how do you sell a rider (and five friends) on
> a route that is 90 km longer and 2.5 hours slower, without it reading as an error?**

Deliver the flow as a step-by-step wireframe-logic map in the same format as `docs/05` Flow A
(ASCII boxes, entry/exit, decision branches). Cover:

1. **Entry** — Plan tab, with Pokhara as a saved/recent destination
2. **Group-first ordering** — trip type selected *before* routing, per the merged flow
3. **Route computation** — three routes drawn simultaneously with realistic numbers:
   - `STRAIGHT` — Prithvi Highway, ~200 km, ~5 h 30 m, low bend count
   - `CURVY` — a moderate diversion, state which
   - `SUPERCURVY` — the real diversion, state the actual route and the honest penalty
4. **⭐ The persuasion moment** — this is the requirement that matters. Specify exactly what UI
   makes the +90 km / +2.5 h feel like a *reward* rather than a *mistake*:
   - What the curvature meter shows
   - What comparative framing appears (bends-per-km? a "why this route" chip?)
   - What the cinematic flyover previews
   - What the honest cost disclosure says, and where it sits relative to the reward framing
   - **Where the line is** between persuasive and manipulative — state it explicitly
5. **The multi-day question** — a Supercurvy Kathmandu→Pokhara likely exceeds comfortable
   daylight riding. Does the planner suggest an overnight at Bandipur? Specify that branch.
6. **Group readiness** — offline tiles for a 290 km corridor, fuel gaps, the Mugling–Pokhara
   landslide-prone segments, and the "2 of 6 riders missing tiles" nudge
7. **Edge cases** — at minimum: a rider joins from Chitwan (different origin), the group
   splits mid-route, monsoon closes the Bandipur spur, one rider's phone can't hold 400 MB

Format matching `docs/05` Flow A. Include a **success metric** (taps and seconds) as that flow does.

---

**R2 · 🔁 EXTEND — 3D map viewport and overlay-control scaling**

`docs/06` §6.1 gives the zone blueprint (Z0–Z8) with pixel heights, and `docs/04` §4.2.4
specifies the map controls. What does **not** exist is the scaling and occlusion logic.

Deliver:

1. **The occlusion budget** — `docs/06` states map visibility must never drop below 55 % of the
   viewport. Prove it: compute the actual occluded percentage in each of the four HUD states
   (idle / navigating-solo / navigating-group / SOS-active) on a 393 × 852 frame. Show the
   arithmetic. Flag any state that breaches 45 % occlusion.
2. **The active-navigation-line protection rule.** The route line is the one thing chrome must
   never cover. Specify a **protected corridor**: how many px either side of the drawn route,
   and what the system does when a control would overlap it (shift? fade? collapse?). Give the
   priority order for which chrome yields.
3. **Control scaling across pitch.** At 0° pitch the map is a plan view; at 60° the horizon
   enters the frame. Specify what happens to: the compass (does it gain a pitch indicator?),
   the scale bar (which is meaningless at high pitch — does it hide?), the squad bubbles
   (do they scale with perspective or stay screen-space?), and the route casing (does 2 px
   hold at 60° pitch, or does the far-field route need widening?).
4. **Progressive chrome disclosure by speed** — merged docs say the nav bar auto-hides above
   15 km/h for 10 s. Extend that into a full ladder:

   | Speed band | Nav bar | Map controls | Search pill | Squad strip | HUD sheet | Rationale |
   |---|---|---|---|---|---|---|
   | 0 (stopped) | | | | | | |
   | 1–15 km/h | | | | | | |
   | 15–40 km/h | | | | | | |
   | 40–80 km/h | | | | | | |
   | > 80 km/h | | | | | | |

   State the hysteresis (what prevents flicker at exactly 15 km/h) and the manual override.
5. **Offline-boundary rendering** — how the downloaded-region boundary is drawn on the live map
   during navigation, and what the rider sees as they approach and cross the edge of their
   downloaded tiles. This is a genuinely dangerous moment and the merged docs don't cover it.

---

**R3 · 🔁 EXTEND — Bottom navigation, one-handed layout pattern**

`docs/05` §5.1.1 fixes the 5 tabs with SOS at centre. Extend into the physical layout spec:

1. **Reach geometry** — on a 393 × 852 device, give each tab's centre coordinate and its
   distance from the natural right-thumb pivot (state where you place the pivot and why).
   Produce a reach-difficulty ranking and confirm it matches the merged tab ordering. If it
   doesn't, raise it.
2. **Left-handed / left-mount riders** — Nepali riders often mount phones on the left bar or
   ride with a throttle-hand constraint. Specify a mirrored layout option: what mirrors, what
   must not (SOS stays centre), and how it's toggled.
3. **The SOS tab's distinct treatment** — it is centre, raised, `sos-500`-ringed, never badged
   and never auto-hidden. Specify its exact geometry within the 64 px bar: does it break the
   bar's top edge? By how many px? What is its hit area versus its visual size?
4. **Ride-mode collapse choreography** — when the bar auto-hides, specify the animation
   (duration, easing, what the SOS tab morphs into, where the 88 px FAB lands), the return
   gesture, and what happens to a badge that arrives while hidden.
5. **Safe-area and gesture-bar conflict** — the 34 px home indicator sits under the bar.
   Specify the padding, and how you prevent the iOS swipe-up gesture from conflicting with a
   tab tap at the bottom edge — a real failure mode with gloves.

---

**OUTPUT REQUIREMENTS**

- R1 is the centrepiece. Match the ASCII flow-box format of `docs/05` Flow A exactly.
- All geometry in px on a 393 × 852 frame; show arithmetic where you compute percentages.
- Real Nepali geography with real distances — Nuwakot, Bandipur, Dumre, Mugling, Damauli.
- Tag every requirement 🆕 / 🔁 / ✅.
- Emit `HANDOFF` with `HUD_CONTRACT`: the occlusion budget per state, the speed-ladder table,
  the protected-corridor rule, and the nav-bar geometry.

── END PROMPT ──

---
---

# 🚨 SUB-PROMPT 3B — Dual-Mode SOS & Community Infrastructure

```yaml
NODE: 3B
PHASES: 5, 6, 7 (Screens 3 & 4)
INHERITS: [LOCKED_CONSTRAINTS, COLOR_CONTRACT, TYPE_CONTRACT, COMPONENT_CONTRACT, HUD_CONTRACT]
EMITS: [SAFETY_CONTRACT, FEED_CONTRACT]
CONSUMED_BY: []
DEFAULT_MODE: EXTEND
SAFETY_CRITICAL: true
```

> ⚠️ **This node is safety-critical.** Output requires a second human reviewer and an explicit
> failure-mode statement before anything merges. Never guess on safety behaviour — escalate.

### Assemble the prompt

```
1. _shared/context-payload.md      (only if the model can't read the repo)
2. _shared/guardrails.md
3. HANDOFF blocks from 1A, 1B, 2A, 3A
4. Everything below the ── PROMPT ── line
5. _shared/output-contract.md
```

---

── PROMPT ──

**CONTEXT**

You are a Principal UX Architect and safety-systems designer working on **RideJaunm**. Phases 5,
6 and 7 are **already complete and merged**:

- `docs/05` **Flow B** — offline SOS in a cellular dead zone (Beni → Jomsom crash scenario),
  including the crash-detection parallel path, the 4-tier degradation ladder, the 3 s arm /
  10 s cancel / 3 s stand-down protocol, and 6 edge cases
- `docs/06` §6.3 (Community Feed) and §6.4 (SOS Console) zone blueprints
- `docs/07` §7.2 — three mandatory micro-interactions: MI-1 GPS Lock Ripple,
  MI-2 SOS Commitment Ring, MI-3 Offline Tile Materialisation

Locked from the root node: **progressive disclosure** — plain language default, technical
diagnostics one tap away, every status showing **state + magnitude + age**.

**EXECUTION MODE: `EXTEND`.** Flow B, the console blueprint and the three micro-interactions are
merged. Do **not** regenerate them.

**TASK**

Specify the two things that are genuinely missing: the *transition choreography* between
connectivity modes, and the *responder side* of the SOS system — which is currently half a
feature. Then complete the feed's hi-fi specification.

**REQUIREMENTS**

---

**R1 · 🆕 NEW — The online → offline transition, choreographed**

`docs/05` Flow B states that offline resilience mode enables "silently" when cellular drops for
> 60 s above 2,000 m. That is the *policy*. The *choreography* does not exist, and this is the
moment where a rider either learns to trust the product or learns to distrust it.

Deliver a **timed sequence table** from the last good cellular packet to fully-established mesh:

| t | System state | What the rider sees | What they hear/feel | Reversibility |
|---|---|---|---|---|
| t+0 s | last successful cellular packet | | | |
| t+5 s | first failed request | | | |
| t+15 s | | | | |
| t+30 s | | | | |
| t+60 s | offline mode arms | | | |
| t+61 s | BLE advertising begins | | | |
| t+75 s | first peer discovered | | | |
| t+90 s | mesh established, N peers | | | |
| — | cellular returns | | | |

Design rules to satisfy and state explicitly:
- **No modal.** The rider is moving. Nothing may demand a tap.
- **No false alarm.** A 5-second tunnel dropout must not trigger the full transition — specify
  the debounce and the hysteresis.
- **Honest, not alarming.** The banner must inform without implying danger.
- **What silently degrades** — enumerate exactly which features stop working, and whether the
  rider is told at transition time or only when they try to use one.
- **The return path** — when cellular comes back, what re-syncs, in what order, and what the
  rider sees. Does queued content post automatically? What about a queued SOS?

Then specify the **banner state machine** (`docs/04` §4.2.5 `Banner/Connectivity`): every state,
its colour token, its copy string, its priority, and the transition rules between them.
Priority order is locked: **SOS > Mesh > Offline > Sync**.

---

**R2 · 🔁 EXTEND — Emergency SOS Console: mesh map, peer count, broadcast status**

`docs/06` §6.4 gives the console zone blueprint. `docs/04` §4.2.5 names `Widget/MeshTopology`
and `Widget/SignalMatrix` but does not specify their internals. Deliver those:

**`Widget/MeshTopology`** — the node-graph visualisation:
- Exact geometry: canvas size, "you" node size and position, peer node size, orbit radii
- How **hop count** maps to orbit ring (1 hop / 2 hops / 3+ hops)
- How **link quality** maps to edge thickness — give the px values and the RSSI thresholds
- The animated packet-dot behaviour: speed, spacing, direction, what a *failed* relay looks like
- Colour: peers use the prayer-flag spectrum; specify what an **unacknowledged** peer looks like
  versus an **acknowledged** one
- Layouts for 0 peers, 1 peer, 3 peers, 8 peers, 20+ peers (clustering rule)
- **The 0-peer state is the most important one.** What does a rider alone in a dead zone see?
  It must be honest ("no riders in range") and actionable ("higher ground may reach a peer")
  without being hopeless.

**`Widget/SignalMatrix`** — the 4-row honesty panel:
- Row anatomy in px: icon, label, state, magnitude, age, expand affordance
- The **expanded diagnostic drawer** per row (root-node Layer 2): exact fields, and the
  copy-as-text affordance for support/rescue coordination
- The refresh cadence per row, and how "age" is rendered as it grows (`4 s` → `41 min` → `2 h`)
- What each row shows when the underlying radio is **off** versus **failing** — these are
  different and must look different

**Broadcast status** — the transmission ladder from Flow B, as a component:
- Per-channel state vocabulary (`SENT` / `RETRYING` / `STANDBY` / `FAILED` / `NOT PAIRED`)
- ACK counting and how it renders (`3 peers · 2 ACK`)
- The re-broadcast countdown (30 s, stretching to 120 s below 15 % battery) — is it a ring,
  a bar, or a number? Justify against the 0.8 s glance budget and a possibly-injured rider.

---

**R3 · 🆕 NEW — The responder side**

This is the biggest gap in the entire specification. `docs/05` Flow B mentions in one line that
peers "receive a full-screen red alert, siren, one-tap I'M COMING". **Half of the SOS feature is
the other rider's screens, and they are unspecified.**

Deliver the responder experience as a flow plus screen blueprints:

1. **Alert receipt** — a rider is *moving at 60 km/h* when a squad member's SOS arrives.
   How is this delivered without causing a second accident? Specify: audio, haptic, screen
   behaviour, and whether it interrupts navigation. State the failure mode you are guarding
   against.
2. **The acknowledge interaction** — one-tap "I'M COMING" is specified, but: what is its tap
   target, where is it placed, what if the responder is also gloved and moving, and what
   happens if they *can't* respond (they're ahead, out of range, or also in trouble)?
3. **Navigate-to-victim** — routing to the coordinates on **offline tiles**, with the victim as
   a live-updating destination. Specify what the HUD looks like in "responder mode" — what
   changes from normal navigation, what is suppressed, what is added.
4. **The responder coordination layer** — if three riders acknowledge, they need to not all
   converge uselessly. Specify: who is designated primary (nearest? first-ACK?), what the
   others are told, and how roles are shown.
5. **The mesh relay role** — a rider may be relaying a packet for a victim they cannot reach.
   Specify what that rider sees: they are *part of the rescue* without being the responder, and
   they must not move out of relay position without knowing they'll break the chain. This is a
   subtle and important state.
6. **The all-clear** — what the responder sees when the victim stands down, and the incident
   report they receive.

Include the screen blueprint for the **responder alert screen** in the zone format of `docs/06`.

---

**R4 · 🔁 EXTEND — Feed card hi-fi specification**

`docs/04` §4.2.3 and `docs/06` §6.3 specify the feed card's anatomy and layout. `docs/07` gives
the depth and glass system. Complete the hi-fi visual spec:

| Spec | Requirement |
|---|---|
| **Media aspect ratios** | Exact permitted ratios (1:1, 4:5, 16:9, multi-up grid) with px dimensions at 393 frame width, and the rule for what happens to a non-conforming upload |
| **Text truncation** | Exact character counts and line clamps for: caption (3 lines), display name, location, hashtag overflow, comment preview. State the truncation glyph and whether "… more" is inline or a new line |
| **Route-strip rendering** | The 96 px map strip: zoom-fit rule, route line weight at that scale (the 7 px in-app line cannot be 7 px here — give the value), whether casing survives, and how the mode badge is placed |
| **Elevation & shadow** | Which elevation layer (L1?), exact shadow values, and whether feed cards get any glass (they should not — state why) |
| **Border radius** | Card, media container, route strip, avatar — with the nesting-law arithmetic shown |
| **Background blur** | Where blur is permitted in the feed (video poster? story rail?) and where it is banned |
| **Dark/light/day-glare** | What changes per mode, especially media rendering under Day-Glare |
| **Data-saver variant** | The low-data card: what is removed, what is retained, exact layout delta |
| **Offline-queued variant** | The `warning-400` left border + `QUEUED` pill — exact geometry |

---

**R5 · 🔁 EXTEND — Three micro-interactions validating background operations**

`docs/07` §7.2 already specifies MI-1 (GPS Lock Ripple), MI-2 (SOS Commitment Ring) and MI-3
(Offline Tile Materialisation). **Do not restate them.**

Propose **three new** micro-interactions that specifically validate *background* system
operations — the things happening while the rider isn't looking, which they must be able to
trust at a glance. Suggested territory (choose three, or better ones):

- **Mesh heartbeat** — proof the mesh is alive and re-broadcasting, without draining attention
- **Breadcrumb write confirmation** — proof the trail is being recorded even with no signal
- **Queued-content flush** — the moment connectivity returns and 14 queued items send
- **Peer join/leave** — a rider entering or dropping out of mesh range
- **Battery-guardian downshift** — the moment broadcast interval stretches 30 s → 120 s

For each of the three, specify: trigger, duration, exact visual (with tokens), haptic, audio,
Reduce Motion behaviour, **what happens if the underlying operation fails**, and — critically —
**why this deserves motion at all** rather than a static indicator. Attention is the scarcest
resource on a moving motorcycle; every animation must earn its place.

---

**OUTPUT REQUIREMENTS**

- **R3 (responder side) is the highest-value requirement in the entire tree.** Go deepest there.
- Every safety-critical behaviour states its failure mode: accidental trigger AND silent failure.
- No emoji, no humour, no exclamation marks in any SOS copy string you write.
- Every status string demonstrates `state + magnitude + age`.
- Tag every requirement 🆕 / 🔁 / ✅.
- Emit `HANDOFF` with `SAFETY_CONTRACT` (transition choreography, banner state machine,
  responder flow, widget specs) and `FEED_CONTRACT` (aspect ratios, truncation, card treatment).

── END PROMPT ──

---
---

## Branch 3 completion gate

- [ ] 3A's Kathmandu → Pokhara flow honestly discloses the Supercurvy time penalty
- [ ] 3A's occlusion arithmetic is shown and no state breaches 45 %
- [ ] 3B's offline transition has a debounce that survives a 5-second tunnel
- [ ] 3B's responder side is specified to the same depth as the victim side
- [ ] 3B's 0-peer mesh state is honest and actionable, not hopeless
- [ ] Every safety-critical item has both failure modes stated
- [ ] **Second human reviewer signed off on 3B** — mandatory, not optional
- [ ] Both self-scored ≥ 8/10
