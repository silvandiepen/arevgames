# Shared Game Systems

This document defines systems that must be implemented once in ArevKit and reused by all apps.

## Round lifecycle

All quiz-style modes follow the same lifecycle:

1. Select app.
2. Select mode.
3. Select preset/settings.
4. Generate fixed round.
5. Present question.
6. Accept answer.
7. Validate answer.
8. Show feedback.
9. Advance.
10. Finish round.
11. Calculate score.
12. Save local progress.
13. Submit Game Center score if this was an official preset and Game Center is available.
14. Offer replay, practice misses, or study.

A generated round should not change after it starts.

## Question counts

Supported shared counts:

- 5: quick practice.
- 10: default official mode.
- 25: longer challenge.
- 50: long study/challenge, local-only unless explicitly added as official.

Game Center official modes should use only fixed counts.

## Difficulty

Difficulty is not one variable. It is a combination of filters and UI assists.

### Easy

- Common countries/cities.
- Same-continent distractors only when helpful.
- Borders visible in map modes.
- Country labels may be available in map study modes.
- Larger answer grids avoided.

### Normal

- Mixed common and regular items.
- Plausible distractors.
- Borders visible by default.
- Labels off.

### Hard

- More obscure items.
- Hard distractors.
- Borders optional/off in map modes.
- Country names may be omitted from capital/city prompts.

### Brutal

- Obscure/tiny countries and cities.
- No borders.
- No labels.
- Larger answer grids.
- Local-only at first unless a preset is deliberately made official.

## Region filters

Shared region filters:

- World.
- Africa.
- Antarctica.
- Asia.
- Europe.
- North America.
- Oceania.
- South America.
- Custom list, future only.

Default: World.

Antarctica should generally be excluded from official modes unless the mode explicitly supports it.

## Recognized countries and territories

Default gameplay should use recognized countries only.

A setting can allow territories/disputed entries later, but do not include them in first official Game Center modes unless the data and map behavior are carefully validated.

## Scoring: multiple choice

Default multiple-choice score:

```txt
Correct: 1000
Wrong: 0
```

Store response details:

- selected answer.
- correct answer.
- response time locally, if measured.
- whether the item should be added to weak history.

Do not use response time in score unless a future document explicitly adds timed modes.

## Scoring: map tap country

For Arev Map Tap:

```txt
Correct country tapped: 1000
Wrong country tapped: 0
```

For Arev Pinpoint / Find the Country:

```txt
Tap inside target country: 1000
Tap outside target country: distance-based score
```

If exact distance-to-polygon is not available in the first version, use centroid fallback with documented limitations and tests.

## Scoring: pinpoint distance

General formula:

```txt
score = max(0, round(1000 * (1 - distanceKm / maxDistanceKm)))
```

Use target-specific `maxDistanceKm`.

Suggested defaults:

| Target type | maxDistanceKm | Notes |
| --- | ---: | --- |
| Country | 3000 | If tap outside polygon; inside is always 1000 |
| Capital | 1500 | Should feel forgiving but meaningful |
| City | 1000 | Stricter than capital mode |
| Tiny country | 500 | Use special inside/near tolerance |
| Island country | 750 | Avoid overly punishing small island targets |

The implementation should tune these after testing.

## Perfect answer definition

A perfect answer is:

- Multiple choice: correct first selection.
- Map Tap: correct country first tap.
- Pinpoint country: tap inside country polygon.
- Pinpoint city/capital: distance below a tight threshold.

Suggested city/capital perfect threshold:

```txt
<= 25 km: perfect
```

For tiny island capitals, tune threshold if needed.

## Feedback system

Feedback should be immediate and calm.

Correct:

- Small positive haptic.
- Soft success state.
- Show country/flag/city name.

Wrong:

- Small error haptic.
- Show correct answer.
- Show selected answer if applicable.
- No harsh wording.

Map modes:

- Draw line from tap to target where useful.
- Show distance in km.
- Highlight correct country or point.

## Practice misses

Every app should track weak items locally.

Track per item:

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

Weak item selection:

1. Recently wrong.
2. Low mastery.
3. Not seen recently.
4. New items.

Practice rounds are local-only and should not submit to Game Center.

## Study mode

Study mode is not a quiz.

Shared study screen should support:

- Browse by region.
- Search.
- Filter weak items.
- Show learned/studied state.
- Tap item for details.

Arev Flags uses this first.

## Achievements triggers

Shared events:

- First round completed.
- First perfect answer.
- Perfect round.
- 10 perfect answers.
- 100 correct answers.
- Completed all countries in an app.
- Correct answer in every continent.
- No Borders round completed.
- Tiny country correct.
- Practice round completed.

The app config maps these events to Game Center achievement IDs.

## Randomness

Use injectable RNG for question generation.

Reason:

- Tests must be deterministic.
- Replays/debugging should be possible.
- Agents should avoid untestable random UI behavior.

## Duplicate prevention

A round should not repeat the same target unless the mode explicitly allows it.

Wrong answer options should not include:

- Correct answer duplicated.
- Duplicate display names.
- Countries outside filter unless needed to reach option count and documented.

## Local high scores

Store local scores per:

- App ID.
- Mode ID.
- Settings fingerprint.
- Question count.
- Date.

Game Center only receives scores for official presets.

## Settings fingerprint

A settings fingerprint is a stable string/hash representing local custom score conditions.

Include:

- mode ID.
- question count.
- region.
- obscurity.
- border/label settings if relevant.
- recognized/territory setting.

Do not include device-specific values.

## Offline behavior

All games must work offline.

If Game Center is unavailable:

- Round still works.
- Local score still saves.
- Submission is queued if the mode is official.
- UI may show `Game Center unavailable` in settings, not during gameplay.

## Sound and haptics

Sound/haptics are shared services.

Minimum events:

- tap.
- correct.
- wrong.
- round complete.
- new best score.

Respect app settings and system reduce motion/silent expectations.

## Error handling

Player-facing errors should be rare.

If data for a mode is invalid:

- Hide the mode or mark unavailable.
- Log internally in debug builds.
- Tests should catch it before release.

Do not crash in production for missing individual country assets; skip invalid questions instead.
