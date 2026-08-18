# PACK `packs/designer-session`

> Use when designing or reviewing a screen.
> Version: `1.0.0` · Safety class: `public` (escalate if the surface is SOS)

## Load stack

1. `root/system`
2. `root/constitution`
3. `root/output-contract`
4. `branches/04-components` (atom or molecule leaf)
5. `branches/06-lofi` if structure is not locked
6. `branches/07-hifi` if structure is locked
7. `branches/12-review-qa/ship-gate` before calling it done

## Context docs

- The matching screen in `docs/06-lofi-wireframes.md` or `docs/07-hifi-specifications.md`
- The component section in `docs/04-component-architecture.md`
- `tokens/ridejaunm.tokens.json`

## If the surface is 3.x SOS

Stop. Switch to `packs/safety-review`.

## Invocation

```
SESSION
pack:     packs/designer-session
goal:     <one sentence>
surface:  <LF-/HF- frame or sitemap id>
state:    <default | offline | mesh-only | blackout | …>
constraints:
  - glove-first
  - real Nepali content
USER TASK
  <the request>
```
