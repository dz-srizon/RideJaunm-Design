# BRANCH `09-nepal-offline` — Himalayan data reality

> Parents: `root/*` · Phase: A · Safety class: `public` (safety fields escalate)
> Version: `1.0.0`
> Context: `docs/09-nepal-offline-data-spec.md`

This is the layer that makes RideJaunm a Nepal app rather than a translated
Alpine one. Do not model "generic mountains".

---

## Leaf `09-nepal-offline/field`

```
TASK: Spec how an Appendix A field is captured, stored, and surfaced.

SELECTION MODES
Route Corridor (default, 15 km buffer + 25 km around waypoints) ·
Administrative (province → district → municipality, EN+NP names) ·
Custom Box (live size estimate).

ZOOM TIERS
Essential z0–12 · Standard z0–14 (default) · Detailed z0–16 · Satellite +raster

FIELD GROUPS (cite the row, do not invent a parallel one)
A.2.1 Region metadata
A.2.2 Road & route (surface, quality, width, curvature, monsoon_risk,
      landslide_zone, river_crossing, bridge_type, seasonal_status…)
A.2.3 Fuel & services (NOC / private / bottle, fuel_gap_km, punchar shops,
      mobile_coverage polygons that feed SOS)
A.2.4 Safety & emergency  → also load 10-sos-safety if you touch these
      hospitals + trauma flag, army posts, heli landing zones, permit zones,
      cell_dead_zone, emergency numbers (100 / 102 / 101 / 1144 / 103)
A.2.5 Terrain (passes, snowline, AMS > 3,500 m, Kali Gandaki wind after 11:00)
A.2.6 Culture (chiya/bhatti, Dashain/Tihar/bandh closures)
A.2.7 Localisation (en/ne, AD+BS calendar, NPR, Asia/Kathmandu UTC+05:45,
      +977 phones, decimal / DMS / MGRS coords)

OUTPUT
field, type, source, refresh cadence, UI surface, empty/stale/offline
behaviour, trust caption ("Verified by 12 riders · 4 days ago").
If the field is community-sourced, include confirm / dispute affordance.
```

---

## Leaf `09-nepal-offline/maps-ui`

```
TASK: Spec or review the Offline Maps Manager and in-map freshness.

BEHAVIOURAL RULES (locked)
- Never silently stale. > 90 d amber everywhere, including map desaturation.
- Hazard / closure / bandh data expires in 14 d even if tiles are fresh.
- Partial downloads are usable; missing tiles = wireframe, not a void.
- Wi-Fi-first. Mobile-data override shows estimated cost.
- Storage guardian at 90 %. Suggests LRU region. Never auto-deletes.
- Saved-trip corridor stale + start ≤ 7 d → prompt refresh.

WHAT WORKS OFFLINE (print this list verbatim in Help)
✅ Full routing (3 modes), navigation, HUD, GPS, SOS mesh, PTT,
   breadcrumb, saved trips, cached feed, post drafting.
❌ New search beyond cache, live traffic, feed refresh,
   live group location beyond mesh range.

ATTRIBUTION
© OpenStreetMap contributors on every map view, bottom-left, Micro/11,
graphite-300. Licence requirement.

OUTPUT
Screen zones, Card/OfflineRegion content (EN+NP name, MB, zoom, age,
fuel/health/heli counts, permit chip, freshness badge), and the
MI-3 progress state if a download is active.
```
