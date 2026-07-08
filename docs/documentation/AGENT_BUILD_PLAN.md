# Agent Build Plan

This is the implementation plan for coding agents.

Agents should follow this order unless a later instruction explicitly changes it.

## Phase 0: keep the repository clean

Before adding code:

1. Read `AGENTS.md`.
2. Read `docs/documentation/MONOREPO.md`.
3. Do not create new top-level folders beyond the documented structure.
4. Do not scatter docs outside `docs/documentation/`.
5. Do not duplicate shared code inside app targets.

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

Add fixture output if full export is not ready yet.

## Phase 4: ArevKitData

Implement:

- bundled JSON loader.
- country lookup.
- flag lookup.
- similar flag lookup.
- city lookup.
- capital lookup.
- geography lookup.
- region filters.
- obscurity filters.
- invalid data handling.

Add tests.

## Phase 5: shared game engine

Implement:

- round generation.
- answer validation.
- multiple-choice scoring.
- local progress store.
- weak item tracking.
- settings fingerprint.
- deterministic RNG support.

Add tests.

## Phase 6: shared UI shell

Implement ArevKitUI:

- `ArevGameShell`.
- home screen.
- mode picker.
- round view.
- question card.
- answer grid.
- result view.
- settings sheet.
- study view.

Use dummy/fixture data until real app wiring is ready.

## Phase 7: Arev Flags vertical slice

Create Arev Flags app target in:

```txt
apps/arev-flags/
```

Implement modes:

1. Flag to Country.
2. Country to Flag.
3. Study Flags.
4. Practice Misses.
5. Flag Grid.
6. Flag Match.

The first playable MVP can ship with Flag to Country + Country to Flag + Study Flags.

## Phase 8: Game Center adapter

Implement in ArevKitGameCenter:

- authentication.
- official preset check.
- leaderboard submission.
- achievement reporting.
- submission queue.
- fake client for tests.

Wire Arev Flags official presets.

## Phase 9: map engine

Implement ArevKitMap:

- world map renderer.
- country shape renderer.
- zoom/pan viewport.
- coordinate projection.
- country hit testing.
- distance calculation.
- pinpoint scoring.

Add tests for:

- Europe countries.
- island countries.
- tiny countries.
- city/capital distances.

## Phase 10: Arev Pinpoint

Create Arev Pinpoint app target.

Implement modes:

1. Find the Country.
2. Find the Capital.
3. Find the City.
4. No Borders.
5. Practice Misses.

Add official Game Center presets only after scoring feels stable.

## Phase 11: Arev Guess the Country

Create app target.

Implement modes:

1. By Flag.
2. By Shape.
3. By Capital.
4. By Map Clue.
5. Mixed.

Reuse existing clue renderers and generators.

## Phase 12: Arev Map Tap

Create app target.

Implement modes:

1. Find Country.
2. Continent Focus.
3. No Borders Lite.
4. Practice Misses.

Reuse Pinpoint map engine with simpler correct/wrong scoring.

## Phase 13: CI and release polish

Add/finish:

- Swift tests in CI.
- data export validation in CI.
- app scheme builds.
- App Store metadata.
- screenshots.
- icons.
- review notes.
- privacy labels verification.

## Agent prompt to start implementation

Use this as the build prompt:

```txt
Read AGENTS.md and all files in docs/documentation. Build ArevGames as a clean native iOS monorepo. Use apps/ for app targets, libs/ArevKit for shared Swift packages, data/ for generated local data, tools/export-arev-data for the ArevData export pipeline, and docs/documentation for all long-form docs. Start with the repository foundation, ArevKit core modules, data export pipeline, and Arev Flags vertical slice. Keep app targets thin. Put shared UI, game logic, data loading, map logic, scoring, progress, settings, and Game Center integration in ArevKit. Use conventional commits by feature and add tests as you build. Do not create extra top-level folders or duplicate shared code inside app targets.
```
