# 📦 SHARED CONTEXT PAYLOAD

> **Use only when the model cannot read this repository.**
> If your model has file access (Claude Code, Cursor, an agent), skip this and let the
> `docs/` references resolve directly — this payload is a lossy summary and will always
> be less accurate than the source.

Paste this **first**, before the guardrails and the sub-prompt.

---

```yaml
PROJECT: RideJaunm
TAGLINE: "राइड जाऔं — Let's go. The road knows the way."
WHAT: Mobile app (iOS + Android) for motorcycle riders in Nepal
STATUS: Phases 1–8 design specification COMPLETE and MERGED to main
DESIGN_DIRECTION: "Himalayan-Tactical Tech" — rugged expedition base + one neon accent

CORE_FEATURES:
  - Trip planning with 3 routing modes: Straight / Curvy / Supercurvy
  - Live GPS tracking on a 3D satellite-terrain map
  - Offline map downloads (Nepal-specific data layers)
  - Solo and group trip modes with live squad tracking
  - Community feed for riders (photos, video, shareable routes, chat)
  - Dual-mode SOS: online cellular/GPS + offline Bluetooth mesh / walkie-talkie

POSITIONING: >
  For Nepali motorcycle riders who treat the road as the destination, RideJaunm is the
  expedition companion that plans curvier rides, keeps the squad visible, and keeps calling
  for help after the network stops. Unlike commute navigation and Alpine-tuned route apps,
  it is engineered for Himalayan reality — offline-first, group-native, hostile-terrain-ready.

THE_VOID:
  - Global apps (Rever, Calimoto, Kurviger) tune curvature for the Alps, not for landslide
    season, single-lane exposure, army checkpoints or monsoon impassability
  - Local apps are commute-shaped: A→B, no curvature, no group state, no offline resilience
  - Nobody solves the dead-zone: no cellular above ~2,500 m in Mustang/Manang/Dolpa/Humla/
    Karnali — exactly where riders crash
  - Routes die as Facebook screenshots; nowhere treats a route as an importable object

# ─────────────────────────────────────────────────────────────
# BRAND
# ─────────────────────────────────────────────────────────────
BRAND_PILLARS: [Adventurous, Rugged/Expedition-grade, Reliable/Instrument-honest,
                Community-centric, Locally-rooted-globally-sharp]

AESTHETIC_KEYWORDS: [Himalayan-Tech, Tactical Glass, Hi-Vis Volt, Instrument Cluster,
                     Topographic Brutalism, Prayer-Flag Chromatics, Monsoon Grit]

VOICE:
  onboarding: warm, inviting — "Let's go. Where are we riding?"
  planning: confident, playful — "Supercurvy: 142 bends, 68 km. Worth it."
  in_ride: silent, glanceable — values only, no sentences
  weak_signal: honest, calm — "No cell service. Mesh is holding — 3 riders in range."
  sos: absolute, unambiguous — no emoji, no humour, no exclamation marks

ANTI_PATTERNS: [extreme-sports bro culture, poverty-porn Himalaya tourism imagery,
                skeuomorphic leather/carbon textures, decorative red, thin font weights,
                pure #000000 or #FFFFFF surfaces]

# ─────────────────────────────────────────────────────────────
# COLOUR  (docs/03 · tokens/ridejaunm.tokens.json)
# ─────────────────────────────────────────────────────────────
COLOR:
  volt-400:      "#B4FF39"   # PRIMARY — movement, energy, interactive, "you"
  volt-300:      "#C2FF4D"   # hover
  volt-500:      "#9FE81F"   # pressed
  volt-700:      "#5E930A"   # accent text on light
  cyan-400:      "#22C9EE"   # SECONDARY — info, GPS, terrain, links
  graphite-900:  "#0B0F0E"   # dark app background (elevation 0)
  graphite-850:  "#111716"   # elevation 1
  graphite-800:  "#171F1D"   # elevation 2
  graphite-700:  "#202A27"   # elevation 3
  graphite-600:  "#2C3835"   # strong border
  graphite-500:  "#3C4B47"   # default border
  graphite-400:  "#5A6D68"   # disabled
  graphite-300:  "#7E918C"   # tertiary text
  graphite-200:  "#A6B6B1"   # secondary text
  graphite-050:  "#E9EFED"   # primary text on dark (NOT pure white)
  snow-050:      "#F7F9F8"   # light app background
  snow-600:      "#54615D"   # secondary text on light
  snow-900:      "#0F1513"   # primary text on light
  success-400:   "#2FD07A"
  warning-400:   "#FFB020"
  danger-400:    "#F2603C"   # NON-EMERGENCY destructive
  sos-500:       "#FF1F3D"   # RESERVED — emergency only, never decorative
  sos-400:       "#FF4D64"   # glow
  sos-600:       "#D80D28"   # pressed
  sos-900:       "#3D0209"   # full-bleed emergency wash

ROUTE_MODES:   # QUAD-CODED: hue + pattern + icon + label
  straight:   {color: "#22C9EE", line: "solid 6px + 2px casing",           icon: "arrow-straight", label: STRAIGHT}
  curvy:      {color: "#B4FF39", line: "solid 7px + glow 8px @20%",        icon: "wave-single",    label: CURVY}
  supercurvy: {color: "#C25CFF", line: "animated dash 8px, 12/6 @40px/s",  icon: "wave-double",    label: SUPERCURVY}
  alt:        {color: "#5A6D68", line: "solid 5px @60%"}
  detour:     {color: "#FFB020", line: "dashed 6px"}

PEER_SPECTRUM:  # prayer-flag, deuteranopia-verified, for group members
  [ "#3D8BFF", "#E9EFED", "#FF7A5C", "#2FD07A", "#FFD028", "#8A6BFF" ]
  # note: slot 3 is CORAL, not red — true red is SOS-reserved

THEME_MODES: [night (default), day-glare, dusk, blackout]
CONTRAST_FLOORS: {body: 4.5, telemetry: 7.0, sos: 10.0}

# ─────────────────────────────────────────────────────────────
# TYPE  (docs/02)
# ─────────────────────────────────────────────────────────────
FONTS:
  display: "Space Grotesk 400–700"
  body:    "Inter 400–700, tabular-nums mandatory on changing values"
  nepali:  "Mukta (+ Noto Sans Devanagari fallback)"
  mono:    "JetBrains Mono"

TYPE_SCALE:   # token: size/line-height/weight/tracking
  display-hero:  "48/52/700/-0.02em"
  display-1:     "40/46/700/-0.02em"
  h1:            "32/38/700/-0.015em"
  h2:            "24/30/600/-0.01em"
  h3:            "20/26/600/-0.005em"
  h4:            "17/24/600/0"
  body-lg:       "17/26/400/0"
  body-md:       "15/22/400/0"
  body-sm:       "13/20/400/+0.005em"
  caption:       "12/16/500/+0.02em"
  caption-caps:  "12/16/600/+0.08em UPPER"
  micro:         "11/14/500/+0.03em"
  micro-caps:    "11/14/700/+0.10em UPPER"
  legal:         "10/14/400/+0.02em"

TELEMETRY_SCALE:   # all tabular-nums
  tel-xxl:   "Space Grotesk 700 64/64/-0.03em"
  tel-xl:    "Space Grotesk 700 44/46/-0.025em"
  tel-lg:    "Space Grotesk 700 32/34/-0.02em"
  tel-md:    "Space Grotesk 600 24/28/-0.01em"
  tel-sm:    "Inter 600 17/22/0"
  tel-unit:  "Inter 700 12/14/+0.10em UPPER"
  tel-label: "Inter 600 11/14/+0.10em UPPER"

TYPE_RULES: [no weight <400 anywhere, ≥500 in-ride, ≥14px in-ride, 11px absolute floor,
             tabular-nums on all runtime values, Devanagari +2–6px line-height,
             never centre-align >3 words in-ride]

# ─────────────────────────────────────────────────────────────
# SPATIAL & MOTION
# ─────────────────────────────────────────────────────────────
SPACING: [2,4,8,12,16,20,24,32,40,48,64,80,96]   # px, 4px base grid
RADIUS:  {xs:4, sm:8, md:12, lg:16, xl:20, "2xl":28, full:999}
TARGETS: {min:48, in_ride:56, sos:88, ptt:72}
ICONS:   "Phosphor, 24px grid, 2px stroke (32px/2.5px for HUD)"

ELEVATION:
  L0: "map (no shadow)"
  L1: "graphite-850 + 0 1px 2px rgba(0,0,0,.40)"
  L2: "glass-map: rgba(11,15,14,.62) + blur 24 + 1px rgba(255,255,255,.08)"
  L3: "graphite-800 @96% + blur 32 + 0 -12px 40px rgba(0,0,0,.55)"
  L4: "graphite-700 + 0 8px 24px rgba(0,0,0,.50)"
  L5: "graphite-800 + scrim rgba(5,8,7,.72)"
  L6: "sos-900 full-bleed + glow-sos + 2px pulsing sos-500 border"

GLASS_RULES: [blur ≥20px, fill ≥55%, always 1px top/left inner border, never nest,
              never behind body paragraphs, DISABLED in day-glare (solid + border instead),
              auto-strengthen to blur 32 / fill 80% when map luminance >0.45]

MOTION: {instant:100ms, fast:200ms, base:320ms, slow:600ms, cinematic:1200ms}
EASING: {standard: "cubic-bezier(.4,0,.2,1)", outExpo: "cubic-bezier(.16,1,.3,1)"}
SPRINGS: {ui: "stiffness 260 damping 24", map: "stiffness 180 damping 22"}

# ─────────────────────────────────────────────────────────────
# IA  (docs/05)
# ─────────────────────────────────────────────────────────────
BOTTOM_NAV: [Ride 🗺, Plan 🧭, SOS ⬤ (centre, raised, sos-ringed), Squad 👥, Profile 👤]
NAV_RULES: [64px + safe area, glass over map, auto-hides above 15 km/h for 10s,
            SOS never hides — collapses to an 88px FAB, SOS never badged]
FEED_LOCATION: "One layer deep — top-bar entry on Ride + default Squad sub-tab.
                Deliberately NOT a primary tab (no scroll-browsing at 60 km/h)."

# ─────────────────────────────────────────────────────────────
# SOS SUBSYSTEM  (docs/05 Flow B, docs/06 §6.4)
# ─────────────────────────────────────────────────────────────
SOS:
  ladder: "cellular/GPS → BLE mesh (TTL 6 hops) → walkie-talkie PTT → breadcrumb + queued SMS"
  arming: "3s long-press (haptic ticks at 0/1/2s, heavy at 3s) → 10s cancel window → active"
  stand_down: "3s long-press, never a single tap"
  disclosure: "plain language default, technical diagnostics one tap away"
  broadcast_interval: "30s (stretches to 120s below 15% battery)"
  crash_detection: ">4g impact + 12s no motion → 30s auto-countdown, siren + max brightness"
  isolation: "separate Figma file, restricted permissions, separate sign-off gate"
  signal_matrix_rows: [Cellular, GPS, Mesh, Satellite]   # each: state + magnitude + age

# ─────────────────────────────────────────────────────────────
# NEPAL REALITY  (docs/09)
# ─────────────────────────────────────────────────────────────
NEPAL:
  timezone: "Asia/Kathmandu UTC+05:45 — the 45-minute offset must be handled"
  calendar: "Gregorian + Bikram Sambat toggle"
  currency: "NPR रू"
  scripts: "Latin + Devanagari, place names render both (Jomsom / जोमसोम)"
  dead_zones: "Mustang, Manang, Dolpa, Humla, much of Karnali above ~2,500m"
  permit_zones: [Upper Mustang, Upper Dolpa, Manaslu, Kanchenjunga, Humla]
  hazards: [monsoon landslide (Asar–Bhadra), winter pass closure, bandhs, festival closures]
  fuel: "NOC + private + informal bottle/jerrycan sellers; fuel_gap_km is a routing warning"
  bikes: [RE Himalayan 450, Classic 350, Hunter 350, Pulsar NS200, Xpulse 200,
          KTM Duke/Adventure, Honda CB/XL, TVS Apache, Bajaj CT100]
  roads: [Prithvi Highway, BP Highway, Tribhuvan Rajpath, Araniko, Mugling–Narayanghat,
          Beni–Jomsom, Karnali Highway, Nagarkot, Ilam tea hills]
  emergency: {police: 100, ambulance: 102, fire: 101, tourist_police: 1144, traffic: 103}
  data_cost: "mobile data is metered and expensive — Wi-Fi-first everything"

# ─────────────────────────────────────────────────────────────
# SOURCE OF TRUTH (conflict resolution order)
# ─────────────────────────────────────────────────────────────
AUTHORITY:
  1: "tokens/ridejaunm.tokens.json"
  2: "docs/01–09 (merged)"
  3: "prompts/ (this tree)"
  4: "LLM output (draft until reviewed)"
```

---

## Repository map (for models that can read files)

```
docs/01-brand-identity.md            Brand pillars, void, voice, keywords, moodboard spec
docs/02-typography.md                Fonts, 17-step scale, telemetry scale, Devanagari
docs/03-color-system.md              Palette, 4 theme modes, route colours, contrast audit
docs/04-component-architecture.md    ~26 atoms + ~18 molecules, SOS button, RouteMode switch
docs/05-information-architecture.md  5-tab nav, sitemap, Flow A (group+supercurvy), Flow B (SOS)
docs/06-lofi-wireframes.md           Zone blueprints for 4 core + 12 second-wave screens
docs/07-hifi-specifications.md       Elevation, glass, radii, 3 micro-interactions, motion
docs/08-figma-setup-roadmap.md       Figma structure, conventions, 10-week gated roadmap
docs/09-nepal-offline-data-spec.md   ~90 Nepal offline map data fields
tokens/ridejaunm.tokens.json         W3C DTCG tokens — highest authority
prompts/                             This tree
```
