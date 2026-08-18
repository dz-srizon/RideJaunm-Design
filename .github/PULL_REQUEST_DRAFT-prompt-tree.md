# Pull request draft — Prompt Tree artefact v1.0.0

Use the marked block as the `gh pr create` body.

# ▼ PR BODY

## feat(prompts): RideJaunm Prompt Tree artefact v1.0.0

Turns the locked Phase 1–8 decisions into a **loadable instruction tree**, so a designer, copywriter, reviewer or agent can produce on-brand work without re-deriving the law.

> Figma holds the pixels. `docs/` holds the decisions. `prompts/` holds the instructions that keep both honest.

This is not a pile of chat starters. It is a versioned artefact with a constitution, an output contract, a machine-readable catalogue, and a restricted SOS branch that will refuse illegal requests.

---

### 📦 What's in this PR

| Path | Role |
|---|---|
| `prompts/README.md` | How to run a session |
| `prompts/TREE.md` | Visual index of the whole tree |
| `prompts/catalog.json` | Machine-readable node catalogue (`v1.0.0`, 4 root / 13 branches / 31 leaves / 4 packs) |
| `prompts/00-root/SYSTEM.md` | Identity, product, working method — always first |
| `prompts/00-root/CONSTITUTION.md` | The 10 visual laws + token / type / content / safety law |
| `prompts/00-root/OUTPUT-CONTRACT.md` | Required header, citation rules, artefact shapes |
| `prompts/00-root/COMPOSITION.md` | Load order, packs, context budget, semver |
| `prompts/branches/01-brand.md` … `13-eng-handoff.md` | One branch per domain, leaves are single jobs |
| `prompts/branches/10-sos-safety.md` | **Restricted.** Gate → console → mesh. Failure-mode block required. |
| `prompts/packs/*` | Pre-composed stacks: designer · copy · safety-review · agent |
| `README.md` | Structure + contributing updated to point at the tree |

---

### How a session is supposed to run

```
root/system → root/constitution → root/output-contract
        → exactly one primary branch (or a pack)
        → docs/* as CONTEXT, not as prompts
```

Four packs cover the sessions we actually run:

| Pack | When |
|---|---|
| `packs/designer-session` | Designing or reviewing a screen |
| `packs/copy-session` | Writing or translating UI copy |
| `packs/safety-review` | Anything SOS / crash / mesh / contacts |
| `packs/agent-session` | Autonomous repo work |

---

### 🚨 Safety isolation

`branches/10-sos-safety` is safety-class `restricted`. Loading it (or Flow B, Screen 4, MI-2, or `Button/SOS`) sets `needs-safety-review: true` and requires a written `failure_mode` block.

The branch will **refuse**:

- single-tap or swipe-to-activate SOS
- decorative red
- humour / emoji on an armed surface
- a map on the Emergency Console
- theming SOS away in Blackout
- dropping the Signal Matrix
- removing the 10-second cancel window

That is the same isolation the Figma file already has, expressed as instructions.

---

### Why this is v1.0.0 and not a draft

- Every leaf cites a locked phase document or token. Nothing invents a new direction, a new primary, or a new emergency number.
- Output is transcribable: token names, Figma style names, sitemap ids, component names.
- Uncertainty is an `UNDECIDED` block, not a pretty guess.
- Semver is defined: constitution changes are majors and are themselves safety-class.

---

### ✅ Verification

- `prompts/catalog.json` parses as JSON
- Catalogue counts match the files on disk: 4 root, 13 branches, 4 packs
- Internal links from `README.md` → `prompts/` resolve
- No application code paths touched
- No raw hex introduced as new law — existing tokens are quoted, not extended

### 👀 Suggested review order

1. `prompts/00-root/CONSTITUTION.md` — the law the rest of the tree cannot override
2. `prompts/branches/10-sos-safety.md` — the restricted gate
3. `prompts/catalog.json` + `prompts/TREE.md` — the map
4. `prompts/packs/agent-session.md` — how an agent is supposed to compose this

# ▲ END PR BODY
