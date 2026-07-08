# Game Center

Game Center is used for official leaderboards and achievements. It must not be required to play.

## Rules

- The games work without Game Center authentication.
- Game Center is optional at runtime.
- Only official presets submit to Game Center.
- Custom settings save local scores only.
- Practice modes never submit to Game Center.
- Failed official submissions are queued and retried later.
- The result screen must not block on network/Game Center.

## Implementation location

Game Center implementation belongs in:

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

Official preset fields:

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

Do not create a leaderboard for every possible combination of settings.

## Local vs Game Center scores

### Local scores

Save every completed round locally.

Local score key:

- App ID.
- Mode ID.
- Settings fingerprint.
- Question count.
- Date.

### Game Center scores

Submit only when:

- mode supports Game Center.
- current settings match an official preset exactly.
- the player finished the round.
- the score is valid.

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

These IDs are proposals. Final IDs must match App Store Connect.

## Achievement suggestions

Use shared events mapped per app.

### Shared achievements

| ID suffix | Trigger |
| --- | --- |
| `first_round` | Complete first round |
| `first_correct` | First correct answer |
| `first_perfect` | First perfect answer |
| `perfect_round` | Complete a round with all perfect/correct answers |
| `ten_perfect` | 10 perfect answers total |
| `hundred_correct` | 100 correct answers total |
| `all_continents` | Correct answer in every continent |
| `practice_complete` | Complete a practice round |

### Arev Flags specific

| ID suffix | Trigger |
| --- | --- |
| `flag_master_25` | 25 flags correct |
| `flag_master_100` | 100 flags correct |
| `similar_flags_win` | Correctly answer a similar-flag distractor question |

### Arev Pinpoint specific

| ID suffix | Trigger |
| --- | --- |
| `pinpoint_country_perfect` | Perfect country tap |
| `pinpoint_capital_close` | Capital tap within perfect threshold |
| `no_borders_win` | Complete No Borders official round |
| `tiny_country_found` | Correct tiny country target |

## Submission queue

Store pending submissions locally.

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

Retry when:

- app launches.
- player opens settings/progress.
- Game Center authentication succeeds.

Do not retry aggressively.

## Authentication UX

Game Center authentication should be passive and non-blocking.

- Try to authenticate on launch if appropriate.
- Do not force sign-in before a round.
- Show Game Center status in settings/progress.
- If unavailable, hide or soften leaderboard UI.

## Testing

Required tests:

- Custom settings do not submit.
- Practice modes do not submit.
- Official presets submit with correct leaderboard ID.
- Failed submissions are queued.
- Queue retry removes successful submission.
- Achievements report correct IDs.
- Fake client works without GameKit.

## App Store Connect notes

Game Center leaderboard and achievement IDs must be created in App Store Connect for each app before release.

Documentation and code IDs must stay synchronized. Add a config validation test to catch missing/unknown IDs.
