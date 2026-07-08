# Shared Game Systems

All shared gameplay behavior belongs in ArevKit, not in app targets.

## Round lifecycle

All round-based modes follow this lifecycle:

1. Select mode.
2. Select preset/settings.
3. Generate fixed question list.
4. Present question.
5. Accept answer.
6. Validate answer.
7. Show feedback.
8. Save response.
9. Advance.
10. Complete round.
11. Calculate score.
12. Save local progress.
13. Submit Game Center score only if this was an official preset.

Generated rounds must not mutate after start except for responses/progress.

## Question counts

Supported counts:

- 5: quick practice.
- 10: default official mode.
- 25: longer challenge.
- 50: long local mode.

## Difficulty model

Difficulty combines item pool, distractor quality, and UI assists.

### Easy

- Common items.
- Borders visible in map modes.
- Smaller answer sets.
- More explicit prompts.

### Normal

- Mixed items.
- Plausible distractors.
- Borders visible by default.
- Labels off.

### Hard

- Harder/obscure items.
- More similar distractors.
- Optional border hiding.
- Less explicit prompts.

### Brutal

- Obscure/tiny countries and cities.
- No borders.
- No labels.
- Larger answer grids.
- Local-only until validated.

## Region filters

Initial filters:

- World.
- Africa.
- Asia.
- Europe.
- North America.
- Oceania.
- South America.

Antarctica should be excluded from official first-release modes unless a mode explicitly supports it.

## Multiple-choice scoring

```txt
Correct: 1000
Wrong: 0
```

Do not use time in score for first releases.

## Map Tap scoring

```txt
Correct country: 1000
Wrong country: 0
```

## Pinpoint scoring

For country targets:

```txt
Tap inside target country: 1000
Tap outside target country: distance score
```

For city/capital targets:

```txt
score = max(0, round(1000 * (1 - distanceKm / maxDistanceKm)))
```

Suggested defaults:

| Target | maxDistanceKm | Notes |
| --- | ---: | --- |
| Country | 3000 | Only when outside polygon |
| Capital | 1500 | Medium precision |
| City | 1000 | Stricter |
| Tiny country | 500 | Special handling |
| Island country | 750 | Special handling |

Perfect thresholds:

- Country: inside polygon.
- Capital/city: within 25 km.

## Feedback

Correct answer:

- Soft success state.
- Small positive haptic.
- Show correct item name.

Wrong answer:

- Soft error state.
- Small error haptic.
- Show selected answer.
- Show correct answer.
- No harsh wording.

Map answer:

- Show tap.
- Show target.
- Show distance.
- Highlight correct country/point.

## Practice misses

Each app tracks weak items locally.

```swift
struct ArevItemProgress: Codable {
    let itemID: String
    var seenCount: Int
    var correctCount: Int
    var wrongCount: Int
    var lastSeenAt: Date?
    var lastWrongAt: Date?
    var mastery: Double
}
```

Weak item priority:

1. Recently wrong.
2. Low mastery.
3. Not seen recently.
4. New items.

Practice mode never submits to Game Center.

## Local high scores

Store local scores per:

- App ID.
- Mode ID.
- Settings fingerprint.
- Question count.
- Date.

Settings fingerprint should include mode, question count, region, obscurity, map assists, and territory/recognized settings.

## Achievements event model

Shared events:

- First round completed.
- First correct answer.
- First perfect answer.
- Perfect round.
- 10 perfect answers.
- 100 correct answers.
- Completed all countries in an app.
- Correct in every continent.
- No Borders round completed.
- Tiny country correct.
- Practice round completed.

Apps map these events to Game Center achievement IDs.

## Randomness

All generators must support injectable RNG.

Required for:

- deterministic tests.
- reproducible rounds.
- stable bug reports.

## Duplicate prevention

A round should not repeat targets unless explicitly allowed.

Options should not include:

- duplicate countries.
- duplicate display labels.
- the correct answer twice.
- invalid country codes.

## Offline behavior

Gameplay must work fully offline.

If Game Center is unavailable:

- continue gameplay.
- save local score.
- queue official score submission if appropriate.
- avoid blocking result screen.

## Error handling

Invalid data should be caught in tests and exporter validation.

Production behavior:

- Skip invalid questions.
- Hide unavailable modes.
- Avoid player-facing crashes.

Debug behavior:

- Assert/log missing assets.
- Show validation details to developers.
