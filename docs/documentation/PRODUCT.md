# Product Scope

ArevGames is a family of small native iOS geography-learning games powered by ArevData.

This is not one large atlas product. It is a clean monorepo with several focused apps, a shared ArevKit library, generated local data, and a controlled documentation structure.

## Brand structure

- `ArevData`: source data library/API/site.
- `ArevGames`: this games monorepo.
- `ArevKit`: shared Swift/SwiftUI game harness in `libs/ArevKit`.
- Individual apps: focused products in `apps/`.

## Product rules

- One app = one learning objective.
- Modes may vary the practice mechanic, but must support the same objective.
- Shared functionality belongs in ArevKit.
- App targets should be thin.
- Gameplay must work offline.
- Game Center is optional and only for official score modes.

## Principles

- Native iOS first.
- Small apps.
- Shared implementation.
- Paid once by default.
- No ads.
- No subscriptions.
- No account requirement.
- No tracking.
- No timer pressure in first releases.
- Calm learning, not noisy gamification.
- Large tappable UI.
- Useful progress and practice misses.

## Non-goals

Do not build these in the first phase:

- A single giant atlas app.
- A web app.
- Android.
- Multiplayer.
- Accounts.
- Analytics dashboards.
- In-app subscriptions.
- School/LMS features.
- User-generated content.
- Timed competitive modes.
- Complex custom map editor.

## Planned apps

### 1. Arev Flags

Objective: learn flags.

Modes:

- Flag to Country.
- Country to Flag.
- Flag Grid.
- Flag Match.
- Study Flags.
- Practice Misses.

This should be the first vertical slice.

### 2. Arev Pinpoint

Objective: learn where countries, capitals, and cities are.

Modes:

- Find the Country.
- Find the Capital.
- Find the City.
- No Borders.
- Obscure Places.
- Practice Misses.

This is the most distinctive score-based map game.

### 3. Arev Guess the Country

Objective: identify countries from different clues.

Modes:

- By Flag.
- By Shape.
- By Capital.
- By Map Clue.
- Mixed.

This uses the breadth of ArevData, but still answers only one question: which country is this?

### 4. Arev Map Tap

Objective: simple country recognition on a map.

Modes:

- Find Country.
- Continent Focus.
- No Borders Lite.
- Practice Misses.

This is simpler and more kid-friendly than Pinpoint.

## Future app candidates

Only build later if the first apps work well:

- Arev Capitals.
- Arev Shapes.
- Arev Neighbours.
- Arev Space.

## Pricing assumption

Default model: paid once per app.

Recommended range for first apps: low paid app pricing. Do not add ads or subscriptions.

A free/lite strategy can be evaluated later, but it should not complicate MVP implementation.

## Progress philosophy

Each app should track locally:

- Completed rounds.
- Local high scores.
- Correct/wrong item history.
- Weak countries/flags/cities.
- Study completion where relevant.
- Official Game Center submissions where relevant.

Progress exists to help learning. It should not become manipulative streak pressure.

## First release scope

First release target:

- Clean monorepo.
- ArevKit foundation.
- Exported bundled data.
- Arev Flags app.
- Local scores and progress.
- Game Center adapter behind a protocol.
- At least one official leaderboard-ready preset.
- App Store metadata and screenshots.

Pinpoint should not block the first Arev Flags release.
