# TIDE

A cozy RuneScape-inspired island skilling game built with Three.js, designed for iPhone in landscape.

Chop, mine, fish, cook and climb across a procedurally generated island of terraced cliffs, then row out to the sandbar, rest by the fire, and collect pets.

## Features
- Skills: Woodcutting, Mining, Fishing (Stardew-style reel), Cooking, Agility
- Pets, rowboat crafting, wading shallows, day/night cycle
- Pixel render with ink outlines, ordered dithering and color-grade presets
- Health + stamina, resting by campfires

## Run locally
It's a single static file. Open `index.html` in a browser, or serve the folder:

```
npx serve .
```

## Deploy
Static site, no build step. Vercel serves `index.html` from the repo root.

Progress is saved in the browser's localStorage for the domain it's served from.
