# BRANCH `11-copy-localisation` — Bilingual strings

> Parents: `root/*` + usually `01-brand` · Phase: cross-cutting
> Safety class: `public` (SOS copy escalates) · Version: `1.0.0`

Machine translation is a draft, not a ship. Nepali UI is reviewed by a
native copywriter (Phase 8 post-v1 P0). This node produces a reviewable
pair, not a locked translation.

---

## Leaf `11-copy-localisation/string`

```
TASK: Produce or review a bilingual string.

RULES
- Every user-facing string has `en` and `np`.
- Language is switchable without restart.
- Place names render both scripts: Jomsom / जोमसोम.
- Devanagari never ALL CAPS, never negative tracking.
- Devanagari runs ~4 % taller: +2 px line-height, may drop one size step.
- Longest-string test uses real names (साङ्खुवासभा जिल्ला, खाँदबारी).
- Tight HUD chips may use Barlow Condensed SemiBold as an overflow face.
- Bikram Sambat is a display toggle, not a replacement calendar internally.
- Phone validation: +977 9XXXXXXXXX.
- Timezone: Asia/Kathmandu, UTC+05:45 — ETAs and SOS timestamps must not
  silently round to the hour.
- Currency: NPR, रू.
- In-ride: no sentences. SOS: no humour.

OUTPUT
id, en, np, surface, style (EN + NP override), max_en, max_np,
overflow_strategy, tone row, reviewer: human-native-required yes/no
(yes for anything safety-critical or idiomatic).
```

---

## Leaf `11-copy-localisation/glossary`

```
TASK: Add or audit a locked term.

LOCKED TERMS — do not "improve" these
RideJaunm          राइड जाऔं          (the invitation, not a noun)
Let's go. The road knows the way.
                   राइड जाऔं — बाटो हामीलाई थाहा छ।
Never ride alone.  (safety tagline — keep EN on SOS surfaces unless
                   a native reviewer signs off a NP pair)
Straight           सिधा
Curvy              घुमाउरो
Supercurvy         सुपरघुमाउरो        (the product word; do not localise away)
Squad              स्क्वाड / समूह     (Squad in UI chrome, समूह in long copy)
Ride               राइड
Plan               प्लान / योजना
SOS                SOS                 (do not translate the glyph word)
Offline            अफलाइन
Mesh               मेश
Hold 3 seconds     ३ सेकेन्ड थिचिराख्नुहोस्

If you propose a new glossary row, mark it ASSUMED until a native
reviewer accepts it.
```
