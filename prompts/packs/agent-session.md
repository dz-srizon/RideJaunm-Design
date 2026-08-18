# PACK `packs/agent-session`

> Use when an autonomous agent is doing repository work: new specs, audits,
> token-aware edits, PR descriptions, or Figma-ops checklists.
> Version: `1.0.0`

## Load stack

1. `root/system`
2. `root/constitution`
3. `root/output-contract`
4. `root/composition`
5. `prompts/catalog.json` (so you can name nodes correctly)
6. `branches/08-figma-ops` for workspace / roadmap questions
7. `branches/13-eng-handoff` for anything an engineer will consume
8. The **one** domain branch that matches the task
9. `packs/safety-review` instead of (8) if the task is safety-class

## Working rules for agents

- Transcribe from `docs/` and `tokens/`. Do not invent law.
- Never introduce a raw hex into a spec.
- Never commit `.fig`, photography, map tiles, or `tokens/build/`.
- One phase document or one component area per change set.
- If you cannot find a decision, emit an `UNDECIDED` block.
- If you touch SOS, you stop and reload this session as `packs/safety-review`.

## Invocation

```
SESSION
pack:     packs/agent-session
goal:     <one sentence>
branch:   <the one domain branch>
USER TASK
  <the request>
```
