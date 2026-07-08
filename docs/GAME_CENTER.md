# Game Center

Game Center is used for official leaderboards and achievements. It must not be required to play.

## Rules

- Games work without Game Center authentication.
- Game Center is optional at runtime.
- Only official presets submit to Game Center.
- Custom settings save local scores only.
- Practice modes never submit to Game Center.
- Failed official submissions are queued and retried later.
- Result screens must not block on network/Game Center.

## Implementation location

```txt
libs/ArevKit/Sources/ArevKitGameCenter/
```

App targets only provide IDs and app-specific configuration.

## Protocol

```swift
public protocol ArevGameCenterClient {
    var isAvailable: Bool { get }
    var localPlayerDisplayName: String? { get }

    func authenticate() async
    func submit(score: Int, leaderboardID: String) async throws
    func reportAchievement(id: String, percentComplete: Double) async throws
}
```

Use a fake client for tests.

## Official presets

A preset is official when all score-affecting settings are fixed.

```swift
public struct ArevOfficialPreset: Codable, Equatable, Identifiable {
    public let id: String
    public let modeID: ArevGameModeID
    public let title: String
    public let questionCount: Int
    public let region: ArevRegionFilter
    public let obscurity: ArevObscurityLevel
    public let showBorders: Bool?
    public let showLabels: Bool?
    public let leaderboardID: String
}
```

Do not create a leaderboard for every possible settings combination.

## Initial leaderboard suggestions

### Arev Flags

- `arev.flags.flag_to_country.world.10`
- `arev.flags.country_to_flag.world.10`
- `arev.flags.flag_grid.world.10`

### Arev Pinpoint

- `arev.pinpoint.countries.world.10`
- `arev.pinpoint.capitals.world.10`
- `arev.pinpoint.countries.europe.10`
- `arev.pinpoint.no_borders.world.10`

### Arev Guess the Country

- `arev.guesscountry.mixed.world.10`
- `arev.guesscountry.shape.world.10`
- `arev.guesscountry.capital.world.10`

### Arev Map Tap

- `arev.maptap.countries.world.10`
- `arev.maptap.countries.europe.10`
- `arev.maptap.no_borders.world.10`

Final IDs must match App Store Connect.

## Achievement suggestions

Shared achievement triggers:

- first round.
- first correct.
- first perfect.
- perfect round.
- 10 perfect answers.
- 100 correct answers.
- all continents.
- practice complete.

Arev Flags specific:

- 25 flags correct.
- 100 flags correct.
- similar flag solved.

Arev Pinpoint specific:

- perfect country tap.
- capital close tap.
- no borders win.
- tiny country found.

## Submission queue

```swift
struct PendingGameCenterSubmission: Codable, Identifiable {
    let id: UUID
    let appID: ArevGameAppID
    let leaderboardID: String
    let score: Int
    let createdAt: Date
    var attemptCount: Int
}
```

Retry when app launches, authentication succeeds, or the player opens progress/settings. Do not retry aggressively.

## Authentication UX

- Try to authenticate passively.
- Do not force sign-in before a round.
- Show Game Center status in settings/progress.
- If unavailable, hide or soften leaderboard UI.

## Tests

Required tests:

- Custom settings do not submit.
- Practice modes do not submit.
- Official presets submit with correct leaderboard ID.
- Failed submissions are queued.
- Queue retry removes successful submission.
- Achievements report correct IDs.
- Fake client works without GameKit.
