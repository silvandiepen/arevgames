# Agent Build Plan

Agents should follow this order unless a later instruction explicitly changes it.

## Phase 0: keep the repository clean

Before adding code:

1. Read `AGENTS.md`.
2. Read `docs/README.md`.
3. Read `docs/MONOREPO.md`.
4. Read `docs/TESTING_CI.md`.
5. Read `docs/WEBSITE.md`.
6. Read this file.
7. Do not create new top-level folders beyond the documented structure.
8. Do not create another docs root.
9. Do not duplicate shared code inside app targets.

## Phase 1: repository foundation and CI

Create:

```txt
apps/
libs/
data/
tools/
site/
.github/workflows/
```

Add the first CI workflow:

```txt
.github/workflows/ci.yml
```

The workflow should be conditional and safe before app code exists.

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

## Phase 2: ArevData brand assets

Find and copy/export the actual ArevData/arev logo from arevdata.com/source assets.

Store it in:

```txt
data/source/brand/arev-logo.svg
site/public/logo.svg
site/src/assets/logo.svg
```

Do not redraw or approximate the logo.

Use generated asset catalog outputs if iOS launch screens require PNG/PDF variants.

## Phase 3: ArevKit foundation

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

## Phase 4: data export

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
- brand logo copy/export validation.
- validation.
- deterministic sorting.

Write output to:

```txt
data/generated/
```

## Phase 5: website scaffold

Create `site/` using the Luys-style stack:

- Vue 3.
- Vite.
- TypeScript.
- Vue Router 4.
- Sass.
- `@sil/ui`.
- `vue-tsc`.
- Static Cloudflare Pages output.

Implement:

- home page.
- apps index.
- dynamic app detail page.
- privacy page.
- not found page.
- app data file.
- ArevData logo usage.
- light/dark mode.
- SEO/meta basics.

CI should typecheck and build the website once `site/package.json` exists.

## Phase 6: ArevKitData

Implement bundled JSON loading, lookup, filters, validation, and missing-data handling.

## Phase 7: shared game engine

Implement round generation, answer validation, scoring, progress, weak item tracking, settings fingerprint, and deterministic RNG support.

## Phase 8: shared UI shell

Implement:

- `ArevGameShell`.
- `ArevSplashView` using the ArevData logo.
- home screen.
- mode picker.
- round view.
- question card.
- answer grid.
- result view.
- settings popup.
- study view.

## Phase 9: Arev Flags vertical slice

Create app target in:

```txt
apps/arev-flags/
```

First MVP:

1. Splash with ArevData logo.
2. Flag to Country.
3. Country to Flag.
4. Study Flags.
5. Local progress.
6. Local scores.

Then add Practice Misses, Flag Grid, and Flag Match.

## Phase 10: Game Center adapter

Implement authentication, official preset check, leaderboard submission, achievement reporting, submission queue, and fake client.

Wire Arev Flags official presets.

## Phase 11: map engine

Implement world map renderer, country shape renderer, zoom/pan viewport, coordinate projection, country hit testing, distance calculation, and Pinpoint scoring.

## Phase 12: Arev Pinpoint

Implement Find the Country, Find the Capital, Find the City, No Borders, and Practice Misses.

## Phase 13: Arev Guess the Country

Implement By Flag, By Shape, By Capital, By Map Clue, and Mixed.

## Phase 14: Arev Map Tap

Implement Find Country, Continent Focus, No Borders Lite, and Practice Misses.

## Phase 15: release polish

Add/finish:

- Swift tests in CI.
- Data export validation in CI.
- Website build in CI.
- App scheme builds in CI.
- App Store metadata.
- screenshots.
- icons.
- review notes.
- privacy labels verification.
- Cloudflare Pages setup.

## Agent prompt to start implementation

```txt
Read AGENTS.md and all files in docs/. Build ArevGames as a clean native iOS monorepo. Use apps/ for native app targets, libs/ArevKit for shared Swift packages, data/ for generated local data and brand source assets, tools/export-arev-data for the ArevData export pipeline, site/ for the Vue/Vite marketing website, docs/ for all long-form docs, and .github/workflows/ for CI. Start with repository foundation, conditional GitHub CI, ArevData logo asset copy/export, ArevKit core modules, data export pipeline, website scaffold, and the Arev Flags vertical slice. Keep app targets thin. Put shared UI, game logic, data loading, map logic, scoring, progress, settings, and Game Center integration in ArevKit. Use the ArevData/arev logo for the splash. Use conventional commits by feature and add tests as you build. Do not create extra top-level folders or duplicate shared code inside app targets.
```
