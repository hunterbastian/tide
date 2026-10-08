# TIDE: notes for Claude Code

A cozy RuneScape-3-inspired island skilling game. **Three.js r170** (ES module from jsdelivr), **one static file: `index.html`**, no build step. Played mostly on **iPhone in landscape**, so every change must feel right on a small touch screen.

## Run / test
- `npx serve .` then open on desktop and phone (same Wi-Fi), or just open `index.html`.
- There are no tests. Verify by playing: walk, chop, mine, fish, cook, climb, row, rest by the fire, open each panel. Check the console for errors.
- Deploy: push to `main`. Vercel auto-deploys (https://tide-ecru.vercel.app).

## Map of index.html (search for these markers)
| Marker | What lives there |
|---|---|
| `/*GEN*/` ... `/*ENDGEN*/` | Deterministic world gen: heightmap (0–8 levels, mountain massif `W.peaks`, tundra plateau `W.tundra`/`W.plateau` with standing stones), ramps, paths, shallows, dock + Fern Isle position (`W.dock.islet`), entities incl. smithy, Odda's stall, pines, goats (`W.goats`), summit cairn (`W.summit`). Must stay pure + seeded. |
| `save state` | `G` object, localStorage `brackenholt-v1`, one-time migrations (`lookV`, `worldV`). Coins, tools, summit flag live here. |
| `data` | Skills, items, resource defs (`DEF`), recipes (`SMELT`, `SMITH`), `PRICE`, `SHOP`, `FOOD`, pets |
| `icons` | Canvas-drawn item icons (`ICON`) |
| `renderer & scene` | Renderer, sun/hemi lights (`LX`, `PLX` scaling), fog, camera |
| `terrain` | Terrain mesh + colours (Mediterranean coast band via `coastAt`/`coastK`, palette `MED`, limestone walls + `aCoast` warm bounce); caustics/pebbles/refraction/wet-sand injection (`CAUST`, `LANDTEX`); water shader (`WATER_U`: shallows, teal night, sparkles, reflections `uRL/uRC`, foam); ripples, splashes, leaves, `updateWaterFx` |
| `prop helpers` / `entities` | Builders for trees, pines, rocks, fire, chest, cottage, furnace, anvil, stall, goat, cairn, sheep, standing stones, and the coast set (`buildCypress`, `buildStonePine`, merged via `part()` + `mergeGeos()`) |
| `Mediterranean coast` | Instanced lavender/agave/prickly pear, white pebbles, limestone sea stacks (`SEASTACKS`) with foam, the quay (bollards, amphorae, lantern `QUAY`), `updateMedCoast` |
| `rowboat + sandbar` | Boat, rowing, Fern Isle (`buildIsletWorld`, `ISLET`), brontosaurs (`buildBronto`, `makeLimb`/`shapeLimb`, `updateBronto`), beachcombing |
| `pets` | Pet models, following, drop rolls |
| `time of day lighting` | Day cycle, stars, moon, fireflies, wisp |
| `player` | The Wanderer model, tools (axe/pick/hammer + tier tints) |
| `panels` | Backpack/skills/pets/settings; crafting (furnace + anvil) and Odda's stall modals |
| `interaction` / `picking` / `input` | Menus, pathing, walking, climbing; voxel minimap (`MM`) |
| `fishing minigame` | Stardew-style reel (hook tier widens the zone) |
| `color grading` | Post pass: grades (`GRADES`, default `abyss`), pixel modes (0 off, 1 fine, 2 chunky, 3 crisp), dither, outlines, bloom, tilt-shift |
| `loop` | Frame loop, `animateWeight`, summit cinematic (`CINE`, aura `AURA`), celebration pose |
| `resting by the fire + vitals` | Health/stamina, sitting, embers, Warmed buff |

## Engine notes (Three.js r170)
- A tiny `<script type="module">` imports Three.js, sets `window.THREE`, then runs the game code from `<script type="text/plain" id="tide-src">` as a classic script, so all game globals behave like before. Don't turn the game itself into a module unless you also rework the globals.
- The palette was tuned on r128, so colour management is **off** (`THREE.ColorManagement.enabled=false`, `renderer.outputColorSpace=LinearSRGBColorSpace`). Keep it that way or every colour shifts.
- Lights use r155+ physical units: multiply legacy-style intensities by `LX` (PI) for sun/hemisphere, and by `PLX` (PI*0.85, decay 1) for point lights.
- Lambert materials don't use `flatShading` (r128 ignored it; turning it on now would facet everything).
- Custom shader hooks inject at `#include <begin_vertex>`, `<color_fragment>`, `<project_vertex>`, `<tonemapping_fragment>`. If you upgrade Three.js again, check these chunk names still exist.

- **Locked pixels:** `lockPixels()` (called at the end of `updateCamera`) snaps camera translation to the pixel grid and offsets `#game` via CSS; pixel modes also use a long lens (`LENS_FOV`) blended by pitch. If you change the camera, keep `cam.target`/`_look` semantics and call it after `lookAt`.

## Conventions
- Keep it a single file unless asked to split. No frameworks, no bundler.
- Never break old saves: add new `G` fields with defaults, migrate with a one-time flag.
- Anything that changes world gen changes every player's island. Bump `worldV` and reset `G.tile` when you do. (Exception: purely additive props placed last with their own RNG via `place()`; the load check already moves a player standing on a newly blocked tile.)
- Many small parts on one prop? Paint them with `part()` and merge with `mergeGeos()` into one mesh to keep draw calls down.
- Mobile performance first: instanced meshes, no per-frame allocations in hot loops, keep draw calls low.
- The pixel filter is **locked to Crisp (mode 3, no dither)**: `G.settings.pixel` is forced to 3 on every load and the Pixel render setting is gone. Modes 1/2 still exist in the shader if you ever want them back. Default grade is Abyss, outlines off (migration `lookV=3`).
- Write UI copy in plain, warm, short sentences.

## Testing tips
- Serve over http (the module import fails on file://). Headless Chrome with SwiftShader works for screenshots.
- To capture frame-accurate clips, freeze the loop (`dt=0` behind a flag) and step systems manually (`updatePlayer`, `updateCamera`, `updateBronto`, `updateFx`).
- `W.reach` only covers land; shallow tiles are found with `isSh(i)`.

## Ideas backlog
organic curved shorelines + wider shallows, mountain stream + waterfall, rain + fog weather, curated palette, Construction (use nails on the cottage), Firemaking, collection log, daily tasks, sound ambience, photo mode, export/import save, NPCs.
