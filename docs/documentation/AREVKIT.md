# ArevKit

ArevKit is the shared Swift/SwiftUI framework for all ArevGames apps.

The goal is that new games are added mostly by configuration and game-specific mode registration, not by copying screens or logic.

## Location

ArevKit belongs here:

```txt
libs/ArevKit/
  Package.swift
  Sources/
    ArevKitCore/
    ArevKitData/
    ArevKitUI/
    ArevKitMap/
    ArevKitGameCenter/
    ArevKitTesting/
  Tests/
```

## Responsibilities

ArevKit owns:

- App shell.
- Navigation.
- Visual system.
- Shared screens.
- Shared components.
- Data loading and indexing.
- Game mode registration.
- Round lifecycle.
- Question generation.
- Answer validation.
- Scoring.
- Progress tracking.
- Practice mode item selection.
- Settings.
- Sound and haptics.
- Map rendering.
- Map hit testing.
- Pinpoint scoring helpers.
- Game Center integration behind protocols.
- Local persistence.
- Test doubles.

App targets own only:

- App entry point.
- Bundle ID.
- App icon.
- Launch assets.
- App display name.
- App-specific mode list.
- App-specific default settings.
- App-specific Game Center IDs.
- App-specific App Store copy/assets.

## Module responsibilities

### ArevKitCore

No SwiftUI. No GameKit. No app imports.

Owns models and logic:

- Country/flag/city/geography models.
- Game app and game mode models.
- Question and answer protocols.
- Round models.
- Score models.
- Settings models.
- Progress models.
- RNG protocols.
- Distractor generation.
- Generic validation.

### ArevKitData

Owns:

- Loading bundled JSON/assets.
- Data-store protocol and implementation.
- Country lookup.
- Flag lookup.
- City lookup.
- Capital lookup.
- Map availability lookup.
- Similar flag lookup.
- Region and obscurity filtering.
- Data validation.

### ArevKitUI

Owns:

- `ArevGameShell`.
- Home screen.
- Mode picker.
- Round screen.
- Question cards.
- Answer grids.
- Study views.
- Result screens.
- Settings sheets.
- Progress screens.
- Shared buttons/cards/pills.
- Typography and spacing tokens.

### ArevKitMap

Owns:

- World map rendering.
- Country shape rendering.
- Map viewport.
- Zoom/pan behavior.
- Coordinate projection.
- Country tap hit testing.
- Point-to-country lookup.
- Distance calculations.
- Distance scoring helpers.

### ArevKitGameCenter

Owns:

- Authentication.
- Local-player state.
- Leaderboard submission.
- Achievement reporting.
- Submission queue.
- Test fake.

### ArevKitTesting

Owns:

- Fixture data.
- Deterministic RNG.
- Mock data stores.
- Mock progress stores.
- Mock Game Center.
- Helpers for unit tests.

## Core protocols

### ArevGameApp

```swift
public protocol ArevGameApp {
    var id: ArevGameAppID { get }
    var displayName: String { get }
    var shortDescription: String { get }
    var modes: [AnyArevGameMode] { get }
    var defaultSettings: ArevGameSettings { get }
    var gameCenterConfiguration: ArevGameCenterConfiguration? { get }
}
```

### ArevGameMode

```swift
public protocol ArevGameMode {
    associatedtype Question: ArevQuestion
    associatedtype Answer: ArevAnswer

    var id: ArevGameModeID { get }
    var title: String { get }
    var subtitle: String { get }
    var objective: ArevLearningObjective { get }
    var supportedQuestionCounts: [Int] { get }
    var defaultQuestionCount: Int { get }
    var supportsGameCenter: Bool { get }

    func makeRound(
        data: ArevDataStore,
        settings: ArevGameSettings,
        rng: inout any RandomNumberGenerator
    ) throws -> ArevRound<Question, Answer>
}
```

### ArevQuestion

```swift
public protocol ArevQuestion: Identifiable, Equatable, Codable {
    var prompt: String { get }
    var accessibilityPrompt: String { get }
}
```

### ArevAnswer

```swift
public protocol ArevAnswer: Identifiable, Equatable, Codable {
    var title: String { get }
    var accessibilityLabel: String { get }
}
```

### ArevDataStore

```swift
public protocol ArevDataStore {
    var countries: [ArevCountry] { get }
    var flags: [ArevFlag] { get }
    var cities: [ArevCity] { get }
    var geography: [ArevCountryGeography] { get }
    var worldMap: ArevWorldMap { get }

    func country(alpha2: String) -> ArevCountry?
    func flag(alpha2: String) -> ArevFlag?
    func capital(countryCode: String) -> ArevCity?
    func cities(countryCode: String) -> [ArevCity]
    func geography(alpha2: String) -> ArevCountryGeography?
    func similarFlags(alpha2: String) -> [ArevFlag]
}
```

### ArevProgressStore

```swift
public protocol ArevProgressStore {
    func loadProgress(appID: ArevGameAppID) throws -> ArevAppProgress
    func saveProgress(_ progress: ArevAppProgress, appID: ArevGameAppID) throws
}
```

### ArevGameCenterClient

```swift
public protocol ArevGameCenterClient {
    var isAvailable: Bool { get }
    func authenticate() async
    func submit(score: Int, leaderboardID: String) async throws
    func reportAchievement(id: String, percentComplete: Double) async throws
}
```

### ArevMapEngine

```swift
public protocol ArevMapEngine {
    func countryAt(point: CGPoint, in viewport: ArevMapViewport) -> ArevCountryCode?
    func projectedPoint(lat: Double, lon: Double, viewport: ArevMapViewport) -> CGPoint
    func distanceKm(from tap: ArevMapTap, to target: ArevMapTarget) -> Double
}
```

## Shared settings

```swift
public struct ArevGameSettings: Codable, Equatable {
    public var questionCount: Int
    public var regionFilter: ArevRegionFilter
    public var obscurity: ArevObscurityLevel
    public var showBorders: Bool
    public var showLabels: Bool
    public var includeTerritories: Bool
    public var recognizedOnly: Bool
    public var soundEnabled: Bool
    public var hapticsEnabled: Bool
    public var reduceMotion: Bool
    public var colorBlindAssist: Bool
}
```

Apps may hide settings that are irrelevant to their modes.

## Question generation rules

Question generation must be deterministic when seeded.

Distractors should be plausible:

- Flag modes: similar flags, same continent, same colors, then random.
- Shape modes: same continent, similar area, nearby countries, then random.
- Capital modes: same continent, similar prominence, then random.

Views must not generate random questions directly.

## Shared screen requirement

Apps should use shared screens. Do not copy screen implementations across app targets.

Expected shared views:

- `ArevGameShell`.
- `ArevHomeView`.
- `ArevModePickerView`.
- `ArevRoundView`.
- `ArevQuestionCard`.
- `ArevAnswerGrid`.
- `ArevMapQuestionView`.
- `ArevStudyView`.
- `ArevResultView`.
- `ArevSettingsSheet`.
- `ArevProgressView`.

## Definition of a clean app target

A clean app target should be understandable in minutes. It should say:

- This is the app.
- These are the modes.
- These are the presets.
- These are the Game Center IDs.
- Use ArevKit to run it.

Everything else belongs in `libs/ArevKit`.
