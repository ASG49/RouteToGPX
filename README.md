# RouteToGPX

An HTML/JS app that converts a Google Maps directions route into a simplified GPX track with elevation data. Single file, no install, no API keys, runs in any modern browser.

**Version 2.2** · Last updated October 11, 2026 · © 2026 Tony Gozdz · MIT License

**Live app:** https://asg49.github.io/RouteToGPX/

---

## What it does

1. You paste a full Google Maps directions address.
2. The app reads the stops (and travel mode) from the address and asks a free routing service for the road geometry.
3. The track is automatically simplified (default 1%, the lowest GPXWeaver setting).
4. Elevation is added to every remaining point.
5. The route, a moving-cursor elevation profile and ascent/descent totals are shown; one click saves a GPX file.

## Quick start

1. In Google Maps, get directions with all your stops and travel mode.
2. Copy the **full address** from the browser address bar (`https://www.google.com/maps/dir/...`).
3. Paste it into the top box and click **Convert**.
4. Check the route on the map and the profile at the bottom left.
5. Edit the track name if you wish and click **Save GPX**.

> **Short links (`maps.app.goo.gl/...`) cannot be read.** Browsers are not allowed to follow Google's short-link redirect. Open the short link in your browser first, then copy the expanded address from the address bar.

## Features

- **Google Maps input:** `/maps/dir/` addresses (exact stop coordinates are taken from the address data block), `api=1` links (origin, destination, waypoints, travel mode) and `saddr`/`daddr` links. Stops without coordinates are geocoded with Nominatim.
- **Travel mode:** driving, bicycling, walking and transit links are recognized; transit is routed as driving. Can be overridden in Settings.
- **Automatic simplification:** Douglas-Peucker with the same tolerance formula as GPXWeaver v7.0. Default 1% is about 2 m, so the shape is kept while the point count drops sharply (for example 4,702 to 2,237 points on a 313 km loop).
- **Elevation:** every point gets an elevation from free terrain tiles decoded in the browser (up to about 10 m resolution, no rate limits). If tiles cannot be loaded, it falls back to Open-Meteo (90 m resolution, sampled about every 500 m and interpolated).
- **Elevation profile:** shows distance and elevation range, total ascent and descent (10 m noise threshold).
- **Cursor on the profile:** move the mouse over the track on the map (within 40 px) and a vertical line with a distance/elevation label follows on the profile, with a matching dot on the track. Tap or click works on touch screens.
- **Map:** OpenTopoMap by default (colors muted when zoomed out), plus OpenStreetMap, Google Terrain and Google Satellite layers. Centers on your location when opened (asks permission once). After Convert the map fits the whole route with a 5% margin.
- **Compact top bar:** URL, Convert, track name, Save GPX and Settings in one row.
- **GPX output:** GPX 1.1, one track, 6-decimal coordinates, elevation on each point, GPXWeaver-style structure. Uses the Save dialog when the browser supports it, otherwise a normal download.

## Settings (gear button)

| Setting | Default | Notes |
|---|---|---|
| Simplification | 1% | Same scale as GPXWeaver; higher values remove more points |
| Routing profile | Auto | Auto reads the travel mode from the Google link; or force Car, Bicycle, Walking |
| Add elevation | On | Turn off for a faster, elevation-free track |

## How it works

| Step | Service |
|---|---|
| Road geometry (car) | OSRM public demo server |
| Road geometry (bike, walking) | routing.openstreetmap.de |
| Stop geocoding (only if needed) | Nominatim (OpenStreetMap) |
| Elevation (primary) | AWS terrain tiles ("terrarium" format), bilinear sampling |
| Elevation (fallback) | Open-Meteo elevation API |
| Maps | Leaflet 1.9.4 with OpenTopoMap, OpenStreetMap and Google tiles |

Everything runs in your browser. Nothing is uploaded to any server of mine, and no account or API key is needed.

## Good to know

- **The route is recomputed, not copied.** Google does not publish its route line, so the app asks OpenStreetMap-based routers for a route through the same stops. It is usually very close to Google's line, but it can differ on some roads.
- **Public demo servers.** Routing services have fair-use limits and may be slow or unavailable at times. If Convert fails, wait a moment and try again.
- **Elevation sources.** Terrain-tile elevation is a terrain model, so bridges and tunnels follow the ground below them.
- **Very long routes** use lower-zoom terrain tiles (the status bar shows the zoom used, for example `terrain tiles z13`), which is slightly coarser.
- **Browser support.** Current Chrome and Edge are fully supported (Save dialog). Other browsers use a normal file download.

## Hosting

The app is a single HTML file. This repository hosts it with GitHub Pages:

- `RouteToGPX_2.2.html` - the app
- `index.html` - a short launcher that redirects to the current version; update the file name in it for each new version
- `README.md`, `LICENSE`

To run it locally, open `RouteToGPX_2.2.html` in a browser. Opened from a file, some services may refuse requests, so the hosted version is the most reliable.

## Version history

**2.2** (Oct 11, 2026) - Auto-fit leaves a margin of 5% of the map's shorter side
**2.1** (Oct 11, 2026) - Muted OpenTopoMap colors when zoomed out
**2.0** (Oct 11, 2026) - Moving cursor line on the elevation profile
**1.9** (Oct 11, 2026) - Short-link service removed; paste the full address
**1.0 - 1.8** (Oct 11, 2026) - Initial release: URL parsing, routing, 1% simplification, elevation (terrain tiles with fallback), compact top bar, custom icon, locate on open

## Credits

- Simplification algorithm and scale from GPXWeaver (also by the author)
- Map data (c) OpenStreetMap contributors; tiles (c) OpenTopoMap, OSM, Google
- Elevation: Mapzen terrain tiles on AWS Open Data, Open-Meteo
- Routing: OSRM, routing.openstreetmap.de; geocoding: Nominatim
- Map library: Leaflet

## License

MIT License. See `LICENSE`.
