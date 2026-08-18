# BRANCH `13-eng-handoff` — Dev Mode annotations

> Parents: `root/*` · Phase: 8 / handoff · Safety class: `public`
> Version: `1.0.0`
> Context: docs/08 §8.1.3 Dev Mode · tokens/ridejaunm.tokens.json

An engineer should be able to build the surface without asking a question.

---

## Leaf `13-eng-handoff/spec-block`

```
TASK: Write the Doc/Spec annotation that sits on an approved frame.

SECTIONS (all required)
1. Behaviour        what it does, in rider language then in system language
2. States           default / loading / empty / error / offline / mesh-only /
                    SOS-active. One line each.
3. Tokens           semantic aliases used (not primitives, not hex)
4. Motion           duration + easing tokens; Reduce Motion fallback
5. Interaction      targets, holds, swipes, haptics, audio
6. Offline          what still works, what queues, what hides
7. Analytics        event names, properties, when they fire
                    (never fire analytics that require a network as a
                    precondition of the safety path)
8. API / data       endpoints or local stores, including Appendix A fields
9. Edge cases       the table, not a paragraph
10. DoD             the 10-point ship gate, pre-ticked by design

NAMING PARITY
Variable names in Figma must match code token names
(color/action/primary, not "Volt 400"). If they don't, list the mapping
and flag it as debt — do not silently translate.

OUTPUT
A spec block ready to paste into Figma Dev Mode and, if useful, a
markdown twin for the PR.
```

---

## Leaf `13-eng-handoff/token-export`

```
TASK: Reason about tokens/ridejaunm.tokens.json.

The file is W3C DTCG. Tokens Studio consumes it. Do not rewrite the
schema. Do not add a primitive that Phase 3 did not name.

When proposing a token change:
- say whether it is a primitive, an alias, or a mode override
- say which components will shift
- say whether contrast floors still hold
- if it touches sos.* or duration.sosHold / sosCancel, escalate to
  10-sos-safety

Never commit generated output under tokens/build/ (gitignored).
```
