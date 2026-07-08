# Game Mode Catalog

This file defines the initial modes for all planned ArevGames apps.

## Shared settings

Applicable settings:

- Question count: 5, 10, 25, 50.
- Region: World or continent.
- Obscurity: common, mixed, obscure, brutal.
- Recognized only: on/off.
- Include territories: future, default off.
- Show borders: map modes only.
- Show labels: map modes only.
- Immediate feedback: default on.
- Sound/haptics: on/off.
- Reduce Motion: respect system setting.
- Color-blind assist: on/off.

Only fixed official presets should submit to Game Center. Custom settings save local scores only.

## Arev Flags

Objective: learn flags.

### Flag to Country

Show one flag. Select the correct country name.

Distractors:

1. Similar flags.
2. Same continent.
3. Same dominant flag colors.
4. Random valid countries.

Scoring: correct 1000, wrong 0.

### Country to Flag

Show one country name. Select the correct flag.

Distractors:

1. Similar flags for the target country.
2. Same continent.
3. Same dominant flag colors.
4. Random valid flags.

Scoring: correct 1000, wrong 0.

### Flag Grid

Show a country name. Player taps the correct flag in a larger visual grid.

Grid sizes:

- Easy: 2x2.
- Normal: 3x3.
- Hard: 4x4.

Official scoring should use one tap only.

### Flag Match

Match flags to country names.

Round sizes:

- Easy: 4 pairs.
- Normal: 6 pairs.
- Hard: 8 pairs.

First release can keep this local-only.

### Study Flags

Browse flags by region, search, or weak items. No score. Track studied items locally.

### Practice Misses

Practice weak flags from local history. Never submits to Game Center.

## Arev Guess the Country

Objective: identify the country from different clues.

Answer type: country.

### By Flag

Show flag. Select country.

### By Shape

Show country outline. Select country.

Distractors:

1. Same continent.
2. Similar area.
3. Nearby countries.
4. Random valid countries.

### By Capital

Show capital. Select country.

Prompt variants:

- Easy: `Which country has Valletta as its capital?`
- Normal/hard: `Valletta`.

### By Map Clue

Show world map with one highlighted country. Select country.

Settings:

- Borders on/off.
- Labels off by default.

### Mixed

Random clue type from enabled clue types: flag, shape, capital, map clue.

Do not add currency, language, timezone, TLD, or phone-code clues until after the first stable release.

## Arev Pinpoint

Objective: locate places on a map.

Answer type: map point.

### Find the Country

Prompt: country name. Player taps on the world map.

Target: country polygon/shape.

Scoring:

- 1000 if tap is inside target country.
- Otherwise distance-based score.

### Find the Capital

Prompt variants:

- `Tap Valletta, Malta`.
- `Tap Valletta`.
- `Tap the capital of Malta`.

Target: capital coordinate.

Scoring: distance-based.

### Find the City

Prompt: city name and optionally country.

Target: city coordinate.

Use only validated city tiers in first release. Start with major cities and capitals.

### No Borders

Same as underlying country/capital/city mode, but map borders are hidden or heavily muted.

### Obscure Places

Uses harder obscurity filters. Local-only until validated.

### Practice Misses

Weak items from local history. Local-only.

## Arev Map Tap

Objective: simple country recognition on a map.

Answer type: country tap.

This is simpler than Pinpoint and does not use distance scoring by default.

### Find Country

Prompt: country name. Player taps the correct country.

Scoring: correct 1000, wrong 0.

### Continent Focus

Prompt countries from selected continent.

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
