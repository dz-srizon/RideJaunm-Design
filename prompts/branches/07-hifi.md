# BRANCH `07-hifi` — Visual styling and micro-interactions

> Parents: `root/*` · Phase: 7 · Safety class: `public` (MI-2 escalates)
> Version: `1.0.0`
> Context: `docs/07-hifi-specifications.md`

---

## Leaf `07-hifi/transition`

```
TASK: Take a locked lo-fi frame to hi-fi. Follow the 8-step protocol IN ORDER.

1. Duplicate & lock     Lo-fi stays frozen as the structural contract.
2. Apply tokens only    Greys → graphite/snow semantics. Text styles. NO effects.
3. Instance components  Every box becomes a library instance. Leftover boxes
                        are Phase 4 gaps — stop and spec them.
4. Real content         Real Nepali names, riders, distances, photography.
                        Never lorem. Never stock white-guy-on-a-Ducati.
5. Depth pass           L0–L6, screen-wide, one system at a time.
6. Accent pass          Volt/cyan/magenta last, sparingly. < 10 % of pixels.
7. Map integration      Real MapLibre/Mapbox renders behind glass.
8. Stress & audit       Contrast, glare, colour-blind, Dynamic Type,
                        longest-string, empty/error/offline.

ELEVATION = "how far above the map is this?"
L0 World (the map) · L1 Ground chrome · L2 Floating glass · L3 Sheets & bars ·
L4 Popovers · L5 Modals · L6 Emergency (overrides everything).
Shadows are pure black at varying alpha. Never coloured shadows over satellite.
Glow is a separate effect, only on interactive accents.

GLASS RULES
Blur ≥ 20 px. Fill ≥ 55 %. Adaptive: mean luminance > 0.45 → glass-map-strong.
Always a 1 px rgba(255,255,255,.08) inner border. Never nest glass.
Never set body paragraphs on glass. Day-Glare: glass OFF, solid snow-000 @ 96 %.
Low-tier fallback: graphite-850 @ 94 % solid.

RADIUS = FUNCTION
r-xs precision · r-sm utility · r-md standard interactive · r-lg content ·
r-xl floating over the world · r-2xl major surfaces · r-full human/live/urgent.
Nesting law: inner = outer − padding.

GRADIENTS — four sanctioned uses only
Scrims · CTA fills (≤ 12 % luminance delta) · data encoding (grad-altitude) ·
emergency (grad-sos radial). Banned: gradient text, borders, mesh, cards, nav.

OUTPUT
HF frame name, mode set (Night / Day-Glare / Dusk / Blackout), token binding
notes, glass-legibility backgrounds used (snow / forest / desert), ship-gate
checklist from 7.1.8 ticked or failed.
```

---

## Leaf `07-hifi/micro-interaction`

```
TASK: Spec a micro-interaction. Prefer the locked three.

MI-1 GPS Lock Ripple — the "the app is alive" moment
Searching (radar sweep 2 s, accuracy breathes) → Narrowing (spring contract,
tnum accuracy ticks) → Lock (volt ripple 600 ms, hollow ring → volt chevron,
medium haptic) → Idle breathe 2 s → Degraded (warning-400, 1 s pulse).
Never silently pretend. Reduce Motion: static ring + text accuracy.

MI-2 SOS Commitment Ring — SAFETY. Also load 10-sos-safety.
0 ms scale 0.94 + vignette · 0–3000 ms conic fill, 3·2·1 in Telemetry/XL,
haptic light/medium/medium/HEAVY, rising 3-tone · 3000 ms white flash +
takeover · early release 240 ms unwind + "SOS cancelled".
Exempt from Reduce Motion. VoiceOver counts down. Dexterity setting:
1.5 s hold / 20 s cancel. Never a single tap. Never a swipe.

MI-3 Offline Tile Materialisation
Queued dashed cyan rect → tiles fade grey-wireframe → full colour, spiral
from route centre, tnum "142 / 380 tiles · 84 / 214 MB · 2 min left" →
complete volt sweep + check + success haptic → failed tiles stay amber
wireframe, never silently "done" → stale > 30 d desaturates.

SECONDARY (after the three): MI-4 Route Mode morph · MI-5 Map zoom physics ·
MI-6 Squad arrival pulse · MI-7 Post-ride stat reveal.

MOTION TOKENS
dur-instant 100 · fast 200 · base 320 · slow 600 · cinematic 1200
ease-standard · ease-out-expo · ease-in-out-back
spring-ui 260/24 · spring-map 180/22
Reduce Motion: opacity cross-fades at dur-fast, EXCEPT MI-2 ring and GPS
accuracy readout.

OUTPUT
Trigger, stage table (time / visual / haptic / audio), Reduce Motion
behaviour, implementation hint (Rive preferred), why it exists in one line.
```
