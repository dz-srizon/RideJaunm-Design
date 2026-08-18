# BRANCH `04-components` — Atomic / molecular specs

> Parents: `root/*` · Phase: 4 · Safety class: `public` (SOS leaves escalate)
> Version: `1.0.0`
> Context: `docs/04-component-architecture.md`

A component is done when it has (a) all variants, (b) all states, (c) bound
semantic variables, (d) Auto Layout that survives 200 % Dynamic Type, and
(e) a usage / anti-usage note.

If the component is `Button/SOS`, `Button/PTT`, `Widget/SignalMatrix`,
`Widget/MeshTopology` or `Banner/Connectivity` in an SOS-active state,
also load `branches/10-sos-safety`.

---

## Leaf `04-components/atom`

```
TASK: Spec or extend an atom from the Phase 4 catalogue.

NAMING
[Category]/[Component]/[Variant]
e.g. Button/Primary/Large · Control/RouteMode/Supercurvy-Selected

MANDATORY PROPERTIES
state:   default · hover · pressed · focused · disabled · loading
size:    sm 40 · md 48 · lg 56 · xl 64   (SOS is 88, PTT is 72 — not this scale)
theme:   night · day-glare · dusk · blackout
icon-left / icon-right, label, badge as applicable

LAW
- 4 px base. Heights 32 / 40 / 48 / 56 / 64.
- Tap 48 · in-ride 56 · SOS 88 · PTT 72.
- Radii: r-xs 4 · r-sm 8 · r-md 12 · r-lg 16 · r-xl 20 · r-2xl 28 · r-full 999.
- Icon grid 24 / 2 px Phosphor. HUD icons 32 / 2.5 px.
- Text on volt-400 is graphite-900.
- Button/Destructive is danger-400 outline. Filled red is SOS-reserved.

OUTPUT
1. Name and one-line job
2. Property matrix
3. Token bindings per state × theme
4. A11y (role, label, traits, Reduce Motion)
5. Usage + anti-usage
6. Playground list (every variant × state that must be drawn)
7. Build-order slot (Phase 4.3)
```

---

## Leaf `04-components/molecule`

```
TASK: Spec a molecule. Prefer extending these locked ones:

Card/TelemetryHUD     Compact 120 / Expanded 320 / Blackout
                      Compact = exactly 3 primary values. Never more.
                      Priority: Speed → Distance remaining → ETA → Altitude → Fuel.
                      Stale GPS (>5 s) dims to 50 % + STALE pill. No-GPS shows --.

Card/Rider            List 88 / Map-Bubble / Compact-Strip
                      Status: RIDING · STOPPED · OFFLINE · MESH-ONLY · SOS
                      SOS overrides everything and sorts to top.
                      Prayer-flag ring = slot colour.

Card/Feed             Route strip ABOVE the caption. One card ≈ one screen.
                      Bike + monthly-km is the credibility signal.
                      sos-resolved variant is somber, never celebratory.

Overlay/MapControls   Right column only. 56 px glass. 12 px gaps.
                      Left column reserved for the next-turn card.
                      Nothing in the bottom 220 px or the top 100 px.

Control/RouteMode     Signature control. 64 px, 3 equal segments, 320 ms spring.
                      Icon above 11 px caps label. Swipeable. Live stat strip beneath.
                      Compact 40 px icon-only for the HUD.

Also: Sheet/BottomSheet (peek 120 / half 45% / full 92%), Card/RouteSummary,
Card/OfflineRegion, Card/Waypoint, Widget/ElevationProfile,
Widget/CurvatureMeter, Widget/MeshTopology, Widget/SignalMatrix,
Modal/Confirm, Banner/Connectivity, Header/Group, Row/Setting.

OUTPUT
Same contract as 04-components/atom, plus:
- Composition (which atoms it instances)
- Detent / overflow / empty / offline behaviour
- What happens at 200 % Dynamic Type
```

---

## Leaf `04-components/gap`

```
TASK: A designer has drawn a box that is not in the catalogue.

Decide:
1. It IS an existing component — name it, do not fork it.
2. It is a missing variant of an existing component — spec the variant.
3. It is a genuine new atom/molecule — write the spec AND a one-line
   justification for why Phase 4 was incomplete. Do not add a second
   primary button. Do not add a second SOS.

If (3) touches safety, escalate to branches/10-sos-safety.
```
