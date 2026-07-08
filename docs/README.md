# ArevGames Docs

This folder is the single documentation root for ArevGames.

Do not create `docs/documentation/`, `documentation/`, `notes/`, `planning/`, `architecture/`, or other competing documentation roots.

## Index

Project and repository:

- [`PRODUCT.md`](./PRODUCT.md) — product scope and app boundaries.
- [`MONOREPO.md`](./MONOREPO.md) — required repo structure and anti-mess rules.
- [`AGENT_BUILD_PLAN.md`](./AGENT_BUILD_PLAN.md) — phased implementation plan for Codex/agents.

Shared systems:

- [`AREVKIT.md`](./AREVKIT.md) — shared Swift/SwiftUI kit responsibilities.
- [`DATA.md`](./DATA.md) — ArevData audit, export pipeline, generated schemas, and gaps.
- [`GAME_MODES.md`](./GAME_MODES.md) — complete mode catalog.
- [`GAME_SYSTEMS.md`](./GAME_SYSTEMS.md) — scoring, rounds, settings, progress, feedback.
- [`GAME_CENTER.md`](./GAME_CENTER.md) — leaderboards, achievements, official presets, offline behavior.
- [`UI.md`](./UI.md) — shared UI system, screens, components, map/flag rendering.
- [`PRIVACY_ACCESSIBILITY.md`](./PRIVACY_ACCESSIBILITY.md) — privacy, child-safe defaults, accessibility.
- [`TESTING_CI.md`](./TESTING_CI.md) — test matrix and CI rules.
- [`APP_STORE.md`](./APP_STORE.md) — App Store copy, release, review, privacy notes.

App specs:

- [`apps/AREV_FLAGS.md`](./apps/AREV_FLAGS.md)
- [`apps/AREV_PINPOINT.md`](./apps/AREV_PINPOINT.md)
- [`apps/AREV_GUESS_THE_COUNTRY.md`](./apps/AREV_GUESS_THE_COUNTRY.md)
- [`apps/AREV_MAP_TAP.md`](./apps/AREV_MAP_TAP.md)

## Documentation placement rule

Root-level Markdown should stay limited to conventional files such as `README.md`, `AGENTS.md`, `LICENSE.md`, and `CONTRIBUTING.md`.

All long-form product, architecture, app, and implementation docs belong in this `docs/` folder.

App specs belong in `docs/apps/`.
