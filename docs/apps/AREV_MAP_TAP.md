# Arev Map Tap

Arev Map Tap is the simpler map-recognition app.

## Objective

Help players learn where countries are by tapping them on a map.

This app is more direct and kid-friendly than Arev Pinpoint. It uses correct/wrong country taps, not distance scoring by default.

## App boundary

Allowed:

- tap requested country.
- continent-focused map practice.
- no-borders lite mode.
- practice missed countries.

Not allowed:

- city/capital pinpoint distance scoring.
- flag quizzes.
- country-shape multiple choice.
- broad trivia.

## App folder

```txt
apps/arev-map-tap/
```

## Shared dependencies

Use ArevKit for map rendering, country hit testing, round flow, scoring, settings, progress, Game Center, and feedback.

## Data requirements

Required generated data:

- `countries.json`
- `geography.json`
- `world-map.json`
- `app-indexes.json`

Required fields:

- country name.
- alpha-2 code.
- continent.
- recognized.
- map paths.
- obscurity tier.

## Modes

### Find Country

Prompt: `Tap Armenia.`

Answer: country tap.

Scoring:

- correct country: 1000.
- wrong country: 0.

Feedback:

- highlight correct country.
- show tapped country name if wrong.

### Continent Focus

Prompt countries from selected continent.

Map behavior:

- fit/zoom to relevant continent if supported.
- otherwise use world map with non-relevant areas visually muted.

### No Borders Lite

Borders hidden or reduced. Correct/wrong scoring remains simple.

### Practice Misses

Weak countries from local history. No Game Center submission.

## Settings

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

Default:

- borders on.
- labels off.
- recognized only on.

## Official presets

| Preset | Mode | Questions | Region | Leaderboard ID |
| --- | --- | ---: | --- | --- |
| World Countries 10 | Find Country | 10 | World | `arev.maptap.countries.world.10` |
| Europe Countries 10 | Continent Focus | 10 | Europe | `arev.maptap.countries.europe.10` |
| No Borders World 10 | No Borders Lite | 10 | World | `arev.maptap.no_borders.world.10` |

## Difference from Arev Pinpoint

Arev Map Tap:

- correct/wrong.
- simpler.
- country targets only.
- more kid-friendly.
- no distance scoring by default.

Arev Pinpoint:

- distance score.
- countries, capitals, and cities.
- more competitive.
- stronger Game Center focus.

## Tiny country handling

Use the same map engine as Pinpoint. Tiny countries still need zoom, tap tolerance, target reveal, and possible special handling in official presets.

## MVP cut

Minimum first playable version:

- Find Country.
- world map.
- correct/wrong feedback.
- local score.

Next:

- Continent Focus.
- No Borders Lite.
- Practice Misses.
- Game Center official presets.
