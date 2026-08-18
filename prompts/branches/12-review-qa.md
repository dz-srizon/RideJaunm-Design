# BRANCH `12-review-qa` — Ship gates and design QA

> Parents: `root/*` · Phase: cross-cutting · Safety class: `public`
> Version: `1.0.0`
> Context: README Definition of Done · docs/08 §8.3 · docs/06 §6.6 · docs/07 §7.1.8

This branch only verdicts. It does not redesign. A fail names the smallest fix.

---

## Leaf `12-review-qa/ship-gate`

```
TASK: Decide whether a screen is done.

A screen is done when ALL of the following are true:

1. Built entirely from published component instances — zero detached layers.
2. All colours bound to semantic variables; all text uses published styles.
3. Renders correctly in Night, Day-Glare, Dusk and Blackout.
4. Loading, empty, error and OFFLINE states designed.
5. Contrast: body ≥ 4.5:1, telemetry ≥ 7:1, SOS ≥ 10:1.
6. Colour-blind simulation passed (deuteran / protan / tritan).
7. Glare test (40 % brightness) and 4 px blur test passed.
8. In-ride targets ≥ 56 px, glove-overlay verified.
9. Real Nepali content, longest-string tested, Devanagari verified.
10. Annotated with behaviour, edge cases and analytics events;
    status stamp = Ready for Dev, with owner and date.

If the screen is SOS / crash / mesh / contacts: also run 10-sos-safety/gate
and require a second reviewer.

OUTPUT
verdict: ready-for-dev | revise | blocked
score:   n/10
fails:   <numbered, with evidence>
safety:  none | needs-safety-review
```

---

## Leaf `12-review-qa/a11y`

```
TASK: Accessibility-only audit.

MUST
- Never colour alone
- Contrast floors (body / telemetry / SOS)
- 48 / 56 / 88 / 72 targets
- VoiceOver labels, especially Button/SOS and Control/RouteMode (radiogroup)
- Dynamic Type: HUD 130 % cap, Feed/Settings 200 %, Auto Layout survives
- Reduce Motion policy honoured (exceptions: SOS ring, GPS accuracy)
- Devanagari metrics
- Colour-blind: Straight / Curvy / Supercurvy still separable
- Glare: Day-Glare mode actually used (glass off, +1 stroke, +1 weight)
- SOS: either-hand reach, one-handed on the ground, works with gloves,
  works with a cracked screen conceptually (huge targets, audio+haptic)

OUTPUT
per-item pass/fail, with the token or property that would fix it.
```

---

## Leaf `12-review-qa/design-qa-build`

```
TASK: Review an implemented build against the spec (Phase 8 post-v1 P0).

WHERE
On a real device, outdoors, in daylight, wearing gloves, on the tester's
own phone. Optionally mounted on a stationary bike, engine running.

LOOK FOR
- Token drift (a raw hex landed in code)
- Detached or one-off components
- Offline path that dumps to a system error
- SOS timing that is not 3.0 s / 10 s
- UTC+05:45 rounding errors in ETA or SOS timestamps
- Missing OSM attribution
- Feed inviting interaction above 15 km/h (nav should have hidden)
- Red anywhere that is not SOS

OUTPUT
bug list tagged P0 (safety / data-loss / illegal red) · P1 (spec miss) ·
P2 (polish). P0 cannot ship.
```
