# ZDV Enroute Sector Map

Interactive map of FAA Enroute sectors for Denver ARTCC (ZDV), rendered from GeoJSON and deployable as a static site on GitHub Pages.

## Features

- Toggle between **Low** (SFC–FL260) and **High** (FL270–FL600) sector bands, or view both simultaneously
- Sector polygons with distinct colors, hover highlighting, and click popups showing sector ID and altitude range
- Sector ID labels at polygon centroids
- Dark basemap via CartoDB (no API key required)

## Data

`zdv.geojson` contains 37 features representing ZDV enroute sectors. Each feature has three properties:

| Property | Description |
|---|---|
| `id` | Sector number |
| `alt_lo` | Floor altitude (hundreds of feet; `0` = surface) |
| `alt_hi` | Ceiling altitude (hundreds of feet) |

Sectors with `alt_hi = 260` are low sectors (FL000–FL260); sectors with `alt_lo = 270` are high sectors (FL270–FL600).

## Deployment

Push `index.html` and `zdv.geojson` to a GitHub repository, then enable GitHub Pages under **Settings → Pages** with source set to the `main` branch at root `/`.

The map will be available at `https://<username>.github.io/<repo>/`.

## Local Development

Serve the directory with any static file server, for example:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080` in a browser. Opening `index.html` directly as a `file://` URL will not work because the GeoJSON is loaded via `fetch`.

## Stack

- [Leaflet.js](https://leafletjs.com/) for map rendering
- [CartoDB Dark Matter](https://carto.com/basemaps/) basemap tiles
- No build step — plain HTML/JS
