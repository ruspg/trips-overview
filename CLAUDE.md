# Travel Explorer — Agent Instructions

## Project Structure

```
index.html       — single-file Globe.gl app (data + UI + logic)
travels.yaml     — canonical travel database (source of truth)
CONTRIBUTING.md  — data format reference and examples
```

## Architecture

- **travels.yaml** is the detailed source of truth with full booking data
- **index.html** contains a compact JS copy of the same data in the `TRIPS` array
- Both must be kept in sync — when adding data, update both files
- Site is deployed via GitHub Pages from root `/` on `main` branch

## Reading Data

When asked about trips, stats, itineraries, or travel history:

1. Read `travels.yaml` — it has the most detail (times, prices, addresses, room types)
2. The HTML `TRIPS` array is a compact subset for the globe visualization

## Writing Data

When the user provides tickets, vouchers, PDFs, screenshots, or text with travel info:

1. **Extract** relevant data: flights, hotels, transport, activities
2. **Strip PII**: never include names, passport numbers, phones, emails, or booking IDs
3. **Update `travels.yaml`**: add in chronological order following existing structure
4. **Update `index.html`**:
   - Add/update trip in the `TRIPS` array (compact JS format)
   - Add new city coordinates to `COORDS` if needed
   - Add city to `CITY_PRIORITY` (1=major, 2=secondary, 3=POI)
   - Add country to `FLAGS` and `ISO_A3` if new
5. **Commit and push** to deploy to GitHub Pages

See `CONTRIBUTING.md` for detailed field formats and examples.

## Data Conventions

- Dates: `YYYY-MM-DD`
- Times: `"HH:MM +TZ:00"` (24h with timezone offset)
- Airports: IATA 3-letter codes
- Trip IDs: `"YYYY-MM-slug"`
- Omit optional fields entirely (no null/empty values)
- Prices: `amount` (number) + `currency` (string)
- Flight directions: `outbound`, `segment`, `return`
- Transport modes: `train`, `bus`, `car`, `boat`, `ferry`

## Globe Visualization Notes

- Flight routes render as 3D arcs with gradient colors
- Ground/water transport renders as surface paths (dotted lines)
- City labels use LOD: P1 visible always, P2 at medium zoom, P3 only close
- HTML elements used for labels (not Globe.gl 3D labels — those were buggy)
- Country polygons use Natural Earth 110m (600KB, sufficient for globe scale)

## Common Tasks

### "Add this flight/hotel/trip"
Parse → strip PII → update YAML + HTML → commit → push

### "What trips visited Japan?"
Read YAML, filter by countries

### "Show me the stats"
The HTML computes stats from the TRIPS array (countries, cities, flights, days, nights)

### "Update the ongoing trip"
The trip with `status: ongoing` and `date_end: null` is the current one
