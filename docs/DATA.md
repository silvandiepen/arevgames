# Data Requirements

ArevGames uses ArevData as the source of truth, but the games must run from bundled local data/assets.

The hosted ArevData API is useful for docs and demos. It must not be required during gameplay.

## ArevData audit

Current ArevData coverage is enough for the planned first apps.

| Game need | ArevData source | Used by |
| --- | --- | --- |
| Countries | `countries`, names, ISO codes, capitals, continents, currencies, languages, TLD, recognized flag | All apps |
| Flags | `flagData`, flag colors, similar flag groups, SVG/PNG URL helpers | Arev Flags, Guess the Country |
| Similar flags | `getSimilarFlags()` / `FlagInfo.similar` | Flag distractors |
| Geography | `countryGeography`, centroid, bounds, area, landlocked, neighbors, climate, avg temperature | Pinpoint, Map Tap, Guess the Country |
| Geo helpers | `haversineDistance`, bearings, geo hints | Pinpoint scoring and future hint modes |
| Cities | 430+ cities, all national capitals, coordinates, population, capital metadata | Pinpoint, Guess by Capital |
| World map | SVG paths, world viewBox, highlighting helpers | Pinpoint, Map Tap, map clues |
| Country shapes | standalone country SVG helper derived from world-map data | Guess by Shape |
| Places | sharded larger place data | Future city expansion only |
| Brand/logo | ArevData / arev website or source assets | Splash screens and website |

## Generated data

Generate into `data/generated/`:

```txt
data/generated/
  export-metadata.json
  countries.json
  flags.json
  cities.json
  geography.json
  world-map.json
  app-indexes.json
  assets/
    flags/svg/{alpha2}.svg
    flags/png/{alpha2}@1x.png
    flags/png/{alpha2}@2x.png
    flags/png/{alpha2}@3x.png
```

If SVG is used directly in Swift, PNG generation may be optional, but there must still be local bundled flag assets for gameplay.

## Manual source data and brand assets

Use `data/source/` for game-specific overrides and canonical source assets:

```txt
data/source/
  export-config.json
  manual-country-obscurity.json
  manual-city-obscurity.json
  app-mode-presets.json
  brand/
    arev-logo.svg
```

`data/source/brand/arev-logo.svg` should be copied/exported from the actual ArevData/arev logo asset. Do not redraw it manually.

The same source logo should feed:

- iOS splash/launch assets.
- website logo.
- favicon generation.
- any future ArevGames family mark.

Manual overrides should be small and reviewed. Prefer deriving from ArevData.

## Required schemas

### countries.json

Fields:

- `alpha2`
- `alpha3`
- `name`
- `nativeName`
- `capital`
- `continent`
- `currency`
- `currencyName`
- `currencySymbol`
- `languages`
- `tld`
- `recognized`

### flags.json

Fields:

- `alpha2`
- `assetName`
- `svgPath`
- `pngPath1x`
- `pngPath2x`
- `pngPath3x`
- `colors`
- `similar`

### cities.json

Fields:

- `id`
- `name`
- `country`
- `lat`
- `lon`
- `population`
- `capital`
- `capitalTypes`
- `obscurityTier`

### geography.json

Fields:

- `alpha2`
- `lat`
- `lon`
- `bounds`
- `area`
- `landlocked`
- `neighbors`
- `climate`
- `avgTemperature`
- `obscurityTier`
- `isTinyCountry`
- `isIslandCountry`

### world-map.json

Fields:

- `viewBox`
- `projection`
- country `alpha2`
- country `name`
- country SVG `paths`
- path bounds for quick hit-test rejection

### app-indexes.json

Include:

- countries with flags.
- countries with map paths.
- countries with capital cities.
- countries by continent.
- countries by obscurity tier.
- cities by country.
- cities by obscurity tier.

## Export tool

Create the exporter at `tools/export-arev-data/`.

Responsibilities:

1. Read ArevData packages.
2. Normalize country codes to uppercase alpha-2.
3. Generate compact Swift-friendly JSON.
4. Copy/generate local flag assets.
5. Copy/export the ArevData/arev logo asset.
6. Generate splash/favicon/logo derivative assets where needed.
7. Generate map path data.
8. Derive obscurity tiers.
9. Generate app indexes.
10. Validate all mode requirements.
11. Write stable sorted output.
12. Fail CI on invalid output.

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
- The required ArevData logo source asset is missing after brand asset export/copy is configured.

Warn if:

- A city has no population.
- A country/city uses manual obscurity fallback.
- SVG geometry is too complex and needs simplification.
- A map territory exists outside the main country list.

## Obscurity tiers

Use four tiers:

1. Common.
2. Regular.
3. Hard.
4. Obscure.

Country heuristic:

- population.
- area.
- continent distribution.
- recognized state vs territory.
- tiny/island country flags.
- manual overrides.

City heuristic:

- capital status.
- population.
- country tier.
- global/tourist significance override.

## Runtime rules

At runtime the app must:

- Load bundled resources.
- Work offline.
- Never require `api.arevdata.com`.
- Never require remote flag image URLs.
- Skip invalid questions instead of crashing production builds.
- Expose debug validation failures in development builds.
