# Data Requirements

ArevGames should use ArevData as the source of truth, but the iOS apps should not depend on the hosted API during gameplay.

Use Node/TypeScript export scripts to convert ArevData packages into stable bundled JSON/assets that Swift can load locally.

## ArevData coverage audit

Based on the current ArevData repository/docs, the available data is enough for the first four games.

| Need | ArevData coverage | Use in ArevGames |
| --- | --- | --- |
| Countries | `countries` array, ISO alpha-2/alpha-3, names, capitals, continents, currencies, languages, TLD, recognized flag | All apps |
| Flags | `flagData`, SVG/PNG URLs, colors, similar-flag groups, helpers | Arev Flags, Guess the Country |
| Similar flags | `getSimilarFlags()` and `FlagInfo.similar` | Plausible wrong answers in flag modes |
| Country geography | `countryGeography`, centroids, bounds, area, landlocked, neighbors, climate, avg temperature | Pinpoint, Guess the Country, future Neighbours |
| Geo utilities | Haversine distance, bearings, country distance, geo hints | Pinpoint scoring and future hint modes |
| Cities | 430+ city records, all national capitals, population, coordinates | Pinpoint city/capital modes, Guess by Capital |
| World map | 211 country/territory path entries, viewBox, render/highlight helpers | Pinpoint, Map Tap, map clue modes |
| Country maps/shapes | standalone country SVG helper based on bundled map data | Guess by Shape, shape study |
| Administrative divisions | states/provinces data | Future modes only, not MVP |
| Places | country-sharded place data | Future city expansion only; avoid in first release unless needed |

## Data gaps to solve in ArevGames

ArevData has enough source material, but ArevGames still needs game-specific derived data.

### 1. Native bundle assets

ArevData exposes flag URLs and JS helpers. iOS gameplay should use bundled local assets.

Required export outputs:

```txt
Data/Generated/flags/svg/{alpha2}.svg
Data/Generated/flags/png/{alpha2}@1x.png
Data/Generated/flags/png/{alpha2}@2x.png
Data/Generated/flags/png/{alpha2}@3x.png
```

Alternatively, use bundled SVG rendering if the final Swift stack handles SVG reliably. Do not depend on remote image URLs during gameplay.

### 2. Swift-friendly JSON schemas

Generate stable JSON files with only fields needed by the games.

Do not dump the full ArevData package shape blindly.

### 3. Map hit testing

ArevData provides SVG path data and projection helpers, but ArevGames needs native hit testing.

Required derived/indexed data:

- Country path data in a Swift-loadable format.
- Bounding boxes for quick rejection.
- Country code per path.
- Optional simplified polygons if SVG path hit testing is too slow.
- Point-in-country hit tests.
- Distance to target country/polygon approximation for Pinpoint scoring.

### 4. Obscurity tiers

ArevData does not appear to define game-specific obscurity tiers. Derive and override them locally.

Countries can use a weighted heuristic:

- Population.
- Area.
- Commonness/manual override.
- Recognized state vs territory/disputed.
- Tiny island/tiny land area.

Cities can use:

- Capital status.
- Population.
- Country prominence.
- Manual override for tourist/global cities.

Generated fields:

```json
{
  "alpha2": "MT",
  "obscurityTier": 3,
  "isTinyCountry": true,
  "isIslandCountry": true,
  "isCommonLearningCountry": false
}
```

### 5. Game Center ID config

Game Center IDs are not data. Keep them in app config.

### 6. App-specific indexes

Generate indexes for fast game startup:

- countries by continent.
- countries by recognized state.
- countries by flag availability.
- countries by map availability.
- countries by shape availability.
- countries by obscurity tier.
- cities by country.
- capitals by country.
- cities by obscurity tier.

## Generated schema proposal

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
    "svgPath": "flags/svg/mt.svg",
    "pngPath1x": "flags/png/mt@1x.png",
    "pngPath2x": "flags/png/mt@2x.png",
    "pngPath3x": "flags/png/mt@3x.png",
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
  "countriesWithFlags": ["MT", "AM", "NL"],
  "countriesWithMapPaths": ["MT", "AM", "NL"],
  "countriesWithCapitalCities": ["MT", "AM", "NL"],
  "countriesByContinent": {
    "Europe": ["MT", "AM", "NL"]
  },
  "countriesByObscurityTier": {
    "1": ["US", "FR", "JP"],
    "2": [],
    "3": ["MT", "AM"],
    "4": []
  }
}
```

## Export script requirements

The export script should:

1. Install/use `@arevs/data` or focused `@arevs/*` packages.
2. Read countries, flags, cities, geography, and map path data.
3. Normalize country codes.
4. Validate all referenced country codes exist.
5. Validate all app-required countries have flags.
6. Validate all app-required countries have geography.
7. Validate all country modes can generate enough answer options.
8. Validate all Pinpoint official modes have sufficient map/city data.
9. Generate JSON in stable sorted order.
10. Copy/generate local flag assets.
11. Write export metadata.

### export metadata

```json
{
  "generatedAt": "2026-07-08T00:00:00Z",
  "arevPackageVersion": "x.y.z",
  "source": "@arevs/data",
  "countryCount": 195,
  "flagCount": 195,
  "cityCount": 430,
  "worldMapCountryCount": 211
}
```

## Validation rules

Fail export if:

- A flag references an unknown country.
- A similar-flag code is unknown and not intentionally allowed as historical/territory data.
- A country in an MVP mode has no display name.
- A country in an MVP mode has no flag.
- A country in a map mode has no map path.
- A capital mode country has no capital city coordinate.
- A generated mode has fewer than 4 possible answers.
- Duplicate country codes exist.
- JSON output is not stable between runs with unchanged input.

Warn, but do not fail, if:

- A country has no population-derived obscurity score and uses manual fallback.
- A city has no population.
- A country has a complex geometry that is simplified for hit testing.
- A territory appears in the map data but not the primary countries list.

## Runtime loading requirement

The app must load data at startup from bundled resources.

No gameplay-critical data should require:

- Network.
- ArevData hosted API.
- Remote flag image URLs.
- Remote map files.

## Future data tasks

- Add manual country obscurity overrides.
- Add manual city obscurity overrides.
- Add localizable country-name display names.
- Add map geometry simplification output if SVG path hit testing is too slow.
- Add a data-diff report for ArevData updates.
- Add visual flag asset validation screenshots.
