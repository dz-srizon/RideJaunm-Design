# BRANCH `03-color` — Semantic colour and theming

> Parents: `root/*` · Phase: 3 · Safety class: `public` · Version: `1.0.0`
> Context: `docs/03-color-system.md` · tokens `color`, `contrastFloor`

Never propose a new primary. Never put red anywhere but SOS.

---

## Leaf `03-color/apply`

```
TASK: Bind a surface to the palette.

CHANNELS (locked)
- You / interactive / active tab / primary CTA     → volt-400
- Information / GPS / terrain / links               → cyan-400
- Supercurvy route / badge                          → route-supercurvy (#C25CFF)
- Caution / stale / fuel / weather                  → warning-400
- Success / reachable / downloaded                  → success-400
- Destructive non-emergency                         → danger-400
- Emergency only                                    → sos-500
- Dark foundation                                   → graphite-900 … graphite-050
- Light / Day-Glare foundation                      → snow-050 … snow-900

FOUR MODES
Night (default) · Day-Glare · Dusk · Blackout
Blackout: only speed + next-turn + SOS at full luminance; alerts go amber,
not red, so night vision is preserved. SOS itself stays sos-500 (theme-exempt).

ROUTE QUAD-CODING — do not collapse to hue
Straight     cyan-400   solid 6 px + 2 px casing    ➔
Curvy        volt-400   solid 7 px + glow           ∿
Supercurvy   magenta    animated dash 8 px + glow   ⌇⌇

GROUP MEMBERS — prayer-flag spectrum, in order
1 #3D8BFF Lungta Blue · 2 #E9EFED White · 3 #FF7A5C Coral
4 #2FD07A Green · 5 #FFD028 Yellow · 6 #8A6BFF Newar Indigo
Slot 3 is coral on purpose. Do not "fix" it back to red.

OUTPUT
A binding table:
  element → semantic alias → primitive token → mode overrides
Plus: accent-pixel budget. Accent should cover < 10 % of pixels.
```

---

## Leaf `03-color/audit`

```
TASK: Contrast, colour-blind, glare, and red-quarantine audit.

FLOORS
body ≥ 4.5:1 · telemetry ≥ 7:1 · SOS ≥ 10:1 (surface, not just the glyph)

KNOWN PAIRS (do not re-measure unless the pairing changed)
graphite-050 on graphite-900 = 16.4:1
graphite-200 on graphite-900 = 8.1:1
volt-400 on graphite-900     = 15.8:1
cyan-400 on graphite-900     = 8.9:1
sos-500 on graphite-900      = 6.3:1  → SOS text must be ≥ 17 px / 700
                                      or snow-000 on sos-500 fill (4.6:1 + 2 px sos-600 border)
warning-400 on graphite-900  = 10.4:1
success-400 on graphite-900  = 9.6:1

CHECK
1. Any raw hex in the layer list?
2. Any red that is not sos-* ?
3. Any state coded by colour alone?
4. Day-Glare: strokes +1 px, text +1 weight, glass disabled?
5. Blackout: non-essentials dropped to graphite-400?
6. Deuteranopia / protanopia / tritanopia: Straight vs Curvy vs Supercurvy
   still separable via line pattern + icon?

OUTPUT
verdict: pass | fail
red_quarantine: pass | fail
pairs: <any new pair with estimated ratio and floor>
fixes: <token-level, not "make it brighter">
```
