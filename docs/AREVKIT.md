# ArevKit

`ArevKit` is the shared harness for all ArevGames apps.

The app targets should be thin wrappers around ArevKit configuration. If code can be reused by two apps, it belongs in ArevKit.

## Responsibilities

ArevKit owns:

- App shell and common navigation.
- Shared visual system.
- Shared screens.
- Data loading and indexes.
- Game mode registration.
- Round lifecycle.
- Question generation primitives.
- Answer validation primitives.
- Scoring.
- Progress tracking.
- Practice mode selection.
- Settings.
- Sound and haptics.
- Map rendering.
- Map tap handling.
- Game Center adapter.
- Local persistence.
- Test doubles.

App targets own:

- App name.
- App icon.
- Bundle ID.
- Mode list.
- Default mode presets.
- App-specific copy.
- App-specific Game Center IDs.

## Core concepts

### ArevGameApp

A game product configuration.

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

A playable or study mode.

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

Use type erasure for app configuration:

```swift
public struct AnyArevGameMode: Identifiable {
    public let id: ArevGameModeID
    public let title: String
    public let subtitle: String
    public let objective: ArevLearningObjective
    public let supportsGameCenter: Bool
}
```

### ArevQuestion

A question is displayable data, not a view.

```swift
public protocol ArevQuestion: Identifiable, Equatable, Codable {
    var prompt: String { get }
    var accessibilityPrompt: String { get }
}
```

Examples:

- `FlagQuestion` includes `targetCountryCode` and local flag asset reference.
- `CountryNameQuestion` includes target country name.
- `CountryShapeQuestion` includes country path/asset reference.
- `MapPointQuestion` includes target coordinate or country polygon target.

### ArevAnswer

An answer is selectable data.

```swift
public protocol ArevAnswer: Identifiable, Equatable, Codable {
    var title: String { get }
    var accessibilityLabel: String { get }
}
```

Examples:

- `CountryAnswer`.
- `FlagAnswer`.
- `MapPointAnswer`.
- `MapCountryAnswer`.

### ArevRound

A round contains a fixed list of generated questions.

```swift
public struct ArevRound<Question: ArevQuestion, Answer: ArevAnswer>: Identifiable, Codable {
    public let id: UUID
    public let modeID: ArevGameModeID
    public let questions: [ArevRoundQuestion<Question, Answer>]
    public var currentIndex: Int
    public var responses: [ArevResponse<Answer>]
}
```

### ArevRoundQuestion

```swift
public struct ArevRoundQuestion<Question: ArevQuestion, Answer: ArevAnswer>: Identifiable, Codable {
    public let id: UUID
    public let question: Question
    public let correctAnswer: Answer
    public let options: [Answer]
    public let explanation: ArevExplanation?
}
```

For map-point modes, `options` may be empty because the answer is a map tap.

### ArevScore

```swift
public struct ArevScore: Codable, Equatable {
    public let rawPoints: Int
    public let maxPoints: Int
    public let accuracy: Double
    public let perfectCount: Int
    public let correctCount: Int
    public let questionCount: Int
}
```

## Shared screens

ArevKitUI should provide:

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

Apps should not copy these screens.

## Shared UI components

- `ArevButton`.
- `ArevIconButton`.
- `ArevCard`.
- `ArevFlagView`.
- `ArevCountryShapeView`.
- `ArevWorldMapView`.
- `ArevProgressRing`.
- `ArevScorePill`.
- `ArevModeCard`.
- `ArevSettingsRow`.
- `ArevSegmentedPicker`.

## Data store

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

## Question generation

Question generation should be deterministic when seeded.

```swift
public protocol ArevQuestionGenerator {
    associatedtype Question: ArevQuestion
    associatedtype Answer: ArevAnswer

    func generate(
        count: Int,
        data: ArevDataStore,
        filters: ArevQuestionFilters,
        rng: inout any RandomNumberGenerator
    ) throws -> [ArevRoundQuestion<Question, Answer>]
}
```

All modes should use a shared generator pattern and not custom random logic inside views.

## Distractor generation

Distractors should be plausible, not arbitrary.

Flag modes:

1. Prefer ArevData similar flags.
2. Then same continent.
3. Then same dominant colors.
4. Then random valid countries.

Country/capital modes:

1. Same continent.
2. Similar population/area where available.
3. Nearby countries.
4. Random valid countries.

Avoid duplicate options. Avoid options that are too obscure when the selected difficulty is easy.

## Settings

Shared settings:

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

Apps may hide settings that do not apply.

## Persistence

Use local persistence for:

- Settings.
- Local high scores.
- Item mastery.
- Miss history.
- Completed study items.
- Game Center submission queue.

Use a repository protocol so storage can be tested:

```swift
public protocol ArevProgressStore {
    func loadProgress(appID: ArevGameAppID) throws -> ArevAppProgress
    func saveProgress(_ progress: ArevAppProgress, appID: ArevGameAppID) throws
}
```

## Game Center adapter

Expose Game Center through a protocol only:

```swift
public protocol ArevGameCenterClient {
    var isAvailable: Bool { get }
    func authenticate() async
    func submit(score: Int, leaderboardID: String) async throws
    func reportAchievement(id: String, percentComplete: Double) async throws
}
```

The real implementation lives in `ArevKitGameCenter`. Tests use a fake implementation.

## Map abstraction

Map views must not leak raw SVG/path details into app targets.

```swift
public protocol ArevMapEngine {
    func countryAt(point: CGPoint, in viewport: ArevMapViewport) -> ArevCountryCode?
    func projectedPoint(lat: Double, lon: Double, viewport: ArevMapViewport) -> CGPoint
    func distanceKm(from tap: ArevMapTap, to target: ArevMapTarget) -> Double
}
```

Pinpoint and Map Tap should both use the same map engine.

## Design requirement

ArevKit must make it easy to create new apps by configuration. When adding a fifth app, the expected work should be:

1. Add app target.
2. Add app configuration.
3. Register modes.
4. Add assets/metadata.
5. Add Game Center IDs.
6. Add tests for any genuinely new mode logic.
