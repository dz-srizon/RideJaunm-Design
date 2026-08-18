# RideJaunm Prompt Tree — v1.0.0

A hierarchical prompt system that encodes the locked design decisions in
`docs/` and `tokens/` so a designer, copywriter, reviewer or agent can
produce on-brand work without re-deriving the law.

```
RideJaunm Prompt Tree  v1.0.0
│
├── 00-root/                          ALWAYS LOAD
│   ├── SYSTEM.md                     identity, product, how you work
│   ├── CONSTITUTION.md               10 visual laws + token/type/content/safety
│   ├── OUTPUT-CONTRACT.md            headers, citations, artefact shapes
│   └── COMPOSITION.md                load order, packs, versioning
│
├── branches/                         LOAD EXACTLY ONE PRIMARY
│   ├── 01-brand.md                   voice-and-copy · critique · moodboard
│   ├── 02-typography.md              specify · blur-test
│   ├── 03-color.md                   apply · audit
│   ├── 04-components.md              atom · molecule · gap
│   ├── 05-ia-flows.md                nav-and-sitemap · flow-a · flow-b ⚠
│   ├── 06-lofi.md                    screen · validate
│   ├── 07-hifi.md                    transition · micro-interaction
│   ├── 08-figma-ops.md               structure · roadmap
│   ├── 09-nepal-offline.md           field · maps-ui
│   ├── 10-sos-safety.md              gate · console · mesh          ⚠ RESTRICTED
│   ├── 11-copy-localisation.md       string · glossary
│   ├── 12-review-qa.md               ship-gate · a11y · design-qa-build
│   └── 13-eng-handoff.md             spec-block · token-export
│
└── packs/                            PRE-COMPOSED STACKS
    ├── designer-session.md
    ├── copy-session.md
    ├── safety-review.md              ⚠ RESTRICTED
    └── agent-session.md
```

⚠ = also load `branches/10-sos-safety` and set `needs-safety-review: true`.

## Quick start

1. Pick a **pack** (or compose from `root/composition`).
2. Paste the named node bodies into the chat **in load order**.
3. Attach the listed `docs/` sections as context — not the whole repo.
4. Put the actual request under `USER TASK`.
5. Expect the output header from `root/output-contract`.

Full instructions: [`README.md`](README.md).
Machine-readable index: [`catalog.json`](catalog.json).
