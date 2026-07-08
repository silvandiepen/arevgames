# Arev Guess the Country

Arev Guess the Country is a clue-based country identification game.

## Objective

Help players identify countries from different clue types. Every mode answers the same core question: which country is this?

## App boundary

Allowed:

- guess country by flag.
- guess country by shape.
- guess country by capital.
- guess country by highlighted map clue.
- mixed clue rounds.

Not allowed in first release:

- currency clues.
- language clues.
- timezone clues.
- phone-code clues.
- broad trivia.
- map pinpointing.
- flag matching.

Those can be future clue packs if the app stays focused.

## App folder

```txt
apps/arev-guess-the-country/
```

## Shared dependencies

Use ArevKit for clue rendering, answer grids, round flow, question generation, distractors, scoring, data loading, map/shape rendering, settings, progress, and Game Center.

## Data requirements

Required generated data:

- `countries.json`
- `flags.json`
- `cities.json`
- `geography.json`
- `world-map.json`
- `app-indexes.json`
- local flag assets

Required fields:

- country name.
- alpha-2 code.
- continent.
- recognized.
- flag asset.
- capital city.
- capital coordinate.
- map path/shape.
- obscurity tier.

## Modes

### By Flag

Prompt: `Which country is this?`

Display: flag.

Answer: country.

Same generator as Arev Flags / Flag to Country.

### By Shape

Prompt: `Which country has this shape?`

Display: standalone country outline.

Answer: country.

Distractors:

1. same continent.
2. similar area.
3. nearby countries.
4. random valid countries.

### By Capital

Prompt variants:

- `Which country has Valletta as its capital?`
- `Valletta`

Answer: country.

Distractors:

1. same continent.
2. similar obscurity.
3. nearby countries.
4. random valid countries.

### By Map Clue

Prompt: `Which country is highlighted?`

Display: world map with one country highlighted.

Answer: country.

Settings:

- borders on/off.
- labels off by default.

### Mixed

Random clue type from enabled clue types.

Initial clue types:

- flag.
- shape.
- capital.
- map clue.

The round should balance clue types where possible.

## Scoring

All first-release modes:

- correct: 1000.
- wrong: 0.

No time score.

## Settings

Relevant settings:

- question count.
- region.
- obscurity.
- recognized only.
- show borders for map clue.
- sound.
- haptics.
- reduce motion.

## Official presets

| Preset | Mode | Questions | Region | Leaderboard ID |
| --- | --- | ---: | --- | --- |
| Mixed World 10 | Mixed | 10 | World | `arev.guesscountry.mixed.world.10` |
| Shapes World 10 | By Shape | 10 | World | `arev.guesscountry.shape.world.10` |
| Capitals World 10 | By Capital | 10 | World | `arev.guesscountry.capital.world.10` |

Do not add too many leaderboards early.

## Feedback

Correct:

- show country name.
- show flag/shape/map recap.

Wrong:

- show selected country.
- show correct country.
- show brief extra clue if available.

Example:

```txt
Malta is in Europe. Its capital is Valletta.
```

## MVP cut

Minimum first playable version:

- By Flag.
- By Shape.
- By Capital.
- local score/progress.

Next:

- By Map Clue.
- Mixed.
- Game Center official presets.
- Practice Misses if useful.
