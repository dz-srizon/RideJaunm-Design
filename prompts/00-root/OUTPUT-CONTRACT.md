# NODE `root/output-contract` — How every node must respond

> Load with `root/system` on every session.
> Version: `1.0.0` · Safety class: `public` · Parents: `root/system`

A prompt is not finished when it sounds good. It is finished when another
designer can transcribe it into Figma, or an engineer can implement it, without
asking a clarifying question.

---

## Required header on every output

```
artefact: <what you produced>
node:     <id of the leaf that ran>
phase:    <1–8 or A or cross-cutting>
tokens:   <list of tokens referenced, or "none">
safety:   <none | touches-sos | needs-safety-review>
open:     <decisions you refused to invent, with the doc that should decide them>
```

## Citation rules

- Colour: token name, never a raw hex, unless you are quoting Phase 3 / the token file.
- Type: Figma style name (`Telemetry/XL`, `Body/Medium`, `Caption/12 Caps`).
- Space / radius / size: token (`space.7`, `radius.2xl`, `size.targetInRide`).
- Motion: duration + easing tokens (`dur-base` + `spring-ui`).
- Screens: sitemap id from Phase 5 (`1.1 Map Home`, `3.3 SOS Active`).
- Components: Phase 4 name (`Button/SOS`, `Control/RouteMode`, `Card/TelemetryHUD`).

## Shape by artefact type

| Artefact | Required sections |
|---|---|
| **Copy** | EN string · NP string · context · max length · tone row from 1.1.5 · never-say |
| **Component spec** | Name · properties · states · tokens · a11y · anti-usage · playground variants |
| **Screen / wireframe** | Frame name (`LF-` / `HF-`) · zones with px heights · states list · thumb-zone notes · offline state |
| **Flow** | Actor · goal · success metric · node-by-node · edge cases table |
| **Review / audit** | Verdict (pass / fail / blocked) · evidence · contrast or test cited · required fix |
| **Handoff** | Behaviour · edge cases · analytics events · API / offline contract · DoD checklist |

## Hard bans in output

- No lorem ipsum. No "John Doe". No Alpine place names used as Nepal stand-ins.
- No `#FF0000`, `#000000`, `#FFFFFF` as app-surface recommendations.
- No new primary accent. No second red.
- No SOS single-tap. No SOS in a scrolling container.
- No "something went wrong". No "Oops".
- No inventing emergency numbers. Use the locked directory (100 / 102 / 101 / 1144 / 103).

## Uncertainty protocol

If the source docs do not decide something:

```
UNDECIDED
what:     <the gap>
blocked:  <what you cannot finish because of it>
decides:  <which phase doc / research / rider test should lock it>
interim:  <the reversible assumption you will use if the user insists, labelled ASSUMED>
```

Never silently promote an assumption to law.
