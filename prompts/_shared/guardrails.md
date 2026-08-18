# 🚧 SHARED GUARDRAILS

> Paste this block into **every** node run, immediately before the sub-prompt.
> These are hard constraints, not preferences. Violating any of them invalidates the output.

---

## 🚨 The five inviolable rules

### 1. Red belongs to SOS. Nothing else.

`#FF1F3D` (`sos-500`) appears in exactly one context: emergency.

- Destructive non-emergency actions (delete a trip, remove a rider) use `danger-400 #F2603C`.
- The prayer-flag "Lungta Red" group-member slot is shifted to coral `#FF7A5C`.
- Error states use `danger-400`, never `sos-500`.
- **If your output introduces any second red, it is rejected.**

### 2. No new colours. No new fonts. No new type sizes.

Every colour must resolve to a token in `tokens/ridejaunm.tokens.json`.
Every type style must resolve to the scale in `docs/02`.
If you believe a token is genuinely missing, **do not invent it silently** — emit:

```
⚠️ TOKEN GAP: <what is needed> · <why existing tokens don't cover it> · <proposed name + value>
```

…and continue using the nearest existing token. The gap gets triaged by a human.

### 3. Never colour alone.

Every colour-coded state carries a **second channel**: an icon, a shape, a line pattern, or a
text label. This is required for deuteranopia/protanopia riders *and* for anyone in direct sun.
Route modes are **quad-coded** (hue + pattern + icon + label) — that is not optional.

### 4. Every interactive element states its tap target size.

48 px minimum · 56 px in-ride · 88 px SOS · 72 px PTT.
If a spec describes a control without a size, it is incomplete.

### 5. Every asynchronous or degradable system shows honest state.

GPS accuracy, tile freshness, mesh peer count, battery, sync status, download progress.
**Banned:** fake progress bars, indeterminate "Loading…" with no subject, "Something went
wrong", any state that implies success before it has occurred.
**Required:** state + magnitude + age. `"3 peers · nearest 1.5 km · 4 s ago"`.

---

## ❌ Banned outputs

| Banned | Instead |
|---|---|
| Lorem ipsum | Real Nepali content — `Jomsom`, `Bibek Shrestha`, `जोमसोम`, `2,190 m`, `+977 98…` |
| Generic stock personas ("Sarah, 32, marketing manager") | Nepali riders with real bikes, real routes, real constraints |
| A second red | `danger-400 #F2603C` |
| Font weights below 400 | 400 minimum, 500 minimum in-ride |
| Type below 11 px | 11 px floor; 14 px floor in-ride |
| Pure `#000000` or `#FFFFFF` as a surface | `graphite-900` / `snow-050` |
| Gradient text, gradient borders, mesh/holographic gradients | The 4 sanctioned gradients in `docs/07` §7.1.5 |
| Skeuomorphic leather / carbon fibre / camo | Matte anodised, hard edges |
| Emoji or humour in SOS copy | Absolute, unambiguous, plain |
| Glass applied unconditionally | Glass is **mode-conditional** — disabled in Day-Glare |
| Nested glass (glass inside glass) | Inner elements use `graphite-700` solid |
| Centre-aligned text > 3 words in an in-ride surface | Left-rag, so the eye can reacquire |
| "Users" as a noun in product copy | "Riders" |
| Re-deriving something already in `docs/` | Cite it: `see docs/03 §3.2` |

---

## 🇳🇵 Nepal-reality checks

Any flow, screen or persona that ignores these is not shipping into the real market:

| Reality | Design implication |
|---|---|
| No cellular above ~2,500 m in Mustang, Manang, Dolpa, Humla, much of Karnali | Offline and mesh are the default assumption, not the edge case |
| Monsoon (Asar–Bhadra) closes roads via landslide | Route hazard data expires in 14 days even when tiles are fresh |
| Fuel is scarce; informal bottle/jerrycan sellers are real infrastructure | `fuel_gap_km` is a first-class routing warning |
| Mobile data is metered and expensive | Wi-Fi-first downloads, data-saver feed mode, explicit MB costs |
| Devanagari place names, Bikram Sambat dates, NPR currency | First-class, not an afterthought |
| Timezone is **UTC+05:45** — a 45-minute offset | ETA and SOS timestamp maths must handle it |
| Permit zones: Upper Mustang, Upper Dolpa, Manaslu, Kanchenjunga, Humla | Blocking route advisory, not a footnote |
| Bandhs and festival closures genuinely shut roads | Live advisory layer |
| Group riding is the cultural norm; chiya stops are real waypoints | Squad is a first-class tab; `chiya_stop` is a POI type |
| Common bikes: RE Himalayan/Classic, Pulsar, Xpulse, Duke, CT100 | Garage lists and personas use these, not Ducatis |

---

## 🎯 Scope discipline

- **Answer only the node you were given.** Do not pre-empt sibling or child nodes.
  If node 1A starts specifying screen layouts, it has failed.
- **Depth over breadth.** A specification a designer can build from without asking a question
  beats a broad survey every time.
- **Cite, don't repeat.** If `docs/` already covers a requirement, cite the section and move on.
  Token budget spent restating merged decisions is token budget stolen from the actual delta.
- **Tag every requirement** 🆕 NEW / 🔁 EXTEND / ✅ VALIDATE before answering it.

---

## ⚖️ Escalation triggers — stop and flag, don't guess

Emit a `🛑 ESCALATION` block and continue with your best assumption **clearly labelled** if:

1. A requirement contradicts a `LOCKED_CONSTRAINT` from the root node.
2. A requirement contradicts merged content in `docs/`.
3. Satisfying a requirement would require inventing a token.
4. A safety-critical behaviour (SOS, crash detection, emergency contacts, mesh broadcast) is
   ambiguous. **Never guess on safety.**
5. A requirement depends on a parent node's handoff that wasn't provided.

Format:

```
🛑 ESCALATION
Conflict: <what contradicts what>
Locked source: <file §section>
Options: A) … B) …
Proceeding with: <A or B> — flagged for human decision
```

---

## 🔐 Safety-critical surfaces

These require a second reviewer and an explicit failure-mode note in any PR:

- `Button/SOS` and every SOS state
- Crash detection and its auto-countdown
- Emergency contacts and the medical card
- Mesh broadcast, TTL/relay behaviour, and the transmission ladder
- Anything that could cause a rider to look at the screen for longer than 0.8 s while moving

For these, **explicitly state the failure mode** your design guards against.
"What happens if this fires accidentally?" and "What happens if this fails silently?" must
both have answers in the output.
