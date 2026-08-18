# NODE `root/composition` — How to assemble a session

> Version: `1.0.0` · Safety class: `public` · Parents: `root/system`

A **session** is a stack of nodes. Load from the root downward. Never load a
leaf without its ancestors. Never load `branches/10-sos-safety` without also
setting `needs-safety-review`.

---

## Load order

```
1. root/system
2. root/constitution
3. root/output-contract
4. exactly one primary branch (or a pack, which names its branches)
5. optional: a second branch if the task genuinely crosses domains
6. the relevant phase document(s) from docs/ as CONTEXT, not as prompts
7. tokens/ridejaunm.tokens.json when colour, type, space or motion is in play
```

## Packs (pre-composed stacks)

| Pack | Nodes | Use when |
|---|---|---|
| `packs/designer-session` | root ×3 + 04-components + 06-lofi + 07-hifi | Designing or reviewing a screen |
| `packs/copy-session` | root ×3 + 01-brand + 11-copy-localisation | Writing or translating UI copy |
| `packs/safety-review` | root ×3 + 10-sos-safety + 12-review-qa | Any SOS / mesh / crash-detection change |
| `packs/agent-session` | root ×3 + 08-figma-ops + 13-eng-handoff + catalog.json | An autonomous agent doing repo work |

## Context budget

Prefer citing a section over pasting a whole phase doc.

| Task | Minimum context |
|---|---|
| Brand / voice | `docs/01-brand-identity.md` §§1.1.2, 1.1.5, 1.1.6 |
| Type | `docs/02-typography.md` §2.2 + tokens `font` / `typography` |
| Colour | `docs/03-color-system.md` §3.0–3.2 + tokens `color` |
| Component | `docs/04-component-architecture.md` matching section |
| Flow / IA | `docs/05-information-architecture.md` sitemap + the flow |
| Lo-fi screen | `docs/06-lofi-wireframes.md` matching screen + §6.0, §6.6 |
| Hi-fi / motion | `docs/07-hifi-specifications.md` §7.1 + the MI |
| Figma ops | `docs/08-figma-setup-roadmap.md` §8.1 |
| Nepal data | `docs/09-nepal-offline-data-spec.md` matching A.2 subsection |
| SOS | Phase 5 Flow B + Phase 4 `Button/SOS` + Phase 6 Screen 4 + Phase 7 MI-2 |

## Invocation template

Paste this at the top of a new chat, then the node bodies in load order:

```
SESSION
pack:     <pack id or "custom">
goal:     <one sentence>
surface:  <sitemap id or component name>
constraints:
  - <anything extra>
CONTEXT DOCS
  - docs/<file> #<section>
USER TASK
  <the actual request>
```

## Versioning

- This tree is **v1.0.0**. Bump `prompts/VERSION` and `catalog.json` together.
- Patch (`1.0.x`): copy / clarification, no new nodes.
- Minor (`1.x.0`): new leaf or pack, constitution unchanged.
- Major (`x.0.0`): constitution change, or a token/law inversion.

A constitution change is a safety-class event. Treat it like an SOS PR.
