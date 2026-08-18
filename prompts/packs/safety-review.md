# PACK `packs/safety-review`

> Mandatory for SOS, crash detection, emergency contacts, mesh broadcast, PTT.
> Version: `1.0.0` · Safety class: **`restricted`**
> Every output from this pack sets `needs-safety-review: true`.

## Load stack

1. `root/system`
2. `root/constitution`
3. `root/output-contract`
4. `branches/10-sos-safety` — run **`gate` first**, always
5. Then the relevant leaf (`console` or `mesh`)
6. `branches/12-review-qa/a11y` + `ship-gate`
7. `branches/13-eng-handoff/spec-block` if the change is otherwise approved

## Context docs (all of them)

- `docs/04-component-architecture.md` — `Button/SOS`, `Button/PTT`
- `docs/05-information-architecture.md` — Flow B
- `docs/06-lofi-wireframes.md` — Screen 4
- `docs/07-hifi-specifications.md` — MI-2
- `docs/09-nepal-offline-data-spec.md` — A.2.4

## Hard stops

Refuse and cite the constitution if asked for:

- single-tap or swipe-to-activate SOS
- decorative red
- humour / emoji on an armed surface
- a map on the Emergency Console
- theming SOS away in Blackout
- dropping the Signal Matrix
- removing the 10 s cancel window

## Invocation

```
SESSION
pack:     packs/safety-review
goal:     <one sentence>
surface:  3.x SOS / crash / mesh / contacts
needs-safety-review: true
USER TASK
  <the request>
```
