# Game Mode Catalog

This file defines the initial modes for all planned ArevGames apps.

## Shared settings

Applicable settings:

| Setting | Values | Applies to |
| --- | --- | --- |
| Question count | 5, 10, 25, 50 | All quiz/round modes |
| Region | World, continent | All country/city modes |
| Obscurity | Common, mixed, obscure, brutal | All modes with item pools |
| Recognized only | on/off | Country modes |
| Include territories | on/off | Later; default off |
| Show borders | on/off | Map modes |
| Show labels | off, continent, country | Map modes |
| Immediate feedback | on/off | All round modes |
| Sound/haptics | on/off | All apps |
| Reduce motion | system/on | All apps |
| Color-blind assist | on/off | All apps |

Only fixed official presets should submit to Game Center. Custom settings save local scores only.

## Arev Flags

Objective: learn flags.

### Flag to Country

Show one flag. Select the correct country name.

Question source:

- `flags.json`
- `countries.json`
- similar flag groups

Answer type: country.

Distractors:

1. Similar flags.
2. Same continent.
3. Same dominant flag colors.
4. Random valid countries.

Scoring:

- Correct: 1000.
- Wrong: 0.

### Country to Flag

Show one country name. Select the correct flag.

Answer type: flag.

Distractors:

1. Similar flags for the target country.
2. Same continent.
3. Same dominant flag colors.
4. Random valid flags.

Scoring:

- Correct: 1000.
- Wrong: 0.

### Flag Grid

Show a country name. Player taps the correct flag in a larger visual grid.

Grid sizes:

- Easy: 2x2.
- Normal: 3x3.
- Hard: 4x4.

Official scoring:

- One tap only.
- Correct: 1000.
- Wrong: 0.

Practice scoring may allow a second tap locally only.

### Flag Match

Match flags to country names.

Round sizes:

- Easy: 4 pairs.
- Normal: 6 pairs.
- Hard: 8 pairs.

First release can keep this local-only.

### Study Flags

Browse flags by region, search, or weak items.

No score. Track studied items locally.

### Practice Misses

Practice weak flags from local history.

No Game Center submission.

## Arev Guess the Country

Objective: identify the country from different clues.

Answer type: country.

### By Flag

Show flag. Select country.

Same generator as Arev Flags / Flag to Country.

### By Shape

Show country outline. Select country.

Question source:

- `world-map.json`
- `countries.json`

Distractors:

1. Same continent.
2. Similar area.
3. Nearby countries.
4. Random valid countries.

### By Capital

Show capital. Select country.

Question source:

- `cities.json` where `capital = true`
- `countries.json`

Prompt variants:

- Easy: `Which country has Valletta as its capital?`
- Normal: `Valletta`
- Hard: capital only with harder distractors.

### By Map Clue

Show world map with one highlighted country. Select country.

Settings:

- Borders on/off.
- Labels off by default.

### Mixed

Random clue type from enabled clue types.

Initial clue types:

- Flag.
- Shape.
- Capital.
- Map clue.

Do not add currency, language, timezone, TLD, or phone-code clues until after the first stable release.

## Arev Pinpoint

Objective: locate places on a map.

Answer type: map point.

### Find the Country

Prompt: country name.

Player taps on the world map.

Target: country polygon/shape.

Scoring:

- 1000 if tap is inside target country.
- Otherwise distance-based score.

### Find the Capital

Prompt variants:

- Easy: `Tap Valletta, Malta`.
- Normal: `Tap Valletta`.
- Hard: `Tap the capital of Malta`.

Target: capital coordinate.

Scoring: distance-based.

### Find the City

Prompt: city name and optionally country.

Target: city coordinate.

Use only validated city tiers in first release. Start with major cities and capitals.

### No Borders

Same as underlying country/capital/city mode, but map borders are hidden or heavily muted.

Only fixed presets should submit to Game Center.

### Obscure Places

Uses harder obscurity filters.

Local-only until data is validated.

### Practice Misses

Uses weak items from local history.

Local-only.

## Arev Map Tap

Objective: simple country recognition on a map.

Answer type: country tap.

This is simpler than Pinpoint and does not use distance scoring by default.

### Find Country

Prompt: country name.

Player taps the correct country.

Scoring:

- Correct country: 1000.
- Wrong country: 0.

### Continent Focus

Prompt countries from selected continent.

Can zoom/fit the relevant continent if the map engine supports it.

### No Borders Lite

Simplified challenge with borders hidden/reduced.

### Practice Misses

Weak countries only. Local-only.

## Future modes

Future candidates:

- Country to Capital.
- Capital to Country.
- Shape to Country.
- Country to Shape.
- Pick a Neighbor.
- Landlocked Challenge.
- Border Count Challenge.

Do not build these before the first four apps and shared harness are stable.
