# Repository Architecture

This repository should be a native iOS monorepo with multiple app targets and one shared kit.

## Target layout

```txt
arevgames/
  README.md
  AGENTS.md
  ArevGames.xcworkspace
  Package.swift
  Apps/
    ArevFlags/
      ArevFlagsApp.swift
      ArevFlagsConfig.swift
      Assets.xcassets/
      Info.plist
    ArevGuessTheCountry/
    ArevPinpoint/
    ArevMapTap/
  Packages/
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
  Data/
    Source/
      arev-export-metadata.json
    Generated/
      countries.json
      flags.json
      cities.json
      geography.json
      world-map.json
      app-indexes.json
  Scripts/
    export-arev-data/
      package.json
      src/
        export.ts
        validate.ts
        build-indexes.ts
  docs/
```

The exact Xcode project generation approach can be decided by the builder, but the product separation must remain:

- Shared framework code lives in `Packages/ArevKit`.
- App-specific Swift files stay thin.
- Generated data stays separate from source package code.
- Scripts are deterministic and repeatable.

## Package modules

### ArevKitCore

Shared models and logic with no SwiftUI dependency where possible.

Owns:

- `ArevCountryCode`.
- `ArevCountry`.
- `ArevFlag`.
- `ArevCity`.
- `ArevGeography`.
- `ArevGameMode`.
- `ArevQuestion`.
- `ArevAnswer`.
- `ArevRound`.
- `ArevScore`.
- `ArevDifficulty`.
- `ArevRegionFilter`.
- Question generation protocols.
- Distractor generation.
- Progress models.
- Settings models.

### ArevKitData

Data loading, indexing, and validation.

Owns:

- Loading bundled JSON.
- Version metadata.
- Country lookup.
- Flag lookup.
- City lookup.
- Capital lookup.
- Similar flag lookup.
- Region filtering.
- Obscurity tier filtering.
- Practice item selection.
- Generated data schema validation tests.

### ArevKitUI

Shared SwiftUI components and app shell.

Owns:

- Game shell.
- Home screen.
- Mode picker.
- Round screen layout.
- Question card.
- Answer grid.
- Result screen.
- Settings sheet.
- Progress summary.
- Study list.
- Shared buttons.
- Shared typography/tokens.
- Haptics and sound wrappers.

### ArevKitMap

Map rendering and interaction.

Owns:

- World map renderer.
- Country shape renderer.
- Map viewport and zoom/pan.
- Country tap hit testing.
- Point-to-country lookup.
- Coordinate projection.
- Distance calculations.
- Pinpoint scoring helpers.
- Border/label display settings.

### ArevKitGameCenter

Game Center integration behind protocols.

Owns:

- Authentication.
- Capability state.
- Leaderboard submission.
- Achievement reporting.
- Submission queue.
- Test doubles.
- App-specific leaderboard ID registration.

### ArevKitTesting

Shared test fixtures and helpers.

Owns:

- Small fixture data.
- Deterministic RNG.
- Mock data stores.
- Mock Game Center.
- Snapshot helpers if added.

## App target responsibilities

Each app target should define only:

- App entry point.
- Bundle ID.
- App icon.
- Display name.
- App-specific tint/accent if needed.
- Mode list.
- Default settings.
- App-specific Game Center leaderboard/achievement IDs.
- App Store metadata assets.

Example:

```swift
@main
struct ArevFlagsApp: App {
    var body: some Scene {
        WindowGroup {
            ArevGameShell(
                app: ArevFlagsConfiguration()
            )
        }
    }
}
```

## Bundle IDs

Use consistent bundle IDs. Final IDs may change, but use this pattern unless changed deliberately:

| App | Bundle ID |
| --- | --- |
| Arev Flags | `app.arev.flags` |
| Arev Guess the Country | `app.arev.guesscountry` |
| Arev Pinpoint | `app.arev.pinpoint` |
| Arev Map Tap | `app.arev.maptap` |

## Naming conventions

- Public shared types use `Arev` prefix.
- App-specific types use app prefix, e.g. `ArevFlagsMode`.
- Country codes are uppercase ISO alpha-2 strings internally.
- Alpha-3 is only used where ArevData/Wikimedia map URLs require it.
- Do not use raw string IDs across the codebase where a typed wrapper is reasonable.

## Dependency direction

Allowed:

```txt
Apps -> ArevKitUI -> ArevKitCore
Apps -> ArevKitData
Apps -> ArevKitMap
Apps -> ArevKitGameCenter
ArevKitUI -> ArevKitCore
ArevKitMap -> ArevKitCore
ArevKitData -> ArevKitCore
ArevKitGameCenter -> ArevKitCore
```

Avoid:

```txt
ArevKitCore -> SwiftUI
ArevKitCore -> GameKit
ArevKitData -> App targets
ArevKitMap -> App targets
ArevKitGameCenter -> App targets
```

## Data generation

The export script should be the only place where Node/TypeScript ArevData packages are consumed.

The generated files should be stable and checked into the repo unless a future decision says otherwise.

Reasons to commit generated data:

- Agents can build iOS without always running Node export.
- CI can test native code deterministically.
- App bundle contents are visible in reviews.
- Data diffs are inspectable.

## CI direction

CI should eventually include:

- Swift package tests.
- Xcode build for each app scheme.
- Data export validation.
- Generated data drift check.
- Lint/format where configured.

Do not block early implementation on perfect CI. Add CI once the workspace and package compile.
