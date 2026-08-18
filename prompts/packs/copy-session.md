# PACK `packs/copy-session`

> Use when writing, translating or auditing UI copy.
> Version: `1.0.0`

## Load stack

1. `root/system`
2. `root/constitution`
3. `root/output-contract`
4. `branches/01-brand` (voice-and-copy, plus critique if reviewing)
5. `branches/11-copy-localisation`
6. `branches/10-sos-safety` **if** the string is on an SOS / crash / mesh surface

## Context docs

- `docs/01-brand-identity.md` §1.1.5 Voice & Tone Matrix
- `docs/02-typography.md` §2.2.3 Devanagari companion scale

## Invocation

```
SESSION
pack:     packs/copy-session
goal:     <one sentence>
surface:  <sitemap id + state>
lang:     en | ne | both
max_en:   <n>
max_np:   <n>
USER TASK
  <the request>
```
