# Paris '26 Trip Site

Single-page static site (`index.html`, no build step) — a phone-friendly trip guide for the
Saints @ Steelers NFL Paris Game weekend, Oct 23–26 2026. Hosted on GitHub Pages at
https://hogwor.github.io/paris-2026/, deployed from the `main` branch root (not GitHub Actions —
the `gh` token in use lacks `workflow` scope, so `.github/workflows/deploy.yml` exists locally but
is untracked/not pushed; Pages is configured as "Deploy from a branch").

## Structure
Everything — markup, CSS, JS — lives in `index.html`. Tabs (`<nav class="tabs">` /
`<section class="panel">` pairs) are toggled via `display:none/block` + a click handler near the
bottom `<script>` block. Sections: Itinerary, Train, Where to Stay, Map, Booking, Vote, Packing.
Per-user picks/checklists/votes persist via `localStorage` only (no backend, nothing syncs between
phones).

## Map tab — important history
The Map tab (`#map` section) is a **static grid of plain `<img>` OSM tiles positioned with fixed
inline CSS**, not an interactive Leaflet map. This was deliberate: an earlier Leaflet-based version
had a persistent rendering bug (a left-side gap where tiles wouldn't paint) that was confirmed on a
real iPhone and reproduced identically across 8+ independent fixes (any3d toggling, disabling the
panel fade animation, ResizeObserver, `backface-visibility`, unwrapping `border-radius`+`overflow`
from the transformed tile container, `translateZ(0)`, single-domain tile URLs, removing redundant
`invalidateSize()` calls). A bare-bones Leaflet test page rendered perfectly in the same browser,
proving Leaflet itself was fine — something about how the app page used it broke tile painting in a
way that was never fully root-caused. The static-grid replacement sidesteps the whole problem: no
JS-driven layout, no CSS transforms on the tile grid, so there's no equivalent bug surface.

If you touch the map: it's a 7×6 tile grid at zoom 13 centered on Île de la Cité (48.8636, 2.3445),
centered in its viewport via the pre-transform CSS trick (`left:50%` + negative `margin-left`, no
`transform`) — deliberately avoiding transforms after the above saga. Pins are small
`div`s with `transform:rotate(-45deg)` for the teardrop shape; that's a different, low-risk use of
transform (single small isolated element, not a big tile grid under `overflow:hidden`) and rendered
fine throughout testing. Tile/pin pixel coordinates were precomputed in Python via standard Web
Mercator projection (see chat history / regenerate with a short script if coordinates ever need to
change) — there's no client-side JS doing this math.

If real interactive pan/zoom is ever wanted again, reach for Leaflet with fresh eyes (or try
Mapbox GL / MapLibre as a different rendering engine) rather than re-debugging the old approach.

## Deploying
```
gh api -X POST repos/hogwor/paris-2026/pages/builds   # trigger a rebuild after pushing to main
gh api repos/hogwor/paris-2026/pages/builds/latest     # poll status
```
GitHub's Pages build backend is flaky here — builds intermittently fail with a generic
"Deployment failed, try again later." (check `gh run list -R hogwor/paris-2026`); just retrying the
build usually succeeds on the 2nd attempt. Always verify the live commit hash matches what you
pushed (`gh api repos/hogwor/paris-2026/pages/builds/latest` → `commit` field) before telling the
user it's live.

## Local preview
`python3 -m http.server 8000` from the repo root, or use the Claude Code preview tool
(`.claude/launch.json` lives in the vault root, not this repo, since preview_start looks in the
primary working directory).
