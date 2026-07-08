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

## Data stored locally

Allowed local data:

- Settings.
- Local scores.
- Progress.
- Weak item history.
- Study completion.
- Pending Game Center submissions.

Do not store unnecessary personal data.

## Network behavior

The app should work offline.

Allowed network-related behavior:

- Game Center authentication/submission if the player uses Game Center.
- App Store/system services.

Not allowed during gameplay:

- Fetching remote flags.
- Fetching remote map data.
- Calling hosted ArevData API.
- Sending analytics events.

## Child-safe defaults

Even if the apps are not only for children, they should be safe for children.

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

- Flag image: `Flag of Armenia`.
- Country answer: `Armenia`.
- Map tap target after answer: `Your tap was 132 kilometers from Valletta`.
- Mode card: `Flag to Country, identify the country from its flag`.

Avoid labels like `button`, `image`, or `icon` without context.

### Dynamic Type

Support Dynamic Type where practical.

- Do not truncate critical question text.
- Answer cards should grow or wrap.
- Settings rows should remain readable.

### Reduce Motion

Respect system Reduce Motion.

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

### Haptics and sound

Sound/haptics must be optional.

- Respect settings.
- Avoid required audio cues.
- Do not use audio as the only feedback.

## App Store privacy declaration direction

Expected privacy posture:

- No data collected by app, except Apple/Game Center data handled by Apple services if enabled.
- No tracking.
- No third-party analytics.
- No account.

Before release, verify the actual implementation matches the App Store privacy labels.

## Review notes direction

Use review notes like:

```txt
This app is an offline geography learning game. It does not contain ads, subscriptions, account creation, third-party analytics, or tracking. Gameplay data and progress are stored locally on device. Game Center is used only for optional leaderboards and achievements. The app does not require location, camera, microphone, contacts, or photo permissions.
```

Update this if implementation changes.

## Accessibility test checklist

Before release:

- Test VoiceOver on each app home screen.
- Test VoiceOver during a round.
- Test VoiceOver on result screen.
- Test large Dynamic Type.
- Test Reduce Motion.
- Test light and dark mode.
- Test map modes with tiny countries.
- Test with sound off.
- Test without Game Center login.
