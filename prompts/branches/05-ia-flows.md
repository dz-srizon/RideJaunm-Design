# BRANCH `05-ia-flows` — Information architecture and critical flows

> Parents: `root/*` · Phase: 5 · Safety class: `public` (Flow B escalates)
> Version: `1.0.0`
> Context: `docs/05-information-architecture.md`

---

## Leaf `05-ia-flows/nav-and-sitemap`

```
TASK: Place a destination in the IA, or audit a proposed nav change.

LOCKED NAV (5 tabs, right-thumb arc, 6.1–6.7")
1 Ride     map-trifold     Map Home / HUD          default
2 Plan     compass         Trip Planner
3 SOS      custom glyph    Emergency Console       CENTRE = fastest thumb
4 Squad    users-three     Groups + live + chat
5 Profile  user-circle     Profile / garage / settings

Community Feed is NOT a primary in-ride tab.
It lives as a top-bar entry on Ride AND as Squad → Feed | Groups | Chat.
Rationale: a scrolling social feed in the ride nav invites browsing at 60 km/h.
Documented alternative: swap Profile → Feed, move Profile to the top-right
avatar. Ship one. Do not run both.

RULES
- Tab bar 64 px + safe area, glass-map. Auto-hides > 15 km/h for 10 s.
  SOS does not hide — it collapses to the 88 px FAB.
- SOS is never badged.
- Depth limit: ≤ 3 taps from tab root. SOS ≤ 2 taps from anywhere.
- Offline parity: Ride, Plan, SOS fully functional offline.
  Squad/Feed degrade to cached + queued.
- Deep links: ridejaunm://route/{id} · ://group/{id}/join ·
  ://sos/{incident} · ://offline/{region}

OUTPUT
- Destination's sitemap path
- Entry points (tab, deep link, long-press quick action)
- Offline behaviour
- Why it is not one level shallower (or deeper)
```

---

## Leaf `05-ia-flows/flow-a`

```
TASK: Author, extend or review Flow A — Create a Group Trip with a Supercurvy route.

LOCKED
Actor:   Sabin, 29, KTM, RE Himalayan 450. 6-rider weekend to Daman via Rajpath.
Goal:    Max-bends route, invite 5, everyone has offline tiles.
Metric:  ≤ 90 s tab-open → invites sent. ≤ 8 taps to route selection.

NODES (do not rename)
A1 Trip Planner empty → A2 Trip type GROUP → A3 Destination search →
A4 Route mode selection (THE signature moment, all 3 routes drawn) →
A5 Supercurvy selected (+ optional A5a Waypoint editor) →
A6 Invite squad → A7 Pre-ride readiness → EXIT saved trip.

EDGE CASES that must stay branched
- No Supercurvy exists (terai) → segment disabled + "Not enough bends here."
- Restricted zone (Upper Mustang) → blocking permit chip + reroute
- Invitee offline → queued, PENDING on roster
- Monsoon-closed segment → warning-400 hatch + alternate
- Group > 20 → suggest sub-squads

OUTPUT
Actor, goal, metric, node-by-node (trigger / ui / data / next), edge-case table.
If you add a node, justify why A1–A7 cannot absorb it.
```

---

## Leaf `05-ia-flows/flow-b`

```
TASK: Author, extend or review Flow B — Offline SOS in a cellular dead zone.

STOP. Also load branches/10-sos-safety. Set needs-safety-review: true.

LOCKED
Actor:   Aayush, Beni → Jomsom, low-side at ~2,600 m, no cell.
         2 squad ahead (4 km), 1 behind (1.5 km).
Goal:    Get help without a network.
Metric:  ≤ 3 s to arm, ≤ 10 s to first mesh ACK, gloves, one-handed on the ground.

PRE-CONDITION (automatic, no modal)
cellular = none > 60 s AND altitude > 2,000 m AND ride active
→ Offline Resilience Mode: BLE mesh advertise, peer scan 15 s,
  last-good GPS every 30 s, breadcrumb to disk,
  banner "OFFLINE · MESH ACTIVE · 3 riders in range".

NODES
B0 Crash detection (parallel, 30 s cancel) →
B1 Trigger (FAB / tab / 5× power / voice / helmet 3 s) →
B2 Emergency Console (NO MAP, Signal Matrix first) →
B3 3.0 s long-press arming →
B4 10 s cancel window →
B5 SOS Active multi-channel broadcast →
B6 Stand down (3 s hold) + incident report.

LADDER (parallel, shown live)
1 BLE mesh every 30 s, TTL 6, 512-byte encrypted payload
2 Walkie-talkie PTT standby
3 Cellular retry every 60 s (queued SMS + POST)
4 Satellite if paired

EDGE CASES that must stay branched
Zero peers · GPS lost · phone at 4 % · false alarm · rider is the responder ·
solo rider (Good Samaritan opt-in).

You may not: single-tap arm, swipe-to-activate, hide the Signal Matrix,
put a map on the console, auto-stand-down, or theme SOS away in Blackout.
```
