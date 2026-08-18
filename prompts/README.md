# RideJaunm Prompt Tree

**Version `1.0.0`** · Status: locked with the Phase 1–8 design-ops set.

Figma holds the pixels. `docs/` holds the decisions. **This tree holds the
instructions that keep both honest.**

A prompt that does not cite a phase document is just a vibe. A phase document
that cannot be turned into a prompt is just an essay. The tree is the bridge.

---

## Why a tree

RideJaunm work is not one prompt. Writing SOS copy is not the same job as
binding a Trip Planner to tokens, and neither is the same job as reviewing a
hi-fi screen in Day-Glare. If you dump the whole design system into every
chat you get two failure modes: the model forgets the safety law, or it
forgets the task.

So the instructions are a tree:

- **Root** — identity and law. Always on.
- **Branches** — one domain at a time.
- **Leaves** — a single job, with a locked output shape.
- **Packs** — pre-composed stacks for the four sessions we actually run.

---

## How to run a session

```
1. Open prompts/TREE.md and pick a pack (or a branch).
2. Paste, in order:
     root/system
     root/constitution
     root/output-contract
     the pack's branches / the one domain branch
3. Attach only the docs/ sections the node lists as context.
4. State the USER TASK in the invocation template.
5. Read the output header before the artefact. If `safety: needs-safety-review`
   and you did not load 10-sos-safety, the session is invalid — restart.
```

Templates live in [`00-root/COMPOSITION.md`](00-root/COMPOSITION.md) and in
each pack file.

### Agents

Load `packs/agent-session` and parse [`catalog.json`](catalog.json). Do not
invent node ids. Do not load more than two branches unless a pack says so.

### Humans in Figma

You do not need the whole tree. A designer reviewing a card loads
`packs/designer-session`. A copywriter loads `packs/copy-session`. Anything
red, loud, or irreversible loads `packs/safety-review` and stops being casual.

---

## Safety class

| Class | Meaning |
|---|---|
| `public` | Default. Constitution still applies. |
| `restricted` | SOS / crash / mesh / emergency contacts. Second reviewer required. Failure-mode block required. |

Restricted nodes will **refuse** single-tap SOS, decorative red, humour on an
armed screen, a map on the Emergency Console, and theming SOS away in Blackout.

---

## What this tree will not do

- It will not invent a new brand direction. Himalayan-Tactical Tech is locked.
- It will not introduce a raw hex. Tokens live in `tokens/ridejaunm.tokens.json`.
- It will not replace rider testing. Prompts specify; riders verdict.
- It will not generate exploit payloads, crash-detection bypasses, or anything
  that makes SOS easier to fire by accident.

---

## Versioning

See `00-root/COMPOSITION.md`. Bump [`VERSION`](VERSION) and `catalog.json`
together. A constitution change is a major, and is itself safety-class.

---

## Map back to the docs

| Branch | Source of law |
|---|---|
| 01-brand | `docs/01-brand-identity.md` |
| 02-typography | `docs/02-typography.md` |
| 03-color | `docs/03-color-system.md` |
| 04-components | `docs/04-component-architecture.md` |
| 05-ia-flows | `docs/05-information-architecture.md` |
| 06-lofi | `docs/06-lofi-wireframes.md` |
| 07-hifi | `docs/07-hifi-specifications.md` |
| 08-figma-ops | `docs/08-figma-setup-roadmap.md` |
| 09-nepal-offline | `docs/09-nepal-offline-data-spec.md` |
| 10-sos-safety | Flow B + `Button/SOS` + Screen 4 + MI-2 + A.2.4 |
| 11 / 12 / 13 | Cross-cutting (voice, DoD, Dev Mode) |
