# Sprouse Hollow Explorer

An interactive historical and property map centered on **271 Sprouse Hollow Lane, Craigsville, Augusta County, Virginia**.

Built as a single `index.html` file — no build step, no dependencies to install.

---

## Opening Locally

Just double-click `index.html`, or serve it with any static file server:

```bash
# Python 3
python3 -m http.server 8080
# then open http://localhost:8080
```

> **Note:** The browser Geolocation API requires either `localhost` or an HTTPS origin. Opening the file via `file://` will cause geolocation to be blocked in most browsers — use a local server or deploy to HTTPS for GPS to work.

---

## Deploying to GitHub Pages

1. Create a new GitHub repository (e.g., `sprouse-hollow-map`)
2. Push `index.html` and `README.md` to the `main` branch:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Initial commit — Sprouse Hollow Explorer"
   git remote add origin https://github.com/YOUR_USERNAME/sprouse-hollow-map.git
   git push -u origin main
   ```
3. In the repository on GitHub, go to **Settings → Pages**
4. Under **Source**, select **Deploy from a branch** → `main` → `/ (root)` → **Save**
5. After ~60 seconds, the app will be live at:
   `https://YOUR_USERNAME.github.io/sprouse-hollow-map/`

GitHub Pages serves over HTTPS, so geolocation will work.

---

## Deploying to Netlify

**Drag-and-drop (fastest):**
1. Go to [app.netlify.com](https://app.netlify.com)
2. Drag the folder containing `index.html` onto the deploy dropzone
3. Done — Netlify gives you an instant HTTPS URL

**Via CLI:**
```bash
npm install -g netlify-cli
netlify deploy --dir . --prod
```

---

## Features

| Feature | Details |
|---|---|
| **Basemaps** | Satellite (ESRI World Imagery + reference labels), Topographic (USGS National Map), Street (OpenStreetMap) |
| **Live GPS** | Pulsing blue dot with accuracy ring via browser Geolocation API |
| **Simulate Location** | Snaps displayed position to 271 Sprouse Hollow Lane for remote exploration |
| **Parcel Overlays** | Live data from Augusta County ArcGIS REST service (Layer 1: Parcels) |
| **Parcel Popups** | Owner, Parcel ID, acreage, land/improvement/total values, address, link to Assessor |
| **Historical Markers** | 9 custom vintage-style markers with rich contextual descriptions |
| **Sidebar Panel** | Collapsible on desktop; bottom sheet on mobile |
| **Offline UI** | All fonts, icons, and Leaflet loaded from CDN; UI works without parcel/tile data |

---

## Data Sources & Credits

### Parcel Data
- **Augusta County GIS** — Public ArcGIS REST Service
  `https://gis.co.augusta.va.us/arcgis/rest/services/Augusta_GIS_Map/MapServer`
  Layer 1: Parcels (Feature Layer, Polygon, EPSG:3857)
- **Augusta County Assessor** — Property records via
  `https://gis.vgsi.com/augustava/`

### Historical Points of Interest
- **Virginia Iron Furnaces Project** — Estaline Furnace, Mount Torry Furnace
- **Virginia Dept. of Historic Resources** — Mount Torry Furnace (NRHP listing)
- **USGS Geographic Names Information System (GNIS)** — Lebanon Church, Little River Church, place names
- **Augusta County Historical Society** — Craigsville history
- **Local oral tradition and genealogical records** — Frank Sprouse / Sprouse Hollow Gang, Sprouse Family Cemetery

### Map Tiles
- **Satellite:** Esri World Imagery — Esri, Maxar, Earthstar Geographics, GIS User Community
- **Topographic:** USGS National Map, National Geospatial Program
- **Street:** © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors, CC-BY-SA

### Libraries
- [Leaflet.js](https://leafletjs.com) v1.9.4 — BSD 2-Clause License
- [Font Awesome](https://fontawesome.com) v6.5.0 — Free tier (CC BY 4.0 icons)
- [Google Fonts](https://fonts.google.com) — Playfair Display, Source Sans Pro

---

## Augusta County ArcGIS REST Endpoint Notes

The parcel service at `https://gis.co.augusta.va.us/arcgis/rest/services/Augusta_GIS_Map/MapServer/1` is a **public, unauthenticated** ArcGIS Server 10.91 endpoint. It supports GeoJSON output (`f=geojson`) and spatial queries by bounding box.

**If the service is unavailable or rate-limited:**
- The app will display a notification and automatically load a small set of hand-coded approximate parcel outlines for the Sprouse Hollow Lane area as a fallback
- Parcels already loaded in the current session are cached in memory — navigating back to a visited area won't re-fetch
- Try again later; the county server occasionally goes offline for maintenance

**Query limits:** The service returns a maximum of 400 features per request. At zoom levels below 12, parcel fetching is automatically paused to avoid requesting county-wide data.

---

## Disclaimers

- Parcel data is provided by Augusta County GIS and is for **informational purposes only**. Not a survey. Not legally authoritative.
- Some historical information is based on oral tradition and local genealogical records. **Primary documentary sources may be limited** for certain POIs (noted in-app).
- GPS accuracy varies by device and environment. The Simulate Location feature is for remote exploration only.
- Historical POI coordinates are approximate and intended for general reference, not navigation.
