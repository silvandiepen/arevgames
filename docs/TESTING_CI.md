# Testing and CI

Testing should protect the shared harness before multiple apps depend on it.

Because this repository is open source, CI should be fairly aggressive. Build and test as much as GitHub-hosted runners allow, including iOS app builds once the projects/schemes exist.

## Strategy

Use a layered strategy:

1. **Static validation** — repo layout, docs, generated-data drift, dependency placement.
2. **Exporter tests** — ArevData export and generated schema validation.
3. **Pure Swift unit tests** — ArevKitCore logic.
4. **Data tests** — bundled JSON loading and lookup behavior.
5. **Map tests** — projection, hit testing, distance scoring, tiny-country edge cases.
6. **Game Center tests** — fake client and submission rules.
7. **SwiftUI/UI tests** — core gameplay flows once apps exist.
8. **iOS build CI** — build every app scheme on macOS runners.
9. **Website CI** — typecheck and build the static Vue/Vite site.
10. **Manual TestFlight QA** — final real-device checks before external testing.

## Test locations

```txt
libs/ArevKit/Tests/
  ArevKitCoreTests/
  ArevKitDataTests/
  ArevKitMapTests/
  ArevKitGameCenterTests/

tools/export-arev-data/test/

site/
  src/
  package.json
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
- Antimeridian handling where applicable.

### ArevKitGameCenter

- Official preset submits.
- Custom settings do not submit.
- Practice does not submit.
- Failed submission is queued.
- Queue retry succeeds.
- Achievement IDs are mapped correctly.
- Fake client works without GameKit.

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
- Generated asset paths.
- ArevData logo copy/export path for splash/site use.

## Website tests

When `site/package.json` exists, CI should run:

```bash
cd site
npm ci
npm run typecheck
npm run build
```

The website should not require a backend or secrets to build.

Optional later:

- link check.
- accessibility scan.
- screenshot smoke test.
- Cloudflare Pages preview deployment.

## UI tests

Add UI tests after the first app shell exists.

Minimum Arev Flags paths:

- Launch Arev Flags.
- Start Flag to Country round.
- Answer one correct question.
- Finish a short round.
- Open settings popup.
- Toggle sound/haptics.
- View results.
- Verify splash/logo appears on launch.

Pinpoint UI tests later:

- Start country pinpoint round.
- Tap map.
- See distance feedback.
- Finish round.

## Manual QA checklist

Before TestFlight:

- Fresh install launches.
- Splash uses the ArevData/arev logo.
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
- Website builds and privacy page exists.

## GitHub Actions

Use `.github/workflows/`.

Start with one workflow:

```txt
.github/workflows/ci.yml
```

It should be safe before code exists by conditionally running jobs only when relevant files exist.

Required checks:

1. Docs/repo-layout validation on Ubuntu.
2. Data exporter validation when `tools/export-arev-data/package.json` exists.
3. Site typecheck/build when `site/package.json` exists.
4. Swift package tests when `Package.swift` or `libs/ArevKit/Package.swift` exists.
5. iOS app builds on macOS when `ArevGames.xcworkspace` or app project files exist.

## CI phases

### Phase 1: docs and structure

- Verify required docs exist.
- Verify `docs/documentation/` does not exist.
- Verify root structure conventions.

### Phase 2: data and site

- Run data export tests.
- Run data export drift check.
- Typecheck/build website.

### Phase 3: shared Swift package

- Run ArevKit Swift tests.
- Build ArevKit.

### Phase 4: app builds

- Build Arev Flags.
- Build each additional app as it is added.
- Run iOS simulator tests where available.

### Phase 5: release hardening

- Build all schemes.
- Run all unit/UI tests.
- Archive checks if useful.
- Upload build artifacts when appropriate.

## Suggested CI commands

Swift package:

```bash
swift test --package-path libs/ArevKit
```

Xcode workspace, once schemes exist:

```bash
xcodebuild \
  -workspace ArevGames.xcworkspace \
  -scheme ArevFlags \
  -destination 'platform=iOS Simulator,name=iPhone 16' \
  build test
```

Data exporter:

```bash
cd tools/export-arev-data
npm ci
npm test
npm run export
```

Website:

```bash
cd site
npm ci
npm run typecheck
npm run build
```

## CI artifact policy

Upload artifacts when useful:

- site build output for debugging.
- test reports if generated.
- simulator screenshots from UI tests.
- generated data diff/report.

Do not upload signing secrets or App Store credentials.

## Definition of done

A coding task is done when:

- Code is in the correct folder/module.
- Shared logic is not duplicated in app folders.
- Tests exist for logic changes.
- CI still passes locally or in GitHub Actions.
- Documentation is updated if behavior/architecture changed.
- Commit uses conventional commit format.
