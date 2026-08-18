# BRANCH `02-typography` — Glanceable type

> Parents: `root/*` · Phase: 2 · Safety class: `public` · Version: `1.0.0`
> Context: `docs/02-typography.md` · tokens `font`, `typography`

---

## Leaf `02-typography/specify`

```
TASK: Specify type for a surface, or produce the missing text styles.

STACK (locked)
- Display / UI:     Space Grotesk 400–700
- Body / long-form: Inter 400–700, tnum on changing numbers
- Devanagari:       Mukta + Noto Sans Devanagari fallback
- Mono:             JetBrains Mono — coordinates, mesh node IDs only
- Overflow only:    Barlow Condensed SemiBold for long NP names in tight HUD chips
- Wordmark only:    Chakra Petch — never UI
- Rejected:         Rajdhani, Orbitron

VIBRATION DOCTRINE
1. No weight below 400.
2. In-ride minimum 500; telemetry 600–700.
3. In-ride minimum size 14 px; anywhere 11 px (labels/legal only).
4. tnum mandatory on runtime-changing values.
5. Tracking widens as size shrinks.
6. Uppercase only for labels ≤ 12 px, ≥ 0.06 em tracking. Never uppercase Devanagari.
7. Feed line length 45–75; cards clamp to 2 lines.
8. Never centre-align > 3 words in-ride.
9. Devanagari +2 px line-height; may drop one size step before truncating.
10. Dynamic Type: HUD caps at 130 %; Feed and Settings scale to 200 %.

SCALE
Use the Phase 2 matrix. Do not invent a new step.
Display/Hero 48 · D1 40 · H1 32 · H2 24 · H3 20 · H4 17
Body/Large 17 · Body/Medium 15 (default) · Body/Small 13
Caption/12 · Caption/12 Caps · Micro/11 · Micro/11 Caps · Legal/10
Telemetry: XXL 64 · XL 44 · Large 32 · Medium 24 · Small 17 · Unit 12 · Label 11

PAIRING
Value + unit are one component. 88 at Telemetry/XXL pairs with KM/H at
Telemetry/Unit, baseline-aligned, 4 px gap, unit at text-tertiary.

OUTPUT
For each string on the surface:
  role:     <style name>
  en/np:    <which companion scale>
  tnum:     yes/no
  truncate: <clamp / drop-size / overflow-face>
  notes:    <vibration-doctrine rule that decided it>
```

---

## Leaf `02-typography/blur-test`

```
TASK: Audit whether type survives 0.6–0.8 s, visor, 60 km/h, vibrating bar.

METHOD
1. List every text layer on the frame with style, size, weight, contrast.
2. Flag any in-ride layer < 14 px or < 500 weight.
3. Flag any changing number without tnum.
4. Flag any centred sentence.
5. Flag any Devanagari with negative tracking or a -caps style.
6. Imagine a 3 px Gaussian (the Phase 2 Blur Test frame). If you cannot
   identify the layer, it fails.

OUTPUT
verdict per layer: pass | fail
failures first, with the exact style substitution that would pass.
```
