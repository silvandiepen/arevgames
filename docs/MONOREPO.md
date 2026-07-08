# Monorepo Structure

ArevGames must stay clean and predictable.

## Required top-level structure

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
    apps/
  .github/
    workflows/
```

## Folder ownership

### `apps/`

App targets only. Each app folder may contain app entry, bundle metadata, assets, entitlements, app-specific mode registration, strings, and Game Center IDs.

App folders must not contain reusable UI, popup wrappers, SVG rendering wrappers, map engines, scoring, data loading, generic quiz logic, progress stores, or Game Center implementation.

### `libs/`

Shared libraries only. `libs/ArevKit` is the shared Swift package.

Do not create root folders named `Common`, `Shared`, `Utils`, `Helpers`, `Components`, or similar. Shared code goes into the correct ArevKit module.

Third-party dependencies used by shared UI/rendering should be declared for ArevKit and wrapped inside ArevKit modules. App targets should depend on ArevKit, not directly on UI/rendering packages such as SVGKit or PopupView.

### `data/`

Source and generated local game data.

```txt
data/source/
  export-config.json
  manual-country-obscurity.json
  manual-city-obscurity.json

data/generated/
  countries.json
  flags.json
  cities.json
  geography.json
  world-map.json
  app-indexes.json
```

### `tools/`

Repository tooling. The first required tool is `tools/export-arev-data/`.

Do not create both `scripts/` and `tools/`. Use `tools/`.

### `docs/`

The only long-form documentation root.

Do not create `docs/documentation/`, root `documentation/`, `notes/`, `planning/`, or `architecture/`.

## Naming rules

Use lower-kebab-case for app/tool/data folders:

```txt
apps/arev-flags
tools/export-arev-data
data/generated
```

Use Swift module casing for Swift modules:

```txt
libs/ArevKit/Sources/ArevKitCore
libs/ArevKit/Sources/ArevKitUI
```

Use uppercase underscore names only for app spec docs:

```txt
docs/apps/AREV_FLAGS.md
```

## Dependency direction

Allowed:

```txt
apps/* -> libs/ArevKit
ArevKitUI -> ArevKitCore
ArevKitUI -> ArevKitData
ArevKitUI -> ArevKitMap
ArevKitUI -> PopupView
ArevKitUI -> SVGKit
ArevKitData -> ArevKitCore
ArevKitMap -> ArevKitCore
ArevKitMap -> SVGKit
ArevKitGameCenter -> ArevKitCore
ArevKitTesting -> all ArevKit modules
```

Forbidden:

```txt
apps/* -> PopupView
apps/* -> SVGKit
ArevKitCore -> SwiftUI
ArevKitCore -> GameKit
ArevKitCore -> PopupView
ArevKitCore -> SVGKit
ArevKitData -> apps/*
ArevKitMap -> apps/*
ArevKitGameCenter -> apps/*
libs/* -> apps/*
```

## Anti-mess rules for agents

Agents must not:

- Create undocumented top-level folders.
- Create another docs root.
- Put shared components inside app folders.
- Import PopupView directly in app targets.
- Import SVGKit directly in app targets.
- Add random `Utils.swift`, `Helpers.swift`, or `Extensions.swift` without a clear module/type purpose.
- Duplicate logic across apps.
- Put generated files beside source code when `data/generated` is the source of truth.
- Hide architecture decisions only in comments.

## Adding an app

1. Add `apps/arev-new-app/`.
2. Add `docs/apps/AREV_NEW_APP.md`.
3. Add a thin app configuration.
4. Register modes using existing ArevKit protocols.
5. Add app assets and Game Center IDs.
6. Add tests only for genuinely new behavior.
