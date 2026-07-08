# Arev Flags

Arev Flags is the first ArevGames app.

## Objective

Help players learn and recognize world flags.

## App boundary

This app is only about flags.

Allowed:

- Flag to country.
- Country to flag.
- Flag grid.
- Flag matching.
- Study flags.
- Practice weak flags.

Not allowed in this app:

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

Use ArevKit for:

- app shell.
- mode picker.
- round flow.
- answer grids.
- study UI.
- settings.
- progress.
- scoring.
- Game Center.
- flag data loading.

The app target should only register modes and provide app-specific config.

## Data requirements

Required generated data:

- `countries.json`
- `flags.json`
- `app-indexes.json`
- local flag assets

Required ArevData fields:

- country `alpha2`.
- country name.
- continent.
- recognized.
- flag asset.
- flag colors.
- similar flags.

## Modes

### 1. Flag to Country

Prompt:

```txt
Which country is this flag from?
```

Display:

- Large flag card.
- 4 country answer cards by default.

Answer:

- country.

Distractors:

1. similar flags.
2. same continent.
3. same flag colors.
4. random valid countries.

Scoring:

- correct: 1000.
- wrong: 0.

Game Center:

- official world 10-question preset.
- optional continent 10-question presets later.

### 2. Country to Flag

Prompt:

```txt
Which flag belongs to Malta?
```

Display:

- Country name.
- 4 flag answer cards.

Answer:

- flag.

Distractors:

1. similar flags.
2. same continent.
3. same colors.
4. random valid flags.

Scoring:

- correct: 1000.
- wrong: 0.

### 3. Flag Grid

Prompt:

```txt
Find the flag of Armenia.
```

Display:

- grid of flags.

Grid sizes:

- easy: 2x2.
- normal: 3x3.
- hard: 4x4.

Official mode should use one tap only.

### 4. Flag Match

Prompt:

```txt
Match each flag with its country.
```

Display:

- cards with flags.
- cards with country names.

Round sizes:

- 4 pairs.
- 6 pairs.
- 8 pairs.

First release may keep this local-only.

### 5. Study Flags

Display:

- browse by continent.
- search countries.
- show flag.
- show country name.
- show continent.
- show similar flags.
- show studied/progress state.

No scoring.

### 6. Practice Misses

Uses weak-item history.

Allowed mechanics:

- Flag to Country.
- Country to Flag.

No Game Center submission.

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

Irrelevant settings:

- show borders.
- show labels.

Hide irrelevant map settings.

## Official presets

Initial suggestions:

| Preset | Mode | Questions | Region | Leaderboard ID |
| --- | --- | ---: | --- | --- |
| World Flags 10 | Flag to Country | 10 | World | `arev.flags.flag_to_country.world.10` |
| Country to Flag 10 | Country to Flag | 10 | World | `arev.flags.country_to_flag.world.10` |

Add more only after the first release is stable.

## Achievements

Suggested achievements:

- first round.
- first correct.
- first perfect round.
- 25 flags correct.
- 100 flags correct.
- similar flag solved.
- all continents practiced.

## UI notes

- Flags should be large and crisp.
- Preserve aspect ratio.
- Do not crop flags in the main question card.
- Use local assets.
- Similar flags should be used quietly as distractors, not necessarily explained during official gameplay.
- Study mode can explain similar flags.

## MVP cut

Minimum first playable version:

- Flag to Country.
- Country to Flag.
- Study Flags.
- local progress.
- basic settings.
- local scores.

Next after MVP:

- Game Center official presets.
- Practice Misses.
- Flag Grid.
- Flag Match.
