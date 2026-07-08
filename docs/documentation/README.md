# ArevGames Documentation

This folder is the single source for product, architecture, app, mode, and agent documentation.

Do not scatter long-form specs across the repository. Keep Markdown documentation here unless the file is a conventional root-level file such as `README.md`, `AGENTS.md`, `LICENSE.md`, or `CONTRIBUTING.md`.

## Index

### Project and repository

- [`PRODUCT.md`](./PRODUCT.md) — product scope, app boundaries, and release order.
- [`MONOREPO.md`](./MONOREPO.md) — clean monorepo structure and ownership rules.
- [`AGENT_BUILD_PLAN.md`](./AGENT_BUILD_PLAN.md) — phased build plan for coding agents.

### Shared kit and systems

- [`AREVKIT.md`](./AREVKIT.md) — shared Swift/SwiftUI framework responsibilities.
- [`DATA.md`](./DATA.md) — ArevData audit, export pipeline, generated schemas, and gaps.
- [`GAME_MODES.md`](./GAME_MODES.md) — complete game mode catalog.
- [`GAME_SYSTEMS.md`](./GAME_SYSTEMS.md) — shared round, scoring, settings, progress, and feedback systems.
- [`GAME_CENTER.md`](./GAME_CENTER.md) — Game Center rules, leaderboards, achievements, and offline behavior.
- [`UI.md`](./UI.md) — design system, screens, and component rules.
- [`PRIVACY_ACCESSIBILITY.md`](./PRIVACY_ACCESSIBILITY.md) — privacy, child-safe defaults, accessibility, and App Store review implications.
- [`TESTING_CI.md`](./TESTING_CI.md) — testing matrix and CI requirements.
- [`APP_STORE.md`](./APP_STORE.md) — App Store metadata, review notes, and release rules.

### App specs

- [`apps/AREV_FLAGS.md`](./apps/AREV_FLAGS.md)
- [`apps/AREV_GUESS_THE_COUNTRY.md`](./apps/AREV_GUESS_THE_COUNTRY.md)
- [`apps/AREV_PINPOINT.md`](./apps/AREV_PINPOINT.md)
- [`apps/AREV_MAP_TAP.md`](./apps/AREV_MAP_TAP.md)

## Documentation placement rules

Use this structure:

```txt
docs/
  documentation/
    README.md
    PRODUCT.md
    MONOREPO.md
    AREVKIT.md
    DATA.md
    GAME_MODES.md
    GAME_SYSTEMS.md
    GAME_CENTER.md
    UI.md
    PRIVACY_ACCESSIBILITY.md
    TESTING_CI.md
    APP_STORE.md
    AGENT_BUILD_PLAN.md
    apps/
      AREV_FLAGS.md
      AREV_GUESS_THE_COUNTRY.md
      AREV_PINPOINT.md
      AREV_MAP_TAP.md
```

Do not create extra spec folders such as `docs/specs`, `documentation`, `design-docs`, `notes`, `planning`, or `architecture`.

## Root-level docs allowed

Only these Markdown files should usually exist at repo root:

- `README.md`
- `AGENTS.md`
- `CONTRIBUTING.md` if needed later
- `LICENSE.md` if needed later

## Agent rule

When an agent needs to document a decision, it should update the closest existing file in this folder. Only create a new documentation file when the topic is clearly large enough to deserve its own stable home.
