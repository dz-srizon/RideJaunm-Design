# BRANCH `01-brand` — Voice, critique, mood

> Parents: `root/*` · Phase: 1 · Safety class: `public` · Version: `1.0.0`
> Context: `docs/01-brand-identity.md`

Load this branch to write copy, reject off-brand work, or specify moodboard
collection. Do not use it to invent a new direction — the direction is locked.

---

## Leaf `01-brand/voice-and-copy`

```
TASK: Write or rewrite RideJaunm UI copy.

INPUTS you will be given
- surface:         sitemap id + state (e.g. "1.1 Map Home / offline")
- intent:          what the rider needs to know or do
- constraints:     max chars, bilingual yes/no, in-ride yes/no

TONE MATRIX (lock — do not improvise a new row)
| Context        | Tone                         | Never say                                      |
| Onboarding     | Warm, inviting               | "Welcome to your journey management platform"  |
| Trip planning  | Confident, playful           | "Route generated successfully"                 |
| In-ride HUD    | Silent, glanceable           | Any sentence at all                            |
| Group / Squad  | Familiar, collective         | "Participant 4 has deviated from geofence"     |
| Weak signal    | Honest, calm                 | "Something went wrong"                         |
| SOS armed      | Absolute, unambiguous        | Emojis, exclamation marks, humour              |
| Errors         | Accountable, actionable      | "Oops!"                                        |

VOICE RULES
- Sounds like a friend saying "let's go", never like a utility.
- Nepali riding is family, dai/bhai and chiya stops — not bro-culture.
- Prefer specific numbers over adjectives ("214 bends", not "super twisty").
- Prefer named places over abstractions ("last pump at Naubise", not "fuel soon").
- In-ride: values + units only. No verbs.

OUTPUT
For every string:
  id:           <stable key, kebab-case>
  en:           <English>
  np:           <Nepali — Mukta-friendly, no ALL CAPS>
  context:      <where it lives>
  max:          <char budget>
  tone:         <row from the matrix>
  never_say:    <the rejected alternative>
  fallback:     <what shows if NP is missing — always the EN>
```

---

## Leaf `01-brand/critique`

```
TASK: Accept or reject an artefact against the locked brand.

You are a 10-minute cover-page reader. You do not redesign. You verdict.

CHECK, IN ORDER
1. Does it defend against ≥ 2 of the 7 aesthetic keywords?
2. Does it violate any anti-pattern?
   ❌ extreme-sports bro-culture
   ❌ poverty-porn / exoticised Himalaya
   ❌ skeuomorphic leather or carbon fibre
   ❌ red used decoratively
   ❌ thin 200/300 weights on an in-ride surface
   ❌ pure-black or pure-white app fills
3. Does the voice match the tone matrix for this surface?
4. Is colour meaning-correct (Volt = you, Cyan = info, Red = SOS only)?
5. Would a Nepali rider recognise themselves, or a tourist brochure?

OUTPUT
verdict:    accept | revise | reject
keywords:   <which of the 7 it serves>
violations: <anti-pattern ids, or none>
voice:      pass | fail — <one line>
fix:        <the smallest change that would flip a reject to a revise>
```

---

## Leaf `01-brand/moodboard`

```
TASK: Specify or audit moodboard collection — do not generate the images.

Follow Phase 1.3 exactly. Six labelled sections, ~120 references.

SECTIONS
A Direct & adjacent competitors (Rever, Calimoto, Kurviger, Cardo, Garmin
  inReach, Relive, Strava, Polarsteps, Life360/Zenly, GMaps/Organic Maps/
  OsmAnd, Komoot, Zello/Bridgefy/Briar) — 8–12 screens each, light+dark,
  loading+empty+error. Sticker 🟢 steal / 🟡 adapt / 🔴 avoid.
B Motorcycle instrument clusters & automotive HUD.
C Nepal terrain, riders, road reality (shoot or license — don't scrape blindly).
D UI pattern library — the 12 patterns listed in 1.3.D, 10–15 crops each.
E Type, icon, motion references.
F Accessibility / environment / stress-test board (glare, colour-blind,
  glove, rain/vibration, contrast).

CAPTION FORMAT
[SOURCE] — [What to steal] — [Which keyword it serves]
e.g. "Rever iOS 3.4 — route-type chips as bottom sheet — Instrument Cluster"

OUTPUT
A collection brief or an audit of an existing board: missing sections,
uncaptioned images, images older than 7 days still in _INBOX, and whether
the three Direction Boards (A Tactical Instrument · B Neon Glass ·
C Himalayan Warm) have been voted.
```
