# ArevGames

Small native iOS geography games powered by ArevData.

ArevGames is not one large atlas app. It is a shared harness plus a set of focused games. Each app has one clear learning objective, while the shared `ArevKit` owns almost everything that should not be duplicated: app shell, UI, data adapters, scoring, progress, Game Center, settings, map rendering, accessibility, testing helpers, and app-store-safe defaults.

## Product rule

One app = one learning objective.

Modes may vary the mechanic, but they must support the same objective. Do not turn any app into a general geography suite.

## Planned apps

| App | Objective | Core mechanic |
| --- | --- | --- |
| Arev Flags | Learn and recognize world flags | Select country from flag, select flag from country, flag grid, flag match, study mode |
| Arev Guess the Country | Identify countries from different clues | Guess the country by flag, shape, capital, or map clue |
| Arev Pinpoint | Learn where places are | Given a country, capital, or city, tap its location on a map; closer taps score higher |
| Arev Map Tap | Simple map recognition | Tap the requested country on a map; kid-friendly and less score-heavy than Pinpoint |

Future apps may include Arev Capitals, Arev Shapes, or Arev Neighbours, but they should not be started until the shared harness and first apps are stable.

## Principles

- Native iOS first.
- Paid once or simple purchase model.
- No ads.
- No subscriptions.
- No account requirement.
- No tracking.
- Offline gameplay.
- Game Center for official score modes only.
- Calm learning, not timer pressure.
- Small apps, shared implementation.
- ArevData is the source of truth for countries, flags, maps, cities, capitals, geography, and related metadata.

## Source data

Use ArevData packages directly during the build/export process. The hosted API is not the runtime dependency for the games.

Primary package candidates:

- `@arevs/data` for full aggregate export.
- `@arevs/geo` for countries, cities, geography, states, timezones, currencies, languages.
- `@arevs/flags` for flag metadata, SVG/PNG URLs, colors, and similar flags.
- `@arevs/maps` for world-map and country-map SVG helpers.
- `@arevs/places` only when a mode needs larger country-sharded place data.

The iOS apps should consume generated local JSON/assets, not call the hosted API during gameplay.

## Documentation index

Start here:

- [`AGENTS.md`](./AGENTS.md) — mandatory rules for coding agents.
- [`docs/PRODUCT.md`](./docs/PRODUCT.md) — product scope and app boundaries.
- [`docs/REPOSITORY.md`](./docs/REPOSITORY.md) — target monorepo structure.
- [`docs/AREVKIT.md`](./docs/AREVKIT.md) — shared framework responsibilities and core protocols.
- [`docs/DATA.md`](./docs/DATA.md) — ArevData audit, export pipeline, and missing data requirements.
- [`docs/GAME-MODES.md`](./docs/GAME-MODES.md) — complete mode catalog.
- [`docs/GAME-SYSTEMS.md`](./docs/GAME-SYSTEMS.md) — shared gameplay systems.
- [`docs/GAME-CENTER.md`](./docs/GAME-CENTER.md) — leaderboards, achievements, and official presets.
- [`docs/UI.md`](./docs/UI.md) — visual system and screens.
- [`docs/ACCESSIBILITY-PRIVACY.md`](./docs/ACCESSIBILITY-PRIVACY.md) — privacy, accessibility, and child-safe defaults.
- [`docs/TESTING-CI.md`](./docs/TESTING-CI.md) — testing and CI requirements.
- [`docs/APP-STORE.md`](./docs/APP-STORE.md) — App Store metadata and review notes.
- [`docs/AGENT-BUILD-PLAN.md`](./docs/AGENT-BUILD-PLAN.md) — phased build plan for agents.

App specs:

- [`docs/apps/AREV-FLAGS.md`](./docs/apps/AREV-FLAGS.md)
- [`docs/apps/AREV-GUESS-THE-COUNTRY.md`](./docs/apps/AREV-GUESS-THE-COUNTRY.md)
- [`docs/apps/AREV-PINPOINT.md`](./docs/apps/AREV-PINPOINT.md)
- [`docs/apps/AREV-MAP-TAP.md`](./docs/apps/AREV-MAP-TAP.md)

## First build target

Build the shared foundation and Arev Flags first:

1. Create the Xcode workspace and Swift packages.
2. Create `ArevKit` modules.
3. Create the data export pipeline from ArevData into bundled JSON/assets.
4. Build Arev Flags as the first app.
5. Add Arev Pinpoint after the map interaction layer is ready.
6. Add Arev Guess the Country after flag, shape, capital, and map clue renderers exist.
7. Add Arev Map Tap after Pinpoint, using the same map renderer with simpler scoring.
