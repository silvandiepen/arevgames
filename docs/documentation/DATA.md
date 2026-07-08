# Data Requirements

ArevGames uses ArevData as the source of truth, but the games must run from bundled local data/assets.

The hosted ArevData API is useful for docs, demos, and external users. It must not be required during gameplay.

## ArevData audit

Current ArevData coverage is enough for the planned first apps.

| Game need | ArevData source | Used by |
| --- | --- | --- |
| Countries | `countries`, country names, ISO codes, capitals, continents, currencies, languages, TLD, recognized flag | All apps |
| Flags | `flagData`, flag colors, similar flag groups, SVG/PNG URL helpers | Arev Flags, Guess the Country |
| Similar flags | `getSimilarFlags()` / `FlagInfo.similar` | Flag distractors |
| Country geography | `countryGeography`, centroid, bounds, area, landlocked, neighbors, climate, avg temperature | Pinpoint, Map Tap, Guess the Country |
| Geo helpers | `haversineDistance`, bearing helpers, geo hints | Pinpoint scoring and future hint modes |
| Cities | 430+ cities, all national capitals, coordinates, population, capital metadata | Pinpoint city/capital modes, Guess by Capital |
| World map | SVG paths for countries/territories, world viewBox, highlighting helpers | Pinpoint, Map Tap, map clues |
| Country shapes | standalone country SVG helper derived from world-map data | Guess by Shape |
| Places | sharded larger place data | Future expansion only |

## Required generated data

Generate these files into `data/generated/`:

```txt
data/generated/
  export-metadata.json
  countries.json
  flags.json
  cities.json
  geography.json
  world-map.json
  app-indexes.json
```

Generate or copy these assets into a predictable location:

```txt
data/generated/assets/
  flags/svg/{alpha2}.svg
  flags/png/{alpha2}@1x.png
  flags/png/{alpha2}@2x.png
  flags/png/{alpha2}@3x.png
```

If the final app uses SVG directly, PNG generation may be optional, but there must still be a local bundled flag asset per gameplay country.

## Manual source data

Use `data/source/` for game-specific overrides that do not belong in ArevData yet.

```txt
data/source/
  export-config.json
  manual-country-obscurity.json
  manual-city-obscurity.json
  app-mode-presets.json
```

Manual overrides should be small and reviewed. Prefer deriving from ArevData where possible.

## Swift-friendly schemas

### countries.json

```json
[
  {
    "alpha2": "MT",
    "alpha3": "MLT",
    "name": "Malta",
    "nativeName": "Malta",
    "capital": "Valletta",
    "continent": "Europe",
    "currency": "EUR",
    "currencyName": "Euro",
    "currencySymbol": "€",
    "languages": ["mt", "en"],
    "tld": ".mt",
    "recognized": true
  }
]
```

### flags.json

```json
[
  {
    "alpha2": "MT",
    "assetName": "flag_mt",
    "svgPath": "assets/flags/svg/mt.svg",
    "pngPath1x": "assets/flags/png/mt@1x.png",
    "pngPath2x": "assets/flags/png/mt@2x.png",
    "pngPath3x": "assets/flags/png/mt@3x.png",
    "colors": ["red", "white"],
    "similar": ["ID", "MC", "PL"]
  }
]
```

### cities.json

```json
[
  {
    "id": "mt-valletta",
    "name": "Valletta",
    "country": "MT",
    "lat": 35.8997,
    "lon": 14.5146,
    "population": 5827,
    "capital": true,
    "capitalTypes": ["country"],
    "obscurityTier": 3
  }
]
```

### geography.json

```json
[
  {
    "alpha2": "MT",
    "lat": 35.9375,
    "lon": 14.3754,
    "bounds": {
      "north": 36.08,
      "south": 35.79,
      "east": 14.58,
      "west": 14.18
    },
    "area": 316,
    "landlocked": false,
    "neighbors": [],
    "climate": "mediterranean",
    "avgTemperature": 19,
    "obscurityTier": 3,
    "isTinyCountry": true,
    "isIslandCountry": true
  }
]
```

### world-map.json

```json
{
  "viewBox": "0 0 2000 857",
  "projection": "equirectangular",
  "countries": [
    {
      "alpha2": "MT",
      "name": "Malta",
      "paths": ["M..."],
      "bounds": {
        "minX": 1000,
        "minY": 400,
        "maxX": 1010,
        "maxY": 410
      }
    }
  ]
}
```

### app-indexes.json

```json
{
  "countriesWithFlags": ["AM", "MT", "NL"],
  "countriesWithMapPaths": ["AM", "MT", "NL"],
  "countriesWithCapitalCities": ["AM", "MT", "NL"],
  "countriesByContinent": {
    "Europe": ["AM", "MT", "NL"]
  },
  "countriesByObscurityTier": {
    "1": ["US", "FR", "JP"],
    "2": [],
    "3": ["AM", "MT"],
    "4": []
  }
}
```

## Export tool

Create the exporter at:

```txt
tools/export-arev-data/
```

Responsibilities:

1. Read ArevData packages.
2. Normalize country codes to uppercase alpha-2.
3. Generate compact Swift-friendly JSON.
4. Copy/generate local flag assets.
5. Generate map path data.
6. Derive obscurity tiers.
7. Generate app indexes.
8. Validate all mode requirements.
9. Write stable sorted output.
10. Fail CI on invalid output.

## Validation rules

Fail export if:

- Duplicate country codes exist.
- A flag references an unknown country.
- A similar-flag country code is unknown and not intentionally allowed.
- An MVP gameplay country has no flag.
- A map-mode country has no map path.
- A capital-mode country has no capital coordinate.
- A generated mode cannot produce enough answer options.
- JSON output is not deterministic.

Warn if:

- A city has no population.
- A country/city uses manual obscurity fallback.
- SVG geometry is too complex and needs simplification.
- A map territory exists outside the main country list.

## Obscurity tiers

Use four tiers:

| Tier | Meaning | Example type |
| --- | --- | --- |
| 1 | Common | globally known countries/capitals |
| 2 | Regular | familiar countries/cities |
| 3 | Hard | smaller/less common countries/cities |
| 4 | Obscure | tiny states, remote islands, obscure cities |

Country tier heuristic:

- Population.
- Area.
- Continent distribution.
- Recognized state vs territory.
- Tiny/island country flags.
- Manual overrides.

City tier heuristic:

- Capital status.
- Population.
- Country tier.
- Global/tourist significance override.

## Runtime rules

At runtime the app must:

- Load bundled resources.
- Work offline.
- Never require `api.arevdata.com`.
- Never require remote flag image URLs.
- Skip invalid questions instead of crashing production builds.
- Expose debug validation failures in development builds.

## Future data improvements

- Add localized country names.
- Add audited country shape simplification.
- Add a city expansion using `@arevs/places` only when Pinpoint needs more than the curated city set.
- Add data diff reports for ArevData updates.
- Add visual validation for flag assets and map paths.
