# Shared Game Systems

All shared gameplay behavior belongs in ArevKit, not in app targets.

## Round lifecycle

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

Easy:

- common items.
- borders visible in map modes.
- smaller answer sets.
- explicit prompts.

Normal:

- mixed items.
- plausible distractors.
- borders visible by default.
- labels off.

Hard:

- harder/obscure items.
- more similar distractors.
- optional border hiding.
- less explicit prompts.

Brutal:

- obscure/tiny countries and cities.
- no borders.
- no labels.
- larger answer grids.
- local-only until validated.

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

## Scoring

### Multiple choice

```txt
Correct: 1000
Wrong: 0
```

Do not use time in score for first releases.

### Map Tap

```txt
Correct country: 1000
Wrong country: 0
```

### Pinpoint

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

- soft success state.
- small positive haptic.
- show correct item name.

Wrong answer:

- soft error state.
- small error haptic.
- show selected answer.
- show correct answer.
- no harsh wording.

Map answer:

- show tap.
- show target.
- show distance.
- highlight correct country/point.

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

## Achievement event model

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

All generators must support injectable RNG for deterministic tests and reproducible rounds.

## Duplicate prevention

A round should not repeat targets unless explicitly allowed.

Options should not include duplicate countries, duplicate labels, the correct answer twice, or invalid country codes.

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

- assert/log missing assets.
- show validation details to developers.
