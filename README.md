# Travel Explorer

Interactive 3D globe for visualizing personal travel history.

**[Live demo →](https://ruspg.github.io/trips-overview/)**

## What it does

- **3D globe** (Globe.gl / Three.js) with vector country surfaces, atmosphere, and starfield
- **Flight arcs** — gradient lines with flowing dash animation showing direction
- **Ground transport** — trains, buses, cars, boats rendered as surface paths
- **Interactive hover** — cities show visit count & trip list; arcs show flight details; countries light up
- **LOD labels** — city names appear/hide based on zoom level to avoid clutter
- **Trip sidebar** — click to filter routes, fly-to destination, see full itinerary
- **Timeline & Stats** views with duration charts, airline breakdown, country grid

## Data

All trip data lives in `travels.yaml` — structured, no PII. The HTML reads an embedded JS copy of the same data.

**10 trips · 9 countries · 30 flights · 280+ days**

## Stack

Single `index.html`, no build step. External deps via CDN:

- [Globe.gl](https://globe.gl) — WebGL globe
- [Natural Earth](https://www.naturalearthdata.com/) — country polygons (110m)
- Inter + JetBrains Mono — fonts

## Run locally

```
open index.html
```
