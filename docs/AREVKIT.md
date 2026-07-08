# ArevKit

ArevKit is the shared Swift/SwiftUI framework for all ArevGames apps.

The app targets should be thin wrappers around ArevKit configuration. If code can be reused by two apps, it belongs in ArevKit.

## Location

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

## Third-party dependency ownership

Third-party UI/rendering dependencies belong in ArevKit, not in app targets.

Required dependencies:

```txt
SVGKit
https://github.com/SVGKit/SVGKit

PopupView
https://github.com/exyte/PopupView.git
exactVersion: 5.0.2
```

Rules:

- App targets should not import SVGKit directly.
- App targets should not import PopupView directly.
- ArevKitUI owns popup wrappers.
- ArevKitUI/ArevKitMap own SVG rendering wrappers.
- ArevKitMap owns hit testing and map interaction logic.
- If a dependency needs to be changed later, app targets should not need code changes.

## Module responsibilities

### ArevKitCore

No SwiftUI. No GameKit. No SVGKit. No PopupView. No app imports.

Owns:

- Country, flag, city, geography models.
- Game app and mode models.
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
- Country/flag/city/capital/geography lookup.
- Similar flag lookup.
- Region and obscurity filtering.
- Data validation.

### ArevKitUI

Owns:

- App shell.
- Home screen.
- Mode picker.
- Round screen.
- Question cards.
- Answer grids.
- Study views.
- Result screens.
- Settings popups.
- Popup wrappers based on PopupView.
- Shared buttons/cards/pills.
- SVG rendering wrappers for UI use.
- Typography and spacing tokens.

### ArevKitMap

Owns:

- World map rendering.
- Country shape rendering.
- SVGKit integration for map/shape rendering where useful.
- Map viewport.
- Zoom/pan behavior.
- Coordinate projection.
- Country tap hit testing.
- Point-to-country lookup.
- Distance calculations.
- Pinpoint scoring helpers.

### ArevKitGameCenter

Owns:

- Authentication.
- Leaderboard submission.
- Achievement reporting.
- Submission queue.
- Test fake.

### ArevKitTesting

Owns fixtures, mocks, deterministic RNG, and test helpers.

## Core protocols

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

```swift
public protocol ArevQuestion: Identifiable, Equatable, Codable {
    var prompt: String { get }
    var accessibilityPrompt: String { get }
}
```

```swift
public protocol ArevAnswer: Identifiable, Equatable, Codable {
    var title: String { get }
    var accessibilityLabel: String { get }
}
```

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

```swift
public protocol ArevGameCenterClient {
    var isAvailable: Bool { get }
    func authenticate() async
    func submit(score: Int, leaderboardID: String) async throws
    func reportAchievement(id: String, percentComplete: Double) async throws
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

## App target rule

A clean app target should say:

- This is the app.
- These are the modes.
- These are the presets.
- These are the Game Center IDs.
- Use ArevKit to run it.

Everything else belongs in `libs/ArevKit`.
