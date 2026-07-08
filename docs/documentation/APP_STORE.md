# App Store and Release Notes

This document defines release and App Store requirements for ArevGames apps.

## Product positioning

Shared positioning:

```txt
Clean native geography games. No ads, no subscriptions, no accounts, no tracking.
```

Each app should have a narrow App Store promise.

Do not describe any first app as a complete atlas or full geography suite.

## App names

Initial app names:

- Arev Flags.
- Arev Pinpoint.
- Arev Guess the Country.
- Arev Map Tap.

## Bundle ID pattern

Proposed pattern:

| App | Bundle ID |
| --- | --- |
| Arev Flags | `app.arev.flags` |
| Arev Pinpoint | `app.arev.pinpoint` |
| Arev Guess the Country | `app.arev.guesscountry` |
| Arev Map Tap | `app.arev.maptap` |

Final IDs must be verified before creating App Store records.

## App Store copy direction

### Arev Flags

Short promise:

```txt
Learn world flags in a clean, calm native game.
```

Description points:

- Guess country from flag.
- Guess flag from country.
- Practice similar flags.
- Study by continent.
- No ads.
- No subscriptions.
- Works offline.
- Optional Game Center scores.

### Arev Pinpoint

Short promise:

```txt
Tap where countries, capitals, and cities are. The closer you are, the better the score.
```

Description points:

- Find countries on the map.
- Find capitals and cities.
- Distance-based scoring.
- No Borders challenge.
- Official Game Center leaderboards for fixed presets.
- Works offline.

### Arev Guess the Country

Short promise:

```txt
Identify countries by flag, shape, capital, and map clues.
```

### Arev Map Tap

Short promise:

```txt
A simple map game for learning where countries are.
```

## Privacy labels

Expected posture:

- No data collected by the app.
- No tracking.
- No third-party analytics.
- Game Center is optional and handled through Apple services.

Before submission, verify the final implementation matches privacy labels exactly.

## Review notes template

```txt
This app is an offline geography learning game. It does not contain ads, subscriptions, account creation, third-party analytics, or tracking. Gameplay data and progress are stored locally on device. Game Center is used only for optional leaderboards and achievements. The app does not require location, camera, microphone, contacts, or photo permissions.
```

Add app-specific mode notes if needed.

## Screenshots

Required screenshot themes:

- Home/mode selection.
- Main gameplay.
- Correct/wrong feedback.
- Result screen.
- Study/progress screen if relevant.
- Map interaction for map apps.

Screenshots should communicate:

- clear gameplay.
- no clutter.
- native quality.
- calm learning.

## App icons

Each app needs its own icon while sharing Arev family style.

Direction:

- Simple.
- Bold.
- Readable at small sizes.
- Avoid text in icon.
- Use flat/clean shapes.

App icon ideas:

- Arev Flags: flag/card motif.
- Arev Pinpoint: map pin on simplified globe/map.
- Arev Guess the Country: country silhouette/question cue.
- Arev Map Tap: finger/tap on map tile.

## Game Center setup

Before release:

- Create leaderboards for official presets.
- Create achievements.
- Ensure code IDs match App Store Connect IDs.
- Test with sandbox Game Center account.
- Verify app works without Game Center login.

## Versioning

Use simple semantic-ish marketing versions:

- `1.0` first release.
- Patch build numbers for fixes.

Internal build numbers should be monotonic.

## TestFlight checklist

Before external testing:

- App launches on clean install.
- No debug-only assets visible.
- Offline gameplay works.
- Game Center optional flow works.
- Privacy labels match implementation.
- Review notes are complete.
- Basic screenshots exist.
- No placeholder app icon.
- No unused permissions in plist.

## Release order

1. Arev Flags.
2. Arev Pinpoint.
3. Arev Guess the Country.
4. Arev Map Tap.

Do not hold Arev Flags for later app completeness.
