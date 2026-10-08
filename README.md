# TIDE

A cozy RuneScape-inspired island skilling game in Three.js, built for iPhone in landscape.

Chop, mine, fish, cook, smith and climb across a procedurally generated island of terraced cliffs. Summit Cairnhold Peak, trade with Odda, then row far out to Fern Isle where a small herd of brontosaurs wades in the shallows.

## Features
- **Six skills:** Woodcutting, Mining, Fishing (Stardew-style reel), Cooking, Agility, Smithing
- **Smithing + economy:** furnace and anvil, bronze/iron bars, tool upgrades (hatchet, pickaxe, fish hooks), nails; coins and Odda's market stall (buy/sell); eat cooked fish and stew
- **Cairnhold Peak:** a central mountain with pines, mountain goats and a summit cairn; first ascent triggers a Skyrim-style aura celebration
- **Fern Isle:** a distant island reached by rowboat, with dunes, palms, a shipwreck, tide pool, crabs, beachcombing (shells, pearls, bottles) and a wading brontosaurus herd
- **Mediterranean coast:** turquoise shallows over white sand fading to sapphire water, dark seagrass meadows, pale limestone sea cliffs, limestone sea stacks, cypresses, umbrella pines, lavender, agave and prickly pear, and a lantern-lit quay
- **Frostmere Plain:** a high tundra plateau with a broken ring of rune-carved standing stones
- **Pets, health + stamina, resting by campfires, day/night cycle**
- **Living water:** wadeable shallows, crisp pixel caustics over a pebbled bed, refraction wobble, light reflections at night, floating leaves, wet sand and a moving tide line
- **Look:** always Crisp pixels (no dither, locked pixel grid) with an Abyss colour grade by default; ink outlines and other grades in Settings
- **Voxel minimap** (Cube World style) that rotates with the camera; tap to travel

## Run locally
It's a single static file. Serve the folder (ES modules need http, not file://):

```
npx serve .
```

## Deploy
Static site, no build step. Vercel serves `index.html` from the repo root; pushing to `main` redeploys.

Progress is saved in the browser's localStorage (`brackenholt-v1`) for the domain it's served from.
