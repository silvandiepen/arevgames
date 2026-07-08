# Arev Flags

Arev Flags is the first ArevGames app.

## Objective

Help players learn and recognize world flags.

## App boundary

Allowed:

- Flag to country.
- Country to flag.
- Flag grid.
- Flag matching.
- Study flags.
- Practice weak flags.

Not allowed:

- Capital quizzes.
- Map pinpointing.
- Country shape quizzes.
- Currency/language/timezone trivia.
- General atlas screens.

## App folder

```txt
apps/arev-flags/
```

## Shared dependencies

Use ArevKit for app shell, mode picker, round flow, answer grids, study UI, settings, progress, scoring, Game Center, and flag data loading.

The app target should only register modes and provide app-specific config.

## Data requirements

Required generated data:

- `countries.json`
- `flags.json`
- `app-indexes.json`
- local flag assets

Required ArevData fields:

- country alpha-2.
- country name.
- continent.
- recognized.
- flag asset.
- flag colors.
- similar flags.

## Modes

### Flag to Country

Prompt: `Which country is this flag from?`

Display: large flag card and 4 country answer cards by default.

Distractors:

1. similar flags.
2. same continent.
3. same flag colors.
4. random valid countries.

Scoring: correct 1000, wrong 0.

### Country to Flag

Prompt: `Which flag belongs to Malta?`

Display: country name and 4 flag answer cards.

Distractors:

1. similar flags.
2. same continent.
3. same colors.
4. random valid flags.

Scoring: correct 1000, wrong 0.

### Flag Grid

Prompt: `Find the flag of Armenia.`

Grid sizes:

- easy: 2x2.
- normal: 3x3.
- hard: 4x4.

Official mode should use one tap only.

### Flag Match

Prompt: `Match each flag with its country.`

Round sizes:

- 4 pairs.
- 6 pairs.
- 8 pairs.

First release may keep this local-only.

### Study Flags

Browse by continent, search countries, show flag, country name, continent, similar flags, and studied/progress state.

No scoring.

### Practice Misses

Uses weak-item history. No Game Center submission.

## Settings

Relevant settings:

- question count.
- region.
- obscurity.
- recognized only.
- include territories later.
- sound.
- haptics.
- reduce motion.

Hide irrelevant map settings.

## Official presets

| Preset | Mode | Questions | Region | Leaderboard ID |
| --- | --- | ---: | --- | --- |
| World Flags 10 | Flag to Country | 10 | World | `arev.flags.flag_to_country.world.10` |
| Country to Flag 10 | Country to Flag | 10 | World | `arev.flags.country_to_flag.world.10` |

## Achievements

Suggested:

- first round.
- first correct.
- first perfect round.
- 25 flags correct.
- 100 flags correct.
- similar flag solved.
- all continents practiced.

## MVP cut

Minimum first playable version:

- Flag to Country.
- Country to Flag.
- Study Flags.
- local progress.
- basic settings.
- local scores.

Next:

- Game Center official presets.
- Practice Misses.
- Flag Grid.
- Flag Match.
