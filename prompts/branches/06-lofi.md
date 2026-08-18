# BRANCH `06-lofi` — Greyscale screen blueprints

> Parents: `root/*` · Phase: 6 · Safety class: `public` · Version: `1.0.0`
> Context: `docs/06-lofi-wireframes.md`

Lo-fi discipline is load-bearing. If a layout works in lo-fi, colour cannot
save it later; if it fails in lo-fi, colour will only hide the failure.

---

## Leaf `06-lofi/screen`

```
TASK: Produce a zone-by-zone lo-fi blueprint for one screen.

FRAME
Primary:   iPhone 15 Pro — 393 × 852
Secondary: 360 × 800 (Android / Nepali-market majority), 430 × 932 (Pro Max)
Safe:      top 59, bottom 34
Grid:      4-col, 16 px margins, 16 px gutters, 4 px baseline
Thumb:     Easy 0–45 % · Stretch 45–72 % · Hard 72–100 %
Name:      LF-[Screen#]-[Name]-[State]   e.g. LF-01-MapHome-GPSAcquiring

DISCIPLINE
- Greyscale only: #FFFFFF #E5E5E5 #9E9E9E #4A4A4A #1A1A1A
- Boxes with an X for media
- Inter Regular for everything
- No icons (labelled squares)
- No shadows, no radii above 8 px
- The only colour permitted is a magenta annotation layer

CORE SCREENS AND THEIR STATE COUNTS
1 Map Home / HUD     7 states   (docs/06 §6.1)
2 Trip Planner       8 states   (docs/06 §6.2)
3 Community Feed     7 states   (docs/06 §6.3)
4 SOS Console        8 states   (docs/06 §6.4)  → also load 10-sos-safety
Second wave 5–16 as listed in §6.5.

ZONE CONTRACT
For every zone: id, y-range in px, contents, rules, what happens offline.
Map Home reminders:
  Z6 SOS FAB is 88 px, bottom-right, ≥ 24 px from every other control, never scrolls.
  Z7 HUD peek = 120 px and exactly 3 metrics.
  Map visible area ≥ 55 % of viewport in peek (target 62 %).
Trip Planner reminders:
  Route-mode switch is the visual centre of gravity.
  Consequence-first: stats → elevation → hazards → options.
  START RIDE sticky, never below the fold.
Feed reminders:
  Route strip ABOVE the caption. One card ≈ one screen. No competing FAB.
SOS reminders:
  NO MAP. Signal Matrix above the trigger. Trigger centred for either hand.

OUTPUT
1. ASCII or zone table (top → bottom, px heights)
2. State list that must be drawn
3. Magenta annotations (numbered)
4. Thumb / glove / offline notes
5. Longest Nepali string that must not break the layout
```

---

## Leaf `06-lofi/validate`

```
TASK: Run the Phase 6.6 protocol against a lo-fi frame. No colour allowed.

| Test           | Method                                      | Pass                          |
| Blur           | 4 px Gaussian                               | Primary action ID in < 1 s    |
| Thumb          | Easy / Stretch / Hard overlay               | 100 % of in-ride in Easy      |
| Squint         | 50 cm                                       | 3 clear hierarchy tiers       |
| Glove          | 56 px stamps on every in-ride target        | No overlaps, no near-misses   |
| Content stress | Longest real NP (साङ्खुवासभा जिल्ला, खाँदबारी) | No critical truncation        |
| Offline        | Redraw with no network                      | No dead end, no blank screen  |
| 5-second       | Show 5 s, ask "what can you do here?"       | ≥ 4 of 5 testers correct      |

OUTPUT
per-test verdict, evidence, the smallest structural fix (not a colour fix).
Recommended testers: 6–8 Kathmandu riding-group riders, own phone, outdoors,
daylight, gloves.
```
