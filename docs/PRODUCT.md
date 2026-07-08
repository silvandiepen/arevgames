# Product Scope

ArevGames is a family of small geography-learning games for iOS.

The point is not to build a complete atlas. The point is to use ArevData to make focused, polished, native games that teach one thing at a time.

## Brand structure

- `ArevData`: data library and API/site.
- `ArevGames`: iOS game repository and shared game harness.
- `ArevKit`: shared Swift/SwiftUI framework used by all games.
- `Arev Flags`, `Arev Pinpoint`, etc.: individual app products.

## Product principles

- Small apps.
- Shared harness.
- Native iOS quality.
- Offline gameplay.
- Calm learning.
- No ads.
- No subscriptions.
- No accounts.
- No required onboarding.
- No unnecessary network calls.
- Game Center only for scores/achievements.
- Settings that matter, not settings for everything.

## Non-goals

Do not build these in the first phase:

- A giant atlas app.
- A web app.
- Android.
- Multiplayer.
- User-generated content.
- Accounts.
- Analytics dashboards.
- In-app purchases.
- Lesson plans.
- Complex school LMS features.
- Timed competitive modes.
- Full localization infrastructure beyond preparing strings properly.

## App boundary rule

An app may have multiple modes, but the answer type and learning goal should stay coherent.

Good:

- Arev Flags has multiple flag-learning modes.
- Arev Pinpoint has multiple map-location modes.
- Arev Guess the Country has multiple clues that all answer with a country.

Bad:

- Arev Flags also has capitals, currencies, map tapping, and city pinpointing.
- Arev Pinpoint also includes flag matching.
- Arev Guess the Country becomes a complete geography trivia app.

## Planned app order

### 1. Arev Flags

First app. Lowest risk and strongest App Store search fit.

Modes:

- Flag to Country.
- Country to Flag.
- Flag Grid.
- Flag Match.
- Study Flags.
- Practice Misses.

### 2. Arev Pinpoint

Second app. Most distinctive gameplay.

Modes:

- Find the Country.
- Find the Capital.
- Find the City.
- No Borders.
- Obscure Places.
- Practice Misses.

### 3. Arev Guess the Country

Third app. Uses the breadth of ArevData without becoming too broad.

Modes:

- By Flag.
- By Shape.
- By Capital.
- By Map Clue.
- Mixed.

### 4. Arev Map Tap

Fourth app. Simpler and more kid-friendly than Pinpoint.

Modes:

- Find Country.
- Find Continent Countries.
- No Borders Lite.
- Practice Misses.

## Pricing assumption

Default to paid once per app.

Recommended starting price range: low paid app pricing, such as €1.99-€3.99 depending on final polish and market fit.

Do not add ads or subscriptions. If a free version is desired later, make a separate deliberate product decision.

## Game tone

The games should feel like calm native toys, not school software.

- Clear questions.
- Large answers.
- Immediate but non-annoying feedback.
- No shame language.
- No forced streak pressure.
- No flashing effects.
- Progress should be useful, not manipulative.

## Shared progression philosophy

Each app should track:

- Completed rounds.
- Best official preset scores.
- Correct/wrong item history.
- Weak countries/flags/cities.
- Study completion where applicable.
- Achievements where Game Center is available.

Progress must remain local-first. Game Center mirrors official achievements/scores, not the entire user state.

## First release scope

The first public release should include:

- Shared ArevKit foundation.
- Generated bundled data.
- Arev Flags app.
- Local scores and progress.
- At least a small set of Game Center achievements.
- At least one leaderboard for an official mode.
- App Store metadata and screenshots.

Pinpoint should not block the first Arev Flags release.
