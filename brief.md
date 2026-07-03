# TYPE — Creative Brief

## Concept
A constellation of Pokemon where each creature is a glowing star. Type determines color (fire=red, water=blue, grass=green, electric=yellow, psychic=pink, etc), total base stats determine star size and brightness. Thin constellation lines connect Pokemon that share a type. Tap a star to focus it and reveal its name, types, and stats. The deeper you explore, the more the type families reveal themselves as color-coded clusters in the constellation.

**Narrative Pattern:** Drill-down — overview of entire constellation, tap a star to focus+highlight its connections and show detail.

**Tier 2 Patterns (pick 2):** Quiet Zone Detail Panel + Hero Number (total Pokemon displayed)

## Data Source
- API endpoint: `https://pokeapi.co/api/v2/pokemon?limit=30&offset=0` (fetch a random offset to get variety)
- Detail endpoint: `https://pokeapi.co/api/v2/pokemon/{id}/`
- Response format: JSON with `name`, `types[].type.name`, `stats[].base_stat`, `id`
- Pokemon type colors map: fire=#ff4444, water=#3399ff, grass=#33cc33, electric=#ffdd33, psychic=#ff66cc, poison=#aa44aa, rock=#bbaabb, ground=#ddbb55, ice=#66ddff, bug=#aabb22, flying=#99aaff, fighting=#bb5544, normal=#aaaaaa, ghost=#6644aa, dragon=#7744ff, dark=#554444, steel=#aaaabb, fairy=#ffaaff
- Fallback: hardcoded array of 10 Pokemon with realistic data (name, types, stats, id)

## Visual Style
- **Preset:** Bold — Circuit (digital precision)
- **Palette:**
```js
const PALETTE = {
  bg: '#0a0a0f',
  track: '#1a1a2e',
  accent1: '#00e676',  // green — grass
  accent2: '#2979ff',  // blue — water
  accent3: '#ff1744',  // red — fire
  glow: '#00e676',
  text: 'rgba(255,255,255,0.5)',
};
```
- **Stroke weight scale:** `{hair:0.5, fine:1, medium:2, heavy:4}`
- **Alpha hierarchy:** `{bg:0.08, field:0.25, active:0.7, label:0.45}`
- **Shape language:** geometric (circles, arcs, straight connection lines)
- **Glow budget:** 1 glow color (white/cyan at 15-25px blur), max 20 entities/frame
- **Motion grammar:** `{settle: expo-out, ambient: linear, pulse: ease-in-out}`

## Encoding Contract
| Data Field | Visual Channel | Range | Inverse Function | "Bigger means..." | Legend? |
|---|---|---|---|---|---|
| total base stats | star radius | 5-30px | invertScale(pixel,5,30,180,720) | stronger Pokemon | yes |
| type (primary) | star color + glow | mapped to type color | hex value lookup | type identity | yes (color key) |
| id | color hue shift (secondary) | 1-300 | N/A | newer generation | no (subtle) |
| type connections | constellation line + glow | connected if typeMatch | N/A | type relationship | yes |

## Interaction
- Tap a star: focus it (magnify 1.2x, highlight its constellation connections, show Quiet Zone panel with name, types, stats)
- Tap empty space: defocus, return to overview
- Auto-pulse: each star has a subtle breathing pulse (0.8-1.2x radius oscillation)
- Idle state: constellation drifts slowly, stars pulse gently
- On first visit: show a brief visual glow ripple from center on first tap

## Communication Requirements
**Tier 1 (mandatory):**
1. Persistent Data Stamp — "PokéAPI · updated [time] · live/fallback", bottom-left, 10px system-ui, opacity 0.4
2. Data Process Honesty — fallback labeled "fallback", never "live"
3. One-to-One Sensory Mapping — bigger star = stronger Pokemon (total stats), color = type (type=>colorKey visible at top-right or legend)

**Tier 2 (pick 2):**
4. Quiet Zone Detail Panel — DOM overlay, positioned bottom-center, shows tapped Pokemon's name (capitalized), types, stats. Fades after 3s idle. Style matches Circuit palette.
5. Hero Number — "30 Pokemon" or similar count at top-right of canvas, 48px, with "in constellation" label below at 11px

**Coherence:**
6. Generative-art axes: one stroke weight scale, alpha hierarchy, shape language (geometric), glow budget (1 glow color, ≤40px blur, ≤20 entities/frame), accent color
7. Motion tokens: `--motion-fast:160ms`, `--motion-base:240ms`, `--motion-slow:360ms`, `--ease-out-expo:cubic-bezier(0.22,1,0.36,1)`
8. AI Slop Score < 30 — no Inter/Roboto/Open Sans, no purple gradient, layered bg (dot grid on dark), at least 1 animation (star pulse)

## Canvas Code Requirements (MANDATORY)
1. Single HTML file, no external dependencies, no CDN libraries
2. Config object `C` at top with all tunable values
3. DPR-aware canvas setup: `Math.min(window.devicePixelRatio || 1, 2)`, `ctx.setTransform(dpr, 0, 0, dpr, 0, 0)`
4. Canvas CSS: `position:fixed;top:0;left:0;width:100%;height:100%;display:block`
5. Touch guard pattern — `let tapping=false; ontouchstart` gate
6. First frame never blank — warmup constellation state seeded immediately
7. Realistic fallback data if API fails — 10 Pokemon with real name, types, stats, id
8. Short description text (`#desc` div, bottom center, "Each Pokemon is a star in this type-driven constellation", 11px, opacity 0.45, system-ui sans-serif)
9. Full-viewport, mobile-responsive
10. Works offline after first load (no CDN deps)
11. `.nojekyll` file at project root
12. Font: use system-ui, sans-serif for all chrome text. No serif fonts. No em-dashes — use hyphens for all placeholder values.
13. Chrome positions: stamp at bottom:20px left:16px, legend at bottom:44px center, desc at bottom:68px center. Never overlap chrome elements.

## Interaction Affordances
- On first visit, show subtle visual feedback when user touches canvas (a brief glow at touch point)
- Stars that are tappable have a subtle breathing pulse (scale oscillation) to indicate interactivity
- Focused star shows a brief expanding ring animation
- After 3 seconds of inactivity on a focused star, the Quiet Zone panel fades out

## Type Color Key (use as legend)
fire=#ff4444, water=#3399ff, grass=#33cc33, electric=#ffdd33, psychic=#ff66cc, poison=#aa44aa, rock=#bbaabb, ground=#ddbb55, ice=#66ddff, bug=#aabb22, flying=#99aaff, fighting=#bb5544, normal=#aaaaaa, ghost=#6644aa, dragon=#7744ff, dark=#554444, steel=#aaaabb, fairy=#ffaaff
