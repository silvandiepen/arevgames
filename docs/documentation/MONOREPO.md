# Monorepo Structure

ArevGames must stay clean and predictable. This repository contains multiple iOS apps, shared libraries, generated data, tools, and documentation.

## Required top-level structure

Use this structure unless there is a deliberate documented reason to change it:

```txt
arevgames/
  README.md
  AGENTS.md
  Package.swift
  ArevGames.xcworkspace
  apps/
    arev-flags/
    arev-guess-the-country/
    arev-pinpoint/
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
        ArevKitCoreTests/
        ArevKitDataTests/
        ArevKitMapTests/
        ArevKitGameCenterTests/
  data/
    source/
    generated/
  tools/
    export-arev-data/
  docs/
    documentation/
  .github/
    workflows/
```

## Naming rules

Use lower-kebab-case for folders that represent products or tooling:

```txt
apps/arev-flags
tools/export-arev-data
data/generated
```

Use Swift module casing for Swift packages/modules:

```txt
libs/ArevKit/Sources/ArevKitCore
libs/ArevKit/Sources/ArevKitUI
```

Use uppercase underscore Markdown names inside documentation only for app specs:

```txt
docs/documentation/apps/AREV_FLAGS.md
```

## Top-level folder ownership

### `apps/`

Contains app targets only.

Each app folder may contain:

```txt
apps/arev-flags/
  App/
  Assets.xcassets/
  Preview Content/
  arev-flags.entitlements
  Info.plist
```

An app folder should not contain reusable UI, reusable data logic, or reusable game systems.

### `libs/`

Contains shared libraries.

`libs/ArevKit` is the required shared Swift package. All shared game functionality belongs here.

Do not create random shared folders such as `Common`, `Shared`, `Utils`, `Helpers`, or `Components` at repo root.

If a reusable thing is needed, place it in the correct ArevKit module.

### `data/`

Contains source metadata and generated local game data.

```txt
data/
  source/
    manual-obscurity-overrides.json
    export-config.json
  generated/
    countries.json
    flags.json
    cities.json
    geography.json
    world-map.json
    app-indexes.json
```

Do not put generated data inside app folders unless Xcode requires an app-specific copy step. The source of truth stays in `data/generated`.

### `tools/`

Contains repository tooling.

First required tool:

```txt
tools/export-arev-data/
```

This tool reads ArevData packages and writes deterministic generated data/assets.

Do not create `scripts/`, `bin/`, `tasks/`, and `tools/` all at once. Use `tools/` only.

### `docs/documentation/`

Contains all long-form documentation.

Do not create additional documentation roots.

### `.github/workflows/`

Contains GitHub Actions workflows.

Start simple:

- Swift package tests.
- iOS build when schemes exist.
- data export validation.

## App target responsibilities

App targets should be thin.

Allowed in app target:

- App entry point.
- Bundle ID and entitlements.
- App icon and launch assets.
- App-specific colors if needed.
- App-specific mode registration.
- App-specific Game Center IDs.
- App-specific strings.

Not allowed in app target:

- shared button components.
- shared result screens.
- map engine.
- scoring engine.
- data loader.
- Game Center adapter implementation.
- progress store implementation.
- generic quiz logic.

## Library module responsibilities

### ArevKitCore

Pure-ish models and game logic.

No SwiftUI. No GameKit. No app target imports.

### ArevKitData

Data loading, validation, lookup, and indexes.

### ArevKitUI

SwiftUI components and shared screens.

### ArevKitMap

Map rendering, projection, hit testing, distance helpers, and map-specific scoring support.

### ArevKitGameCenter

Game Center integration behind protocols.

### ArevKitTesting

Fixtures, mocks, deterministic RNG, and testing helpers.

## Dependency direction

Allowed:

```txt
apps/* -> libs/ArevKit
ArevKitUI -> ArevKitCore
ArevKitUI -> ArevKitData
ArevKitUI -> ArevKitMap
ArevKitData -> ArevKitCore
ArevKitMap -> ArevKitCore
ArevKitGameCenter -> ArevKitCore
ArevKitTesting -> all ArevKit modules
```

Forbidden:

```txt
ArevKitCore -> SwiftUI
ArevKitCore -> GameKit
ArevKitData -> apps/*
ArevKitMap -> apps/*
ArevKitGameCenter -> apps/*
libs/* -> apps/*
```

## Anti-mess rules for agents

Agents must not:

- Create `src/` at the repository root.
- Create `Sources/` at the repository root except through a root Swift package if deliberately chosen.
- Create new top-level folders without updating this document.
- Duplicate shared components inside app folders.
- Add another docs root.
- Put app-specific code into ArevKit unless it is generalized.
- Put generated files beside source files when `data/generated` is the correct location.
- Add random helper files called `Utils.swift`, `Helpers.swift`, or `Extensions.swift` without a clear module and type-level purpose.
- Hide important architecture decisions in comments only.

## Adding a new app

To add a new app:

1. Add `apps/arev-new-app/`.
2. Add an app spec under `docs/documentation/apps/`.
3. Add a thin app configuration type.
4. Register modes using existing ArevKit protocols.
5. Add app assets and metadata.
6. Add tests only for genuinely new behavior.

The expected amount of custom code for a new app should be small.

## Adding a new shared feature

To add a shared feature:

1. Pick the correct ArevKit module.
2. Add the implementation there.
3. Add tests in the matching test target.
4. Update documentation if it changes behavior, data, scoring, UI, or architecture.
5. Reuse it from app targets by configuration.

## Generated files policy

Generated data should be committed only if it is stable and useful for native builds.

Generated files must:

- Be deterministic.
- Be sorted.
- Have a clear source.
- Be regenerated by `tools/export-arev-data`.
- Have validation tests.

## Initial implementation order

1. Create root Swift package/workspace.
2. Create `libs/ArevKit` package and modules.
3. Create `tools/export-arev-data`.
4. Generate minimal fixture data.
5. Build ArevKitCore and ArevKitData.
6. Build ArevKitUI shell.
7. Build Arev Flags.
8. Add Game Center adapter.
9. Build map engine.
10. Build Arev Pinpoint.
11. Build Arev Guess the Country.
12. Build Arev Map Tap.
