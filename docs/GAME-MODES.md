# Game Mode Catalog

This document defines the planned modes across all initial ArevGames apps.

## Shared mode settings

All modes may use these settings when relevant:

| Setting | Values | Notes |
| --- | --- | --- |
| Question count | 5, 10, 25, 50 | Official Game Center presets should use fixed counts only |
| Region | World, continent, custom set | Start with World + continents |
| Obscurity | Common, mixed, obscure, brutal | Use generated obscurity tiers |
| Recognized only | On/off | Default on |
| Include territories | On/off | Default off unless app explicitly supports it |
| Show borders | On/off | Map modes only |
| Show labels | Off, continent labels, country labels | Map modes only; default off |
| Immediate feedback | On/off | Default on |
| Sound | On/off | Default on |
| Haptics | On/off | Default on |
| Reduce motion | System/default/on | Respect iOS Reduce Motion |
| Color-blind assist | On/off | Use patterns/labels when colors are meaningful |

## App: Arev Flags

Objective: learn and recognize flags.

Answer types: country or flag.

### Mode: Flag to Country

Prompt: show one flag.

Answer: select country name from multiple-choice grid.

Question source:

- `flags.json`.
- `countries.json`.
- similar flag groups for distractors.

Scoring:

- 1000 points for correct answer.
- 0 points for wrong answer.
- Optional streak bonus may be local-only; do not use for Game Center unless documented later.

Distractor priority:

1. Similar flags.
2. Same continent.
3. Same dominant flag colors.
4. Random valid countries.

### Mode: Country to Flag

Prompt: show one country name.

Answer: select the matching flag.

Question source:

- `countries.json`.
- `flags.json`.

Scoring:

- 1000 points for correct answer.
- 0 points for wrong answer.

Distractor priority:

1. Similar flags to the correct country.
2. Same continent.
3. Same dominant flag colors.
4. Random valid flags.

### Mode: Flag Grid

Prompt: show country name.

Answer: tap the correct flag in a larger visual grid.

Default grid sizes:

- Easy: 2x2.
- Normal: 3x3.
- Hard: 4x4.

Scoring:

- 1000 points for correct first tap.
- 500 points if second tap is allowed in practice mode.
- Official score modes should allow one tap only.

### Mode: Flag Match

Prompt: match country names with flags.

Answer: pair cards.

Round size:

- 4 pairs easy.
- 6 pairs normal.
- 8 pairs hard.

Scoring:

- Prefer local score only in first release.
- If official later, score can be based on attempts: fewer mistakes = higher score.

### Mode: Study Flags

Prompt: browse/study flags by region or weak items.

Answer: none.

Purpose:

- Learn before playing.
- Show flag, country name, continent, colors, and similar flags.

Scoring:

- No score.
- Track studied items locally.

### Mode: Practice Misses

Prompt: same as Flag to Country or Country to Flag.

Answer: same as selected practice mechanic.

Question source:

- Local weak-item history.

Scoring:

- Local only.
- Do not submit practice results to Game Center.

## App: Arev Guess the Country

Objective: identify a country from a clue.

Answer type: country.

### Mode: By Flag

Prompt: show one flag.

Answer: select country.

Uses the same generation as Arev Flags / Flag to Country, but inside a mixed country-identification app.

### Mode: By Shape

Prompt: show a standalone country outline.

Answer: select country.

Question source:

- `world-map.json` country paths.
- `countries.json`.

Distractors:

1. Same continent.
2. Similar area.
3. Nearby countries.
4. Random valid countries.

Scoring:

- 1000 correct.
- 0 wrong.

### Mode: By Capital

Prompt: show capital city name.

Answer: select country.

Question source:

- `cities.json` with `capital: true`.
- `countries.json`.

Distractors:

1. Same continent.
2. Countries with similarly known capitals.
3. Random valid countries.

Scoring:

- 1000 correct.
- 0 wrong.

### Mode: By Map Clue

Prompt: show world map with one country highlighted or pulsing.

Answer: select country.

Question source:

- `world-map.json`.
- `countries.json`.

Settings:

- Borders on/off.
- Labels off by default.

Scoring:

- 1000 correct.
- 0 wrong.

### Mode: Mixed

Prompt: random clue from enabled clue types.

Answer: select country.

Default clue types:

- Flag.
- Shape.
- Capital.
- Map clue.

Do not add currency/language/TLD until the first release is stable.

## App: Arev Pinpoint

Objective: learn where countries, capitals, and cities are located.

Answer type: map point.

### Mode: Find the Country

Prompt: country name.

Answer: tap on the world map.

Target:

- Country polygon/shape.

Scoring:

- 1000 if tap is inside the country shape.
- Otherwise score based on distance to the target shape or fallback centroid.
- Tiny countries need special tolerance.

Settings:

- Borders on/off.
- Labels off/country labels.
- Region filter.
- Question count.
- Obscurity.

### Mode: Find the Capital

Prompt: capital name and country, or capital name only on harder settings.

Answer: tap on the world map.

Target:

- Capital city coordinate.

Scoring:

- Distance from tap coordinate to target coordinate.
- Stricter than country mode.

Prompt variants:

- Easy: `Tap Valletta, Malta`.
- Normal: `Tap Valletta`.
- Hard: `Tap the capital of Malta`.

### Mode: Find the City

Prompt: city name and country, or city name only on harder settings.

Answer: tap on the world map.

Target:

- City coordinate.

Question source:

- `cities.json`.

Default first release scope:

- Major cities and capitals only.
- Avoid very obscure cities until city tiers are reliable.

### Mode: No Borders

Prompt: country/capital/city depending on preset.

Answer: tap on a plain map with borders hidden.

Scoring:

- Same as underlying mode.

Game Center:

- Only official fixed presets.

### Mode: Obscure Places

Prompt: country/capital/city filtered to higher obscurity tiers.

Answer: tap on map.

Scoring:

- Same as underlying mode.

Use local-only until the item pool is validated.

### Mode: Practice Misses

Prompt: weak items from local history.

Answer: tap on map.

Scoring:

- Local only.

## App: Arev Map Tap

Objective: simple country recognition on a map.

Answer type: country region tap.

This is simpler than Pinpoint. It should not use distance scoring by default.

### Mode: Find Country

Prompt: country name.

Answer: tap the country shape.

Scoring:

- Correct: 1000.
- Wrong: 0.

Feedback:

- Highlight correct country after answer.
- If wrong, briefly show tapped country name.

### Mode: Continent Focus

Prompt: country name from selected continent.

Answer: tap country on world map or continent viewport.

Scoring:

- Correct/wrong.

### Mode: No Borders Lite

Prompt: country name.

Answer: tap country on map with borders hidden or reduced.

Scoring:

- Correct/wrong.

### Mode: Practice Misses

Prompt: weak countries.

Answer: tap country.

Scoring:

- Local only.

## Future app candidates

### Arev Capitals

Objective: learn capitals.

Possible modes:

- Country to Capital.
- Capital to Country.
- Capital Grid.
- Capital Map Tap.

Do not build until the first four-app foundation is useful.

### Arev Shapes

Objective: learn country outlines.

Possible modes:

- Shape to Country.
- Country to Shape.
- Shape Grid.
- Shape Study.

This may be folded into Guess the Country unless App Store/search reasons justify a separate app.

### Arev Neighbours

Objective: learn bordering countries.

Possible modes:

- Pick a neighbor.
- Which country borders both?
- Landlocked challenge.
- Border count challenge.

This depends on neighbor data and should wait.
