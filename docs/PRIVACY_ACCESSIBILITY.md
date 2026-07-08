# Privacy and Accessibility

ArevGames should be safe, simple, offline-first, and accessible.

## Privacy rules

- No ads.
- No third-party analytics.
- No account system.
- No tracking.
- No user-generated public profile.
- No gameplay-critical network calls.
- No location permission.
- No contacts/photos/camera/microphone permission.
- Game Center only for leaderboards/achievements.

## Local data

Allowed local data:

- Settings.
- Local scores.
- Progress.
- Weak item history.
- Study completion.
- Pending Game Center submissions.

Do not store unnecessary personal data.

## Network behavior

Allowed network-related behavior:

- Game Center authentication/submission if used.
- App Store/system services.

Not allowed during gameplay:

- Fetching remote flags.
- Fetching remote map data.
- Calling the hosted ArevData API.
- Sending analytics events.

## Child-safe defaults

- No ads.
- No external web browsing from gameplay.
- No public chat.
- No account creation.
- No dark patterns.
- No manipulative streaks.
- No required social features.
- No aggressive notifications.

## Accessibility requirements

### VoiceOver

Every interactive element needs a useful accessibility label.

Examples:

- `Flag of Armenia`.
- `Armenia`.
- `Your tap was 132 kilometers from Valletta`.
- `Flag to Country, identify the country from its flag`.

### Dynamic Type

- Do not truncate critical question text.
- Answer cards should grow or wrap.
- Settings rows should remain readable.

### Reduce Motion

When enabled:

- Avoid large animated map transitions.
- Replace bouncy feedback with simple fades.
- Avoid score count-up animations.

### Color blindness

Do not rely on color alone.

- Correct/wrong states should use text/icons/shape as well as color.
- Map highlights should include labels or outlines where possible.
- Flag color modes, if added later, need explicit text labels.

### Touch targets

Minimum target size: 44x44 points.

Map modes need zoom/pan or tolerance handling for tiny countries.

## App Store privacy direction

Expected privacy posture:

- No data collected by the app, except Apple/Game Center data handled by Apple services if enabled.
- No tracking.
- No third-party analytics.
- No account.

Before release, verify the actual implementation matches the App Store privacy labels.

## Review notes template

```txt
This app is an offline geography learning game. It does not contain ads, subscriptions, account creation, third-party analytics, or tracking. Gameplay data and progress are stored locally on device. Game Center is used only for optional leaderboards and achievements. The app does not require location, camera, microphone, contacts, or photo permissions.
```

## Accessibility test checklist

Before release:

- Test VoiceOver on home, round, and result screens.
- Test large Dynamic Type.
- Test Reduce Motion.
- Test light and dark mode.
- Test map modes with tiny countries.
- Test with sound off.
- Test without Game Center login.
