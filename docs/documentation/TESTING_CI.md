# Testing and CI

Testing should protect the shared harness before multiple apps depend on it.

## Test locations

```txt
libs/ArevKit/Tests/
  ArevKitCoreTests/
  ArevKitDataTests/
  ArevKitMapTests/
  ArevKitGameCenterTests/
```

Tooling tests can live near the tool:

```txt
tools/export-arev-data/test/
```

## Required unit tests

### ArevKitCore

Test:

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

Test:

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

Test:

- Coordinate projection.
- Tap-to-country hit testing.
- Country bounding boxes.
- Country target scoring.
- City/capital distance scoring.
- Tiny country tolerance.
- Island country tolerance.
- No borders setting does not affect scoring.

### ArevKitGameCenter

Test with fake client:

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
- Missing capital coordinates for capital modes.
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

## Snapshot tests

Optional, but useful for shared UI.

Snapshot candidates:

- Mode card.
- Question card.
- Answer grid.
- Result view.
- Settings sheet.
- Flag card.
- Map feedback view.

Do not block MVP on snapshot infrastructure.

## Manual QA checklist

Before each TestFlight build:

- Fresh install launches.
- Offline mode works.
- Game Center unavailable does not break gameplay.
- Light mode works.
- Dark mode works.
- Large Dynamic Type works.
- VoiceOver has usable labels.
- Sound/haptics settings work.
- Local progress persists after app relaunch.
- Wrong answers appear in Practice Misses.
- Official preset submits or queues Game Center score.
- Custom preset does not submit to Game Center.

## CI phases

### Phase 1: foundation

- Swift package tests.
- Data export validation.

### Phase 2: first app

- Build Arev Flags scheme.
- Run ArevKit tests.
- Run exporter tests.

### Phase 3: multi-app

- Build all app schemes.
- Run all Swift tests.
- Run exporter drift check.

## GitHub Actions direction

Use `.github/workflows/`.

Suggested workflows:

```txt
.github/workflows/ci.yml
.github/workflows/data.yml
.github/workflows/ios-build.yml
```

Start with one simple workflow if easier.

## Data drift check

CI should be able to run:

```bash
cd tools/export-arev-data
npm ci
npm test
npm run export
```

Then verify no generated files changed unexpectedly.

## Build matrix

Initial target:

- latest stable macOS runner available in GitHub Actions.
- latest supported Xcode on that runner.
- iOS simulator build.

Do not overcomplicate CI before code exists.

## Definition of done for agents

A coding task is done when:

- Code is in the correct folder/module.
- Shared logic is not duplicated in app folders.
- Tests exist for logic changes.
- CI still passes locally or in GitHub Actions.
- Documentation is updated if behavior/architecture changed.
- Commit uses conventional commit format.
