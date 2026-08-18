# 📋 SHARED OUTPUT CONTRACT

> Paste this at the **end** of every node run, after the sub-prompt.
> It defines the response shape, the self-check gate, and the scoring rubric.

---

## Required response structure

Return **exactly** these sections, in this order:

````markdown
## 0 · Scope Acknowledgement
- Node: <e.g. 1A>  Mode: <GENERATE | EXTEND | AUDIT>
- Inherited: <which handoffs you received>
- Requirement tags: R1 🆕 / R2 🔁 / R3 ✅ / R4 🆕   ← tag each before answering
- Already covered (citing, not regenerating): <docs/§ references>
- Escalations: <none | list>

## 1 · <Requirement 1 title>
<the actual work — tables, specs, values, layouts>

## 2 · <Requirement 2 title>
…

## N · Rationale & Tradeoffs
For every non-obvious decision: what you chose, what you rejected, and **why**.
Minimum 3 entries. "It looks better" is not a rationale.

## ⚠️ Risks, Gaps & Open Questions
Anything you were unsure about, anything that needs a human, anything you assumed.
If this section is empty on a substantial output, you were not being honest.

## ✅ Self-Check
<the gate below, answered line by line>

## 📤 HANDOFF
```yaml
<the machine-readable block this node's children inherit>
```
````

---

## Formatting rules

| Rule | Detail |
|---|---|
| **Tables over prose** for any spec with ≥ 3 attributes | Sizes, states, tokens, fields |
| **Exact values, never ranges** | `56 px`, not "around 56". `#B4FF39`, not "bright green" |
| **Every colour as `token-name #HEX`** | `volt-400 #B4FF39` — both, always |
| **Every dimension in px**, every duration in ms | No rem, no %, no "fast" |
| **ASCII layout diagrams** for any screen or component geometry | With pixel heights annotated per zone |
| **Cite merged docs** as `docs/03 §3.2` | Don't restate them |
| **Real Nepali content** in every example | Never lorem, never "John Doe" |
| **No preamble, no "Certainly!"** | Start at `## 0 · Scope Acknowledgement` |
| **No closing summary** unless it adds information | The handoff block is the ending |

---

## ✅ Self-Check Gate

Answer **every line** with `PASS`, `FAIL` or `N/A — <reason>`. A `FAIL` requires either a fix
before returning, or an entry in *Risks, Gaps & Open Questions*.

```
SC-01  Every colour resolves to tokens/ridejaunm.tokens.json (no invented hex)
SC-02  No second red introduced; #FF1F3D used only for emergency
SC-03  Every colour-coded state has a second channel (icon/shape/pattern/label)
SC-04  Every interactive element states its tap target size (≥48 / ≥56 in-ride / 88 SOS)
SC-05  Every type reference resolves to the docs/02 scale; no weight <400; no size <11px
SC-06  Contrast floors respected and stated (body 4.5:1 / telemetry 7:1 / SOS 10:1)
SC-07  Offline behaviour specified for every screen/component/flow touched
SC-08  All four theme modes considered (night / day-glare / dusk / blackout)
SC-09  Real Nepali content used throughout; Devanagari where place names appear
SC-10  Glass treated as mode-conditional, never applied in day-glare
SC-11  Requirements tagged 🆕/🔁/✅; nothing already merged was regenerated
SC-12  Safety-critical items state their failure mode (accidental fire + silent failure)
SC-13  No scope creep into sibling or child nodes
SC-14  HANDOFF block present, valid YAML, and complete
```

---

## 📊 Scoring rubric — score your own output /10

| # | Criterion | Weight | What full marks looks like |
|---|---|---|---|
| 1 | **Buildability** | ×2 | A designer opens this and builds it in Figma without asking a single question |
| 2 | **Constraint fidelity** | ×2 | Zero violations of locked constraints or guardrails |
| 3 | **Delta focus** | ×1 | Effort went to 🆕/🔁 items; ✅ items were cited, not rewritten |
| 4 | **Nepal specificity** | ×1 | Content and decisions are unmistakably for this market, not generic |
| 5 | **Honest states** | ×1 | Offline / error / degraded / empty are designed, not assumed away |
| 6 | **Rationale quality** | ×1 | Rejected alternatives named with reasons |
| 7 | **Precision** | ×1 | Exact values everywhere; no hedging language |
| 8 | **Handoff integrity** | ×1 | A child node can run correctly from this block alone |

**Score = Σ(criterion × weight) / 10**

| Score | Action |
|---|---|
| **9–10** | Ship. Merge to `docs/`. |
| **8** | Ship with the flagged gaps logged as issues. |
| **< 8** | **Re-run.** Quote the failed criteria back to the model. Do not hand-patch — a hand-patched output desynchronises from the prompt that produced it, and the next run regresses. |

Report as:

```
SELF-SCORE: 9/10 — deducted on #5 (offline state for the responder screen is described
but not laid out; logged in Risks)
```

---

## 📤 HANDOFF block format

Every node emits YAML that its children consume verbatim. Keys must be stable across runs —
children reference them by name.

```yaml
HANDOFF:
  node: "1A"
  version: "1.0.0"
  produces: [COLOR_CONTRACT, PERSONA_SET]
  consumed_by: ["2A", "3A", "3B"]

  # ── the actual payload, node-specific ──
  color_contract:
    …
  persona_set:
    …

  # ── always present ──
  new_tokens_proposed: []      # must be [] or escalated
  constraints_added: []        # new invariants children must respect
  open_questions: []
```

**Rule:** if `new_tokens_proposed` is non-empty, the handoff is **provisional** — a human must
triage the tokens into `tokens/ridejaunm.tokens.json` before child nodes run. Otherwise the
children build on sand.
