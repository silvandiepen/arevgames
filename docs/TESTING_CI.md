# Testing and CI

Testing should protect the shared harness before multiple apps depend on it.

## Test locations

```txt
libs/ArevKit/Tests/
  ArevKitCoreTests/
  ArevKitDataTests/
  ArevKitMapTests/
  ArevKitGameCenterTests/

tools/export-arev-data/test/
```

## Required tests

### ArevKitCore

- Round lifecycle.
- Question generation.
- Distractor generation.
- Duplicate prevention.
- Settings fingerprints.
- Score calculation.
- Practice-miss selection.
- Mastery calculation.
- Deterministic RNG behavior.

### ArevKitData

- Load bundled JSON.
- Country lookup.
- Flag lookup.
- Similar flag lookup.
- City lookup.
- Capital lookup.
- Geography lookup.
- Region filtering.
- Obscurity filtering.
- Missing data handling.

### ArevKitMap

- Coordinate projection.
- Tap-to-country hit testing.
- Country bounding boxes.
- Country target scoring.
- City/capital distance scoring.
- Tiny country tolerance.
- Island country tolerance.
- No-borders setting does not affect scoring.

### ArevKitGameCenter

- Official preset submits.
- Custom settings do not submit.
- Practice does not submit.
- Failed submission is queued.
- Queue retry succeeds.
- Achievement IDs are mapped correctly.

## Data export tests

The export tool must test:

- Generated JSON schemas.
- Stable sorting.
- Duplicate country detection.
- Unknown country references.
- Missing flag assets.
- Missing map paths for map modes.
- Missing capital coordinates.
- Minimum answer pool sizes.
- Obscurity tier generation.

## UI tests

Add UI tests after the first app shell exists.

Minimum paths:

- Launch Arev Flags.
- Start Flag to Country round.
- Answer one correct question.
- Finish a short round.
- Open settings.
- Toggle sound/haptics.
- View results.

Pinpoint UI tests later:

- Start country pinpoint round.
- Tap map.
- See distance feedback.
- Finish round.

## Manual QA checklist

Before TestFlight:

- Fresh install launches.
- Offline mode works.
- Game Center unavailable does not break gameplay.
- Light mode works.
- Dark mode works.
- Large Dynamic Type works.
- VoiceOver has usable labels.
- Sound/haptics settings work.
- Local progress persists after relaunch.
- Wrong answers appear in Practice Misses.
- Official preset submits or queues Game Center score.
- Custom preset does not submit to Game Center.

## CI phases

Phase 1:

- Swift package tests.
- Data export validation.

Phase 2:

- Build Arev Flags scheme.
- Run ArevKit tests.
- Run exporter tests.

Phase 3:

- Build all app schemes.
- Run all Swift tests.
- Run exporter drift check.

## GitHub Actions

Use `.github/workflows/`.

Suggested workflows:

- `ci.yml`
- `data.yml`
- `ios-build.yml`

Start with one simple workflow if easier.

## Definition of done

A coding task is done when:

- Code is in the correct folder/module.
- Shared logic is not duplicated in app folders.
- Tests exist for logic changes.
- CI still passes locally or in GitHub Actions.
- Documentation is updated if behavior/architecture changed.
- Commit uses conventional commit format.
