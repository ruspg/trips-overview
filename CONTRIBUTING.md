# Contributing: Adding Travel Data

## Quick Start

The easiest way to add data: paste a ticket/voucher/booking screenshot or PDF into Claude and say:
```
add this to yaml and html
```

Claude will extract the data, strip PII, and update both files.

## Data Rules

| Rule | Format | Example |
|------|--------|---------|
| Dates | ISO 8601 | `2026-05-25` |
| Times | 24h + timezone | `"13:20 +07:00"` |
| Prices | amount + currency | `amount: 35275.60` / `currency: RUB` |
| Airports | IATA 3-letter | `SVO`, `BKK`, `HAN` |
| Trip ID | `YYYY-MM-slug` | `"2026-03-thailand"` |
| Optional fields | Omit entirely | Don't use `null` or `""` |
| PII | **Never include** | No names, passports, phones, emails, booking IDs |

## Adding a New Trip

Add to `travels.yaml` in chronological order:

```yaml
  - id: "YYYY-MM-slug"
    title: "City A → City B → City C"
    countries: [Country1, Country2]
    cities: [City1, City2, City3]
    date_start: YYYY-MM-DD
    date_end: YYYY-MM-DD        # omit if ongoing
    status: ongoing              # or omit for completed
    travelers: [traveler_1, traveler_2]
```

Then add the same trip to the `TRIPS` array in `index.html` with matching fields plus a `color` hex value.

## Adding Flights

In YAML under the trip's `flights:` list:

```yaml
    flights:
      - direction: outbound      # outbound | segment | return
        leg: "Moscow → Bangkok"
        airline: Aeroflot
        flight_number: SU-270
        aircraft: Boeing 777-300ER   # optional
        departure:
          airport: BKK
          city: Bangkok
          date: 2026-05-25
          time: "13:20 +07:00"       # optional
        arrival:
          airport: SVO
          city: Moscow
          terminal: C                # optional
          date: 2026-05-25
          time: "19:00 +03:00"
        class: Economy               # optional
        baggage: "23 kg"             # optional
        seat: 26G                    # optional (not PII)
        price:                       # optional
          amount: 35275.60
          currency: RUB
          per: person                # or for_all: true
```

In `index.html`, the compact JS version:

```js
{leg:"Moscow → Bangkok",dir:"outbound",airline:"Aeroflot",flight:"SU-270",aircraft:"Boeing 777-300ER",date:"2026-02-28",class:"Economy"},
```

## Adding Ground/Water Transport

YAML:

```yaml
    transport:
      - type: train              # train | bus | car | boat | ferry
        name: "Sapsan"           # optional
        route: "Moscow → Saint Petersburg"
        departure:
          city: Moscow
          date: 2023-12-27
          time: "17:40 +03:00"
        arrival:
          city: Saint Petersburg
          date: 2023-12-27
          time: "21:37 +03:00"
        class: Business          # optional
```

In `index.html`:

```js
transport:[
  {leg:"Moscow → Saint Petersburg",mode:"train",type:"Sapsan (Business)",date:"2023-12-27"},
],
```

## Adding Hotels

YAML:

```yaml
    hotels:
      - name: "Hotel Name 4*"
        city: Bangkok
        address: "123 Street"       # optional
        check_in: 2026-03-01
        check_out: 2026-03-03       # optional
        nights: 2
        room_type: "Deluxe King"    # optional
        meals: breakfast             # optional: breakfast | none | "Room only"
        price:                       # optional
          amount: 25601
          currency: RUB
```

In `index.html`:

```js
{name:"Hotel Name",city:"Bangkok",nights:2},
```

## Adding New Cities / Countries

When a trip includes a city not yet on the globe:

1. Add coordinates to `COORDS` in `index.html`:
   ```js
   "CityName":{lat:XX.XXXX,lng:YY.YYYY},
   ```

2. Add label priority to `CITY_PRIORITY`:
   - `1` = major hub (always visible from far zoom)
   - `2` = secondary city (visible at medium zoom)
   - `3` = POI / micro-location (close zoom only)

3. If new country, add to `FLAGS` and `ISO_A3`:
   ```js
   const FLAGS = {..., "CountryName":"<flag emoji>"};
   const ISO_A3 = {..., "CountryName":"ISO"};
   ```

## Checklist

- [ ] Data added to `travels.yaml`
- [ ] Data added to `index.html` TRIPS array
- [ ] New city coordinates in `COORDS`
- [ ] New city in `CITY_PRIORITY` (1/2/3)
- [ ] New country in `FLAGS` + `ISO_A3` (if applicable)
- [ ] No PII in committed data
- [ ] `git push` triggers GitHub Pages deploy
