# BRANCH `08-figma-ops` — File structure, conventions, roadmap

> Parents: `root/*` · Phase: 8 · Safety class: `public` · Version: `1.0.0`
> Context: `docs/08-figma-setup-roadmap.md`

---

## Leaf `08-figma-ops/structure`

```
TASK: Set up, audit or extend the Figma workspace. Do not invent a new sidebar.

TEAM
00 Brand & Research     Research FigJam · Moodboard · User Flows FigJam
01 Design System        RideJaunm — Design System  ⭐ published library
02 Product Design       Core App · Safety & SOS (SEPARATE FILE) · Onboarding
03 Prototypes           Prototypes & Motion
04 Handoff & Archive    Dev Handoff · Archive

SOS lives in its own file because it is safety-critical: separate permissions,
version history and sign-off. A feed redesign must not be able to touch it.

PAGE SIDEBAR (exact, in order)
ℹ️  Cover & Specs
🎨  Design System (Tokens)
🧩  Atoms & Components
🎛️  Complex Modules
🖼️  Moodboard & Assets
✏️  Lo-Fi Wireframes
✨  Hi-Fi Screens
📱  Prototypes
🚧  WIP / Scratch
🗄️  Archive

FRAME NAMES
LF-01-Ride-MapHome-GPSAcquiring
HF-01-Ride-MapHome-Night
HF-14-SOS-Console-MeshOnly
CMP-Button-SOS-Playground

SECTION COLOURS
Grey not started · Blue in progress · Yellow in review ·
Green approved / ready for dev · Red blocked or deprecated

STATUS STAMP on every frame
DRAFT · IN REVIEW · APPROVED · IN DEV · SHIPPED · DEPRECATED
+ owner avatar + date

PERMISSIONS
Design leads: edit all
Designers: edit product, branch-only on Design System
Engineers: view + Dev Mode + comment
PM / stakeholders: view + comment on Product + Prototypes
SOS file: design lead + safety reviewer only

NEVER edit the published library directly on a Friday.

OUTPUT
A setup checklist or an audit of drift (wrong page order, missing stamps,
SOS living in the core file, detached instances, unpublished tokens).
```

---

## Leaf `08-figma-ops/roadmap`

```
TASK: Plan or triage work against the gated 10-week roadmap. Do not reorder gates.

GATE 1  end week 3   Foundations. Published DS v1.0.0. Test screen in 4 modes.
                     NO hi-fi screens before this gate.
GATE 2  end week 6   All atoms + molecules. 54 lo-fi frames from real instances.
                     Rider test round 1 triaged.
GATE 3  end week 10  Hi-fi × 4 modes, MI-1/2/3, Flow A + B prototypes,
                     SOS written safety sign-off, Dev Mode handoff.

ORDER INSIDE GATES is in docs/08 §8.2. Adjust durations, not order.
Anti-patterns: hi-fi before tokens · Feed before HUD · SOS as "one more screen"
· office-only testing · lorem ipsum · happy-path-only · library drift ·
decorative red.

OUTPUT
The task's gate, its order slot, what it is blocked by, and the exit criterion.
```
