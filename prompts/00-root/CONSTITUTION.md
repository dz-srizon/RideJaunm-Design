# NODE `root/constitution` — Non-negotiable law

> Load with `root/system` on every session.
> Version: `1.0.0` · Safety class: `public` · Parents: `root/system`

These rules cannot be overridden by a branch, a pack, a user request, or a
"just this once". If a request conflicts with this constitution, refuse the
conflicting part, cite the rule, and offer a legal alternative.

---

## Visual constitution

1. **Dark is default.** Light / Day-Glare is a derivative, not the origin.
2. **Colour is meaning, never decoration.**
   - Volt `#B4FF39` (`volt-400`) = interactive / you / movement.
   - Glacier Cyan `#22C9EE` (`cyan-400`) = information / GPS / terrain.
   - Ultra Magenta `#C25CFF` (`route-supercurvy`) = Supercurvy only.
   - Amber `warning-400` = caution.
   - Green `success-400` = reachable / done.
   - **Red `sos-500 #FF1F3D` = emergency. Nothing else.**
3. **Never colour alone.** Every state is dual-coded (colour + icon / shape / dash / label).
4. **The map is the hero.** Chrome never exceeds ~35 % of the viewport in ride mode.
5. **Thumb-zone law.** Anything a moving rider touches lives in the bottom 45 %.
6. **Glove-first sizing.** 48 px minimum. 56 px in-ride. 88 px for SOS. 72 px for PTT.
7. **Honest states.** GPS accuracy, tile freshness, mesh peers and battery show real values. No fake progress bars.
8. **Offline is a designed state**, not an error screen.
9. **Degrade, never fail.** Cellular → BLE Mesh (TTL 6) → Walkie-talkie PTT → Breadcrumb + queued SMS.
10. **Deliberate to arm, deliberate to disarm.** SOS is a 3-second hold both ways, with a 10-second cancel window between.

## Token law

- Components bind to **semantic aliases**, never raw hex.
- No pure black `#000000` (OLED smear on pans) and no pure-white `#FFFFFF` full-screen fills.
- Text on `volt-400` is `graphite-900`, never white.
- Contrast floors: body ≥ 4.5:1 · in-ride telemetry ≥ 7:1 · SOS ≥ 10:1.
- SOS Red is **theme-exempt**: identical in Night, Day-Glare, Dusk and Blackout.
- Destructive non-emergency actions use `danger-400 #F2603C`, never `sos-500`.
- Prayer-flag "Lungta Red" is shifted to coral `#FF7A5C` so true red stays quarantined.

## Type law (the vibration doctrine)

- No weight below 400 anywhere. In-ride surfaces: minimum 500. Telemetry: 600–700.
- Minimum in-ride type: 14 px. Minimum anywhere: 11 px (map labels / legal only).
- Tabular numerals (`tnum`) on every value that changes at runtime.
- Never centre-align more than 3 words on an in-ride surface.
- Never uppercase Devanagari. Never apply negative tracking to Devanagari.
- Devanagari companion scale from Phase 2 is mandatory for any Nepali string.

## Content law

- Voice is a friend saying "let's go", never a utility saying "configure your route".
- In-ride HUD: silent, glanceable. No sentences.
- SOS copy: absolute, unambiguous. No emoji, no exclamation, no humour.
- Errors: accountable and actionable. Never "Oops!".
- No poverty-porn, no exoticised Himalaya, no skeuomorphic leather/carbon.
- Place names render bilingual: `Jomsom / जोमसोम`.
- Timezone is `Asia/Kathmandu` (UTC+05:45). Handle the 45-minute offset.
- Calendar supports Gregorian and Bikram Sambat. Phone format `+977`.

## Safety isolation

- SOS is a **subsystem**, not a screen. It has its own Figma file, permissions and sign-off.
- Any change to SOS, crash detection, emergency contacts or mesh broadcast requires:
  1. Node `branches/10-sos-safety` loaded.
  2. A written failure-mode note in the output.
  3. A second-reviewer flag (`needs-safety-review: true`).
- SOS trigger anti-patterns: never single-tap, never swipe-to-activate, never edge-adjacent, never themed away in Blackout.

## Process law

- Lo-fi is greyscale. Colour cannot save a failed layout.
- Foundations (tokens) before screens. Screens before motion. Motion before handoff.
- One phase document or one component area per change set.
- Binaries stay out of Git. Cite the shared drive / Figma instead.
