# Arev Pinpoint

Arev Pinpoint is a map-location game.

## Objective

Help players learn where countries, capitals, and cities are by tapping their location on a map. The closer the tap is to the target, the better the score.

## App boundary

Allowed:

- tap country on map.
- tap capital on map.
- tap city on map.
- distance scoring.
- no-borders challenge.
- obscure-place challenge.
- practice weak places.

Not allowed:

- flag quizzes.
- capital multiple-choice quizzes.
- general atlas browsing.
- unrelated trivia.

## App folder

```txt
apps/arev-pinpoint/
```

## Shared dependencies

Use ArevKit for map rendering, map projection, tap handling, distance calculation, country hit testing, scoring, round flow, settings, progress, and Game Center.

## Data requirements

Required generated data:

- `countries.json`
- `cities.json`
- `geography.json`
- `world-map.json`
- `app-indexes.json`

Required fields:

- country name.
- alpha-2 code.
- continent.
- recognized.
- country centroid.
- country bounds.
- country map paths.
- capital city coordinates.
- city coordinates.
- obscurity tiers.

## Modes

### Find the Country

Prompt: `Tap Malta.`

Target: country polygon/shape.

Scoring:

- tap inside target country: 1000.
- tap outside: distance-based score.

If distance-to-polygon is not ready, use centroid distance temporarily and document the limitation in code/tests.

### Find the Capital

Prompt variants:

- `Tap Valletta, Malta.`
- `Tap Valletta.`
- `Tap the capital of Malta.`

Target: capital coordinate.

Perfect threshold suggestion: <= 25 km.

### Find the City

Prompt variants:

- `Tap Tokyo, Japan.`
- `Tap Tokyo.`

Target: city coordinate.

Use major cities/capitals first. Avoid very obscure cities until city tiers are validated.

### No Borders

Uses underlying country/capital/city mode but hides or heavily mutes borders.

### Obscure Places

Uses obscure country/city filters. Local-only until the data pool is validated.

### Practice Misses

Uses weak-item history. No Game Center submission.

## Map settings

Relevant settings:

- question count.
- region.
- obscurity.
- show borders.
- show labels.
- recognized only.
- sound.
- haptics.
- reduce motion.

## Scoring

```txt
score = max(0, round(1000 * (1 - distanceKm / maxDistanceKm)))
```

Suggested max distances:

| Target | maxDistanceKm |
| --- | ---: |
| Country outside polygon | 3000 |
| Capital | 1500 |
| City | 1000 |
| Tiny country | 500 |
| Island country | 750 |

Country targets should give 1000 when the tap is inside the country shape.

## Official presets

| Preset | Mode | Questions | Settings | Leaderboard ID |
| --- | --- | ---: | --- | --- |
| World Countries 10 | Find Country | 10 | world, borders on | `arev.pinpoint.countries.world.10` |
| World Capitals 10 | Find Capital | 10 | world, borders on | `arev.pinpoint.capitals.world.10` |
| Europe Countries 10 | Find Country | 10 | Europe, borders on | `arev.pinpoint.countries.europe.10` |
| No Borders World 10 | No Borders | 10 | world, borders off | `arev.pinpoint.no_borders.world.10` |

## Feedback

After a tap:

- show player tap.
- show correct target.
- show distance in km.
- show score for question.
- highlight correct country or target point.
- optionally draw a line from tap to target.

## Tiny country handling

Use zoom support, tap tolerance, special scoring tolerance, and tests for Malta, Monaco, San Marino, Vatican City, Liechtenstein, and Andorra.

## MVP cut

Minimum first playable version:

- world map renderer.
- Find the Country.
- distance feedback.
- local score.
- borders on/off.

Next:

- Find the Capital.
- Game Center official presets.
- No Borders.
- Find the City.
- Practice Misses.
