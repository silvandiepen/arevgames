# Agent Instructions

This repository is documentation-first. Agents should build from these documents without asking product-scope questions unless a hard technical blocker exists.

Before adding or changing code, read:

1. `README.md`
2. `docs/README.md`
3. `docs/MONOREPO.md`
4. `docs/AGENT_BUILD_PLAN.md`

## Hard rules

- Keep apps small.
- Keep the monorepo clean.
- Use `apps/` for app targets only.
- Use `libs/ArevKit/` for shared Swift/SwiftUI code.
- Use `data/` for source/generated data.
- Use `tools/` for repository tooling.
- Use `docs/` for all long-form documentation.
- Do not create extra top-level folders without updating `docs/MONOREPO.md`.
- Do not create extra documentation roots.
- Do not create `docs/documentation/`.
- Do not create `Common`, `Shared`, `Utils`, `Helpers`, `Components`, or similar root folders.
- Put shared behavior in `ArevKit`.
- App targets should mostly contain game-specific registration, assets, app metadata, and mode configuration.
- Use ArevData as the source of truth.
- Do not depend on the hosted ArevData API during gameplay.
- Do not create accounts, analytics, ads, subscriptions, or social features outside Game Center.
- Do not add timer pressure unless a future document explicitly requests it.
- Do not create duplicate UI systems inside app targets.
- Do not create custom leaderboards for arbitrary user settings.
- Prefer simple, testable Swift types over clever abstractions.
- Keep commits small and conventional.

## Required structure

```txt
arevgames/
  README.md
  AGENTS.md
  Package.swift
  ArevGames.xcworkspace
  apps/
    arev-flags/
    arev-pinpoint/
    arev-guess-the-country/
    arev-map-tap/
  libs/
    ArevKit/
      Package.swift
      Sources/
        ArevKitCore/
        ArevKitData/
        ArevKitUI/
        ArevKitMap/
        ArevKitGameCenter/
        ArevKitTesting/
      Tests/
  data/
    source/
    generated/
  tools/
    export-arev-data/
  docs/
    apps/
  .github/
    workflows/
```

## Commit policy

Use conventional commits.

Examples:

- `docs: add pinpoint game spec`
- `feat(arevkit): add shared round engine`
- `feat(flags): add flag to country mode`
- `test(pinpoint): cover distance scoring`
- `fix(data): normalize country code casing`
- `chore(ci): add iOS build workflow`

Commit continuously by feature. Do not make one giant final commit.

## Build strategy

Start shared-first, then app-specific.

1. Create repository structure.
2. Create the Swift package/workspace foundation.
3. Create `libs/ArevKit` modules.
4. Create data export tooling and generated sample data.
5. Build common UI and game flow.
6. Build Arev Flags as the first vertical slice.
7. Add Game Center adapter behind a protocol.
8. Add Pinpoint map infrastructure.
9. Add remaining apps using shared components.
10. Add tests, CI, screenshots, and app metadata.

## ArevKit ownership

`ArevKit` must own:

- App shell.
- Navigation.
- Theme and tokens.
- Shared buttons/cards/sheets.
- Round flow.
- Question generation primitives.
- Answer validation primitives.
- Scoring primitives.
- Progress and weak-item tracking.
- Settings.
- Sound and haptics.
- Accessibility helpers.
- Data loading and indexing.
- Map rendering and map hit testing.
- Game Center authentication, leaderboard submission, and achievements.
- Test doubles.

App targets should own:

- App name and bundle ID.
- App icon and launch assets.
- Mode list.
- Copy strings specific to the app.
- Game-specific mode configuration.
- Game Center leaderboard IDs specific to that app.

## Data rules

- Generate local app data from ArevData packages.
- Normalize all country codes to uppercase ISO alpha-2 unless a field explicitly needs alpha-3.
- Store generated assets deterministically.
- Keep generated data small enough for app bundles.
- Do not include remote URLs as the only source of gameplay-critical images.
- If SVG flags are used, bundle them locally or generate local PNG/PDF vector assets during build.
- If map paths are used for hit testing, write deterministic tests for at least Europe, island nations, tiny countries, and edge cases around the antimeridian.

## Game Center rules

- Game Center is optional at runtime.
- The app must work without Game Center login.
- Submit only official preset modes to Game Center.
- Store all custom-mode scores locally.
- Queue failed submissions and retry later.
- Never block finishing a round on network or Game Center state.

## UI rules

- Native SwiftUI.
- Clean, calm, tactile.
- Large tappable controls.
- No cluttered educational dashboard.
- No fake gamification noise.
- No forced onboarding.
- No account flow.
- Settings should be available, but not dominate gameplay.

## Testing requirement

Every shared system needs tests before more apps are added on top of it.

Minimum tests:

- Data loading.
- Country/flag/city lookup.
- Question generation.
- Distractor generation.
- Round completion.
- Scoring.
- Distance scoring.
- Game Center adapter behavior via test double.
- Local progress persistence.
- Accessibility labels for core components.

## Documentation rules

- Update existing files in `docs/` when possible.
- Do not create `docs/documentation`, `docs/specs`, `docs/plans`, root `documentation`, `notes`, `architecture`, or similar folders.
- App specs belong in `docs/apps/`.
- Root docs should stay limited to `README.md`, `AGENTS.md`, and future conventional files such as `LICENSE.md` or `CONTRIBUTING.md`.

## Definition of done

A feature is done when:

- It is implemented in the correct shared/app layer.
- It has unit tests where applicable.
- It does not introduce remote gameplay dependencies.
- It has accessible labels and dynamic type handling.
- It works in light and dark mode.
- It has no obvious duplicated logic from another app.
- It is documented if it changes architecture, data shape, scoring, or Game Center behavior.
