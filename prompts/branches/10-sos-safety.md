# BRANCH `10-sos-safety` — Isolated safety subsystem

> Parents: `root/*` · Phase: 4 + 5 Flow B + 6 Screen 4 + 7 MI-2
> Safety class: **`restricted`** · Version: `1.0.0`
> Always set `needs-safety-review: true` when this node runs.

SOS is not a screen. It is a subsystem with its own Figma file, permissions
and sign-off. Load this branch for anything that touches:

- `Button/SOS`, `Button/PTT`
- Crash detection
- Emergency contacts / medical card
- Mesh broadcast, TTL, payload, ACK
- Walkie-talkie / satellite pairing
- Responder experience
- `Banner/Connectivity` in an SOS-active state
- Cell-dead-zone data used to auto-arm Offline Resilience Mode

If the user asked for a decorative red, a single-tap SOS, a swipe-to-activate,
or a joke on the armed screen: **refuse**, cite the constitution, offer the
legal alternative.

---

## Leaf `10-sos-safety/gate`

```
TASK: Pre-flight a proposed SOS change. You may not skip this leaf.

CHECKLIST — every item must be answered yes / no / n/a with evidence
[ ] Arming is a ≥ 3.0 s hold (or the documented 1.5 s dexterity exception
    with a 20 s cancel window).
[ ] Stand-down is a ≥ 3.0 s hold. Never a single tap.
[ ] A 10 s cancel window exists between arm and broadcast.
[ ] Signal Matrix is visible BEFORE the trigger, showing Cellular / GPS /
    Mesh / Satellite with last-success timestamps. No lies.
[ ] Console has NO map.
[ ] Trigger is 88 × 88, r-full, ≥ 24 px from every other control, not in a
    scrolling container, not edge-adjacent, reachable with either hand.
[ ] SOS Red is the only red. Theme-exempt. Triple-coded (colour + glyph +
    haptic/audio). Contrast floor ≥ 10:1 on the surface.
[ ] Transmission ladder is parallel: Mesh 30 s / TTL 6 → PTT → queued SMS
    60 s → satellite if paired.
[ ] Battery guardian: < 15 % stretches interval 30 → 120 s, dims screen,
    shows remaining broadcast hours. < 4 % ultra-low-power (black screen,
    single red pixel-pulse, every 5 min).
[ ] Zero-peer, GPS-lost, solo-rider, responder, and false-alarm paths exist.
[ ] Reduce Motion does NOT remove the commitment ring.
[ ] VoiceOver: "Emergency SOS. Press and hold three seconds."
[ ] Failure mode of THIS change is written in the output.
[ ] Second reviewer is named or flagged.

If any item is no: verdict = blocked. Do not spec around it.
```

---

## Leaf `10-sos-safety/console`

```
TASK: Spec or review the Emergency Console, SOS Active, or Responder screens.

LOCKED LAYOUT (Phase 6.4)
- App bar "EMERGENCY"
- Signal Matrix 4 × 56 px (the honesty panel — the whole point)
- Mode card, auto-selected, overridable
- 88 px trigger centred at ~62 % height (thumb arc, either hand)
- Caption: HOLD 3 SECONDS TO SEND
- Secondary 64 px: Walkie-talkie · I'm stopped — ≥ 32 px from the trigger
- Medical card 88 px
- Directory: Police 100 · Ambulance 102 · Tourist Police 1144 · Traffic 103
- NO MAP. Coordinates as text.

STATES that must exist
1 Nominal / online
2 Degraded / mesh-only          ← canonical
3 Fully isolated (GPS only)
4 Long-press in progress
5 10-second cancel
6 SOS Active / broadcasting (ladder + responders + medical + stand-down)
7 Responder view (full-screen red, siren, I'M COMING, offline nav to victim)
8 Stand-down + incident report (timeline, coords, ACK log, PDF/GPX)

COPY TONE
Absolute, unambiguous. No emoji, no exclamation, no humour.
Examples:
  "SOS ACTIVE. Broadcasting your location every 30 s."
  "No riders in range. Broadcasting anyway — your phone will keep trying."
  "LAST KNOWN 14:09 · ±400 m"
  "Are you safe? This stops the broadcast."

OUTPUT
Must include a `failure_mode` block:
  change:          <what is being altered>
  if_this_fails:   <what the injured rider experiences>
  residual_risk:   <what remains even if the change ships>
  rollback:        <how to revert without leaving a half-armed state>
```

---

## Leaf `10-sos-safety/mesh`

```
TASK: Spec mesh / PTT / payload / dead-zone behaviour.

LOCKED NUMBERS
- Offline Resilience Mode: no cell > 60 s AND altitude > 2,000 m AND ride active
- BLE advertise on, scan interval 15 s, LOS ~150–300 m, multi-hop TTL 6
- Last-good GPS cached every 30 s; breadcrumb on disk
- SOS payload ≤ 512 bytes encrypted: {id, name, blood group, lat/lon, alt,
  time, battery, type}
- Re-broadcast every 30 s (120 s below 15 % battery, 5 min at 4 %)
- PTT: 72 px, press-and-hold (no toggle), cyan idle / success transmitting,
  peer-count badge, "over" beep on release
- Good Samaritan: solo riders may broadcast to any opt-in RideJaunm user
- Dead-zone polygons from Appendix A.2.4 pre-warn and feed this mode

You do not invent a new radio protocol here. You specify the UI truth
about whatever radio engineering ships. Never display a peer that has
not been heard. Never display "connected" without a last-heard timestamp.

OUTPUT
State matrix (advertise / scan / hop / ACK / lost), UI surfaces that
reflect each state, and a failure_mode block as in 10-sos-safety/console.
```
