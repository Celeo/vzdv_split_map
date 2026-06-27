# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Local Development

Opening `index.html` directly as a `file://` URL will not work — the GeoJSON is loaded via `fetch`. Use a static server:

```bash
python3 -m http.server 8080
```

## Architecture

This is a no-build static site. All logic lives in `index.html` as inline `<script>` and `<style>` blocks. There is no package.json, bundler, or transpilation step.

**Data flow:** `fetch('zdv.geojson')` → split features into low (`alt_hi <= 260`) and high (`alt_lo >= 270`) bands → build two Leaflet `L.geoJSON` layers and corresponding `L.marker` label arrays → `showBand()` swaps which layers are on the map.

**Key globals in `index.html`:**
- `lowLayer` / `highLayer` — Leaflet GeoJSON layers
- `labelLayers` — `{ low: L.marker[], high: L.marker[] }` for sector ID labels
- `showBand(band)` — the only public-facing function; accepts `'low'`, `'high'`, or `'both'`

## GeoJSON Schema

`zdv.geojson` — 37 polygon features, each with:
- `id`: sector number (not unique; some sectors span multiple polygons)
- `alt_lo`: floor in hundreds of feet (`0` = surface)
- `alt_hi`: ceiling in hundreds of feet

Low sectors: `alt_hi === 260` (16 features). High sectors: `alt_lo === 270` (21 features).
