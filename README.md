# Seattle Crime Dashboard

An interactive, browser-based visualization of Seattle Police Department crime data from 2008 to 2026, covering 1.5 million+ incidents across 60 neighborhoods.

---

## Features

- **Interactive map** — canvas-rendered crime dots on a Leaflet map; hover for incident details, click to select a neighborhood
- **Crime filters** — filter by category (Violent Crime, Property Crime, All Other), subcategory/type, and year range (2008–2026)
- **Neighborhood explorer** — alphabetical sidebar list and ranked panel; click either to zoom in and open the detail panel
- **Neighborhood detail panel** with two tabs:
  - **Top Crimes** — horizontal bar chart of the most frequent crime types
  - **Over Time** — line/area trend chart with **5yr / 10yr / 20yr / All** time-window selector
- **Light / Dark mode** toggle
- **Responsive stats bar** — live incident count and active date range in the header

---

## Project Structure

```
Final_Project/
├── seattle_crime.html      # Main application (single-page, no build step)
├── styles.css              # All styling — dark/light themes, layout, charts
├── crime_aggregated.json   # Pre-aggregated neighborhood stats (used for charts & ranking)
└── crime_points.json       # Sampled individual crime points (used for map dots)
```

---

## Data Format

### `crime_aggregated.json`

Drives the sidebar, ranking panel, trend chart, and bar chart.

```json
{
  "neighborhoods": ["ALASKA JUNCTION", ...],   // 60 neighborhoods
  "categories":    ["ALL OTHER", "PROPERTY CRIME", "VIOLENT CRIME"],
  "subcategories": ["MOTOR VEHICLE THEFT", ...],  // 28 types
  "data": {
    "WALLINGFORD": {
      "subs":  [["MOTOR VEHICLE THEFT", 420], ...],  // sorted by count desc
      "years": {
        "2021": { "PROPERTY CRIME": 310, "VIOLENT CRIME": 45, ... },
        ...
      }
    },
    ...
  }
}
```

### `crime_points.json`

Drives the map canvas. Each entry is a compact array:

```json
[lat, lon, "NEIGHBORHOOD", "CATEGORY", "SUBCATEGORY", year]
```

Example:
```json
[47.65194, -122.33003, "WALLINGFORD", "PROPERTY CRIME", "MOTOR VEHICLE THEFT", 2021]
```

> The points file is a sampled subset (~22 000 records) of the full 1.5 M dataset, used to keep the page fast in the browser.

---

## How to Run

No build tools, frameworks, or package installs are needed.

### Option 1 — Python (recommended, built into macOS / Linux)

```bash
cd Final_Project
python3 -m http.server 8080
```

Then open **http://localhost:8080/seattle_crime.html** in your browser.

### Option 2 — VS Code Live Server extension

Right-click `seattle_crime.html` in VS Code and choose **Open with Live Server**.

> **Do not open the HTML file directly** (`file://…`). The page fetches the JSON data files via `fetch()`, which browsers block on `file://` origins due to CORS restrictions. You must serve it through a local HTTP server.

---

## How to Use the Dashboard

| Action | What it does |
|---|---|
| Hover a dot on the map | Shows a tooltip with crime type, neighborhood, and year |
| Click a dot or neighborhood name | Zooms the map and opens the detail panel |
| Crime Category / Crime Type dropdowns | Filters both the map dots and all charts |
| Year Range sliders | Restricts the data to the selected year window |
| Top 5 / 10 / 20 buttons | Changes how many crime types appear in the bar chart |
| **5yr / 10yr / 20yr / All** buttons | Changes the time window of the trend chart only |
| ⊙ reset button (map) | Clears the selected neighborhood and resets the map view |
| ↺ Reset All Filters button | Resets all dropdowns, sliders, and selections |
| Dark / Light toggle | Switches the map tile and UI color scheme |

---

## Tech Stack

| Library | Version | Purpose |
|---|---|---|
| [Leaflet](https://leafletjs.com/) | 1.9.4 | Interactive map, tile layers |
| [D3.js](https://d3js.org/) | 7.9.0 | Bar chart, trend chart, scales |
| [CartoCDN](https://carto.com/basemaps/) | — | Dark and light map tiles |

No npm, no bundler — everything loads from CDN and local files.

---

## Live demo Link
http://css1.seattleu.edu/~tharris1/final/seattle_crime.html​


## Data Source

Seattle Police Department — Crime Data export via the [Seattle Open Data Portal](https://data.seattle.gov/Public-Safety/SPD-Crime-Data-2008-Present/tazs-3rd5/about_data). Data covers incident reports from January 2008 through early 2026.
