# Agent Build Plan

Agents should follow this order unless a later instruction explicitly changes it.

## Phase 0: keep the repository clean

Before adding code:

1. Read `AGENTS.md`.
2. Read `docs/README.md`.
3. Read `docs/MONOREPO.md`.
4. Read this file.
5. Do not create new top-level folders beyond the documented structure.
6. Do not create another docs root.
7. Do not duplicate shared code inside app targets.

## Phase 1: repository foundation

Create:

```txt
apps/
libs/
data/
tools/
.github/workflows/
```

Create Swift package/workspace foundation:

```txt
Package.swift
ArevGames.xcworkspace
libs/ArevKit/Package.swift
```

Create empty app folders:

```txt
apps/arev-flags/
apps/arev-pinpoint/
apps/arev-guess-the-country/
apps/arev-map-tap/
```

## Phase 2: ArevKit foundation

Create modules:

```txt
libs/ArevKit/Sources/ArevKitCore
libs/ArevKit/Sources/ArevKitData
libs/ArevKit/Sources/ArevKitUI
libs/ArevKit/Sources/ArevKitMap
libs/ArevKit/Sources/ArevKitGameCenter
libs/ArevKit/Sources/ArevKitTesting
```

Implement core types first:

- Country code.
- Country.
- Flag.
- City.
- Geography.
- Game app.
- Game mode.
- Question.
- Answer.
- Round.
- Score.
- Settings.
- Progress.

Add tests immediately.

## Phase 3: data export

Create:

```txt
tools/export-arev-data/
```

Implement:

- ArevData package install.
- export script.
- schema generation.
- local flag asset export/copy.
- app indexes.
- validation.
- deterministic sorting.

Write output to:

```txt
data/generated/
```

## Phase 4: ArevKitData

Implement bundled JSON loading, lookup, filters, validation, and missing-data handling.

## Phase 5: shared game engine

Implement round generation, answer validation, scoring, progress, weak item tracking, settings fingerprint, and deterministic RNG support.

## Phase 6: shared UI shell

Implement:

- `ArevGameShell`.
- home screen.
- mode picker.
- round view.
- question card.
- answer grid.
- result view.
- settings sheet.
- study view.

## Phase 7: Arev Flags vertical slice

Create app target in:

```txt
apps/arev-flags/
```

First MVP:

1. Flag to Country.
2. Country to Flag.
3. Study Flags.
4. Local progress.
5. Local scores.

Then add Practice Misses, Flag Grid, and Flag Match.

## Phase 8: Game Center adapter

Implement authentication, official preset check, leaderboard submission, achievement reporting, submission queue, and fake client.

Wire Arev Flags official presets.

## Phase 9: map engine

Implement world map renderer, country shape renderer, zoom/pan viewport, coordinate projection, country hit testing, distance calculation, and Pinpoint scoring.

## Phase 10: Arev Pinpoint

Implement Find the Country, Find the Capital, Find the City, No Borders, and Practice Misses.

## Phase 11: Arev Guess the Country

Implement By Flag, By Shape, By Capital, By Map Clue, and Mixed.

## Phase 12: Arev Map Tap

Implement Find Country, Continent Focus, No Borders Lite, and Practice Misses.

## Phase 13: CI and release polish

Add/finish:

- Swift tests in CI.
- Data export validation in CI.
- App scheme builds.
- App Store metadata.
- screenshots.
- icons.
- review notes.
- privacy labels verification.

## Agent prompt to start implementation

```txt
Read AGENTS.md and all files in docs/. Build ArevGames as a clean native iOS monorepo. Use apps/ for app targets, libs/ArevKit for shared Swift packages, data/ for generated local data, tools/export-arev-data for the ArevData export pipeline, and docs/ for all long-form docs. Start with the repository foundation, ArevKit core modules, data export pipeline, and Arev Flags vertical slice. Keep app targets thin. Put shared UI, game logic, data loading, map logic, scoring, progress, settings, and Game Center integration in ArevKit. Use conventional commits by feature and add tests as you build. Do not create extra top-level folders or duplicate shared code inside app targets.
```
