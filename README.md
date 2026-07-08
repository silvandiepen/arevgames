# ArevGames

Small native iOS geography games powered by ArevData.

ArevGames is a clean monorepo for focused geography-learning apps. It is not one large atlas app. Each app has one clear learning objective, while the shared `ArevKit` library owns the reusable app shell, UI, data adapters, scoring, progress, Game Center, settings, map rendering, accessibility, testing helpers, and app-store-safe defaults.

## Product rule

One app = one learning objective.

Modes may vary the mechanic, but they must support the same objective. Do not turn any app into a general geography suite.

## Monorepo rule

Use this structure:

```txt
apps/                 # app targets only
libs/ArevKit/          # shared Swift/SwiftUI libraries
data/                 # source and generated local data
tools/                # repository tooling, including ArevData export
docs/                 # all long-form docs and specs
.github/workflows/    # CI
```

Do not create random top-level folders, extra documentation roots, duplicated shared components inside apps, or one-off helper folders.

## Planned apps

| App | Objective | Core mechanic |
| --- | --- | --- |
| Arev Flags | Learn and recognize world flags | Select country from flag, select flag from country, flag grid, flag match, study mode |
| Arev Pinpoint | Learn where places are | Given a country, capital, or city, tap its location on a map; closer taps score higher |
| Arev Guess the Country | Identify countries from different clues | Guess the country by flag, shape, capital, or map clue |
| Arev Map Tap | Simple map recognition | Tap the requested country on a map; kid-friendly and less score-heavy than Pinpoint |

Future apps may include Arev Capitals, Arev Shapes, Arev Neighbours, or Arev Space, but they should not be started until the shared harness and first apps are stable.

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
- [`docs/README.md`](./docs/README.md) — documentation index and placement rules.
- [`docs/PRODUCT.md`](./docs/PRODUCT.md) — product scope and app boundaries.
- [`docs/MONOREPO.md`](./docs/MONOREPO.md) — clean monorepo structure.
- [`docs/AREVKIT.md`](./docs/AREVKIT.md) — shared framework responsibilities and core protocols.
- [`docs/DATA.md`](./docs/DATA.md) — ArevData audit, export pipeline, and missing data requirements.
- [`docs/GAME_MODES.md`](./docs/GAME_MODES.md) — complete mode catalog.
- [`docs/GAME_SYSTEMS.md`](./docs/GAME_SYSTEMS.md) — shared gameplay systems.
- [`docs/GAME_CENTER.md`](./docs/GAME_CENTER.md) — leaderboards, achievements, and official presets.
- [`docs/UI.md`](./docs/UI.md) — visual system and screens.
- [`docs/PRIVACY_ACCESSIBILITY.md`](./docs/PRIVACY_ACCESSIBILITY.md) — privacy, accessibility, and child-safe defaults.
- [`docs/TESTING_CI.md`](./docs/TESTING_CI.md) — testing and CI requirements.
- [`docs/APP_STORE.md`](./docs/APP_STORE.md) — App Store metadata and review notes.
- [`docs/AGENT_BUILD_PLAN.md`](./docs/AGENT_BUILD_PLAN.md) — phased build plan for agents.

App specs:

- [`docs/apps/AREV_FLAGS.md`](./docs/apps/AREV_FLAGS.md)
- [`docs/apps/AREV_PINPOINT.md`](./docs/apps/AREV_PINPOINT.md)
- [`docs/apps/AREV_GUESS_THE_COUNTRY.md`](./docs/apps/AREV_GUESS_THE_COUNTRY.md)
- [`docs/apps/AREV_MAP_TAP.md`](./docs/apps/AREV_MAP_TAP.md)

## First build target

Build the shared foundation and Arev Flags first:

1. Create the documented monorepo folders.
2. Create the Xcode workspace and Swift packages.
3. Create `libs/ArevKit` modules.
4. Create the data export pipeline from ArevData into bundled JSON/assets.
5. Build Arev Flags as the first app.
6. Add Game Center behind a protocol.
7. Add Arev Pinpoint after the map interaction layer is ready.
8. Add Arev Guess the Country after flag, shape, capital, and map clue renderers exist.
9. Add Arev Map Tap using the same map renderer with simpler scoring.
