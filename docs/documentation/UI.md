# UI and Design System

ArevGames should feel native, calm, and polished. The UI should be shared through ArevKitUI.

## Design direction

- Native SwiftUI.
- Simple, flat, tactile.
- Clear spacing.
- Large tappable targets.
- Calm animation.
- No clutter.
- No fake education-dashboard look.
- No timer-pressure visuals in first releases.

## Shared UI location

```txt
libs/ArevKit/Sources/ArevKitUI/
```

Do not create reusable UI inside app folders.

## Screens

### App home

Purpose:

- Show app name.
- Show short app-specific description.
- Show mode cards.
- Show progress/settings access.

Should not be a complicated dashboard.

### Mode picker

Purpose:

- Select mode.
- Show clear mode description.
- Show official/custom score status when relevant.

### Settings sheet

Purpose:

- Question count.
- Region.
- Obscurity.
- Map assists where relevant.
- Sound/haptics.
- Game Center status.

Do not show irrelevant settings for a mode.

### Round view

Purpose:

- Present one question clearly.
- Present answer area.
- Show progress in the round.
- Keep controls minimal.

### Result view

Purpose:

- Show score.
- Show correct count.
- Show best score if relevant.
- Offer replay.
- Offer practice misses if there are misses.
- Offer study when relevant.

### Study view

Purpose:

- Browse by region.
- Search.
- Show weak items.
- Track studied items locally.

## Components

Create shared components:

- `ArevButton`.
- `ArevIconButton`.
- `ArevCard`.
- `ArevModeCard`.
- `ArevQuestionCard`.
- `ArevAnswerGrid`.
- `ArevFlagView`.
- `ArevCountryShapeView`.
- `ArevWorldMapView`.
- `ArevScorePill`.
- `ArevProgressIndicator`.
- `ArevSettingsRow`.
- `ArevSegmentedPicker`.
- `ArevFeedbackBanner`.

## Layout rules

- Minimum tappable target: 44x44 points.
- Prefer large answer cards over small text buttons.
- Keep important controls reachable.
- Avoid dense tables in gameplay.
- Avoid small map targets without zoom/pan support.
- Use safe areas correctly.
- Support iPhone and iPad from the start if practical.

## Typography

Use system fonts.

Suggested semantic styles:

- App title.
- Mode title.
- Question prompt.
- Answer title.
- Caption/detail.
- Score number.

Do not hardcode too many font sizes. Support Dynamic Type.

## Color

Use semantic tokens, not random one-off colors.

Suggested tokens:

```swift
struct ArevTheme {
    var background: Color
    var surface: Color
    var elevatedSurface: Color
    var textPrimary: Color
    var textSecondary: Color
    var accent: Color
    var success: Color
    var warning: Color
    var error: Color
    var mapLand: Color
    var mapBorder: Color
    var mapHighlight: Color
}
```

Support light and dark mode.

## Motion

Use motion sparingly:

- Button press feedback.
- Correct/wrong feedback.
- Card transitions.
- Map target reveal.
- Score count-up if not distracting.

Respect Reduce Motion.

## Haptics and sound

Shared events:

- Tap.
- Correct.
- Wrong.
- Round complete.
- New best score.

Respect app settings.

## Flag rendering

Flags should be large, crisp, and local.

Requirements:

- Preserve flag aspect ratio.
- Do not crop flags unless deliberately using a flag-card style.
- Provide accessibility label: `Flag of Malta`.
- Avoid tiny flags in answer grids.

## Map rendering

Map modes need:

- Pan and zoom where targets may be small.
- Border visibility toggle.
- Label visibility toggle.
- Clear selected tap marker.
- Correct target highlight after answer.
- Distance line for Pinpoint result feedback.

Small countries need special interaction handling. The player should not feel punished by impossible tap targets.

## Empty/error states

Mode unavailable:

- Explain briefly.
- Hide in production if caused by missing data.

No weak items:

- Show positive empty state.
- Offer normal practice.

Game Center unavailable:

- Show status in settings/progress.
- Do not interrupt gameplay.

## App-specific theming

Apps may have subtle accent differences, but not separate design systems.

Example:

- Arev Flags: flag/color accent.
- Arev Pinpoint: map/location accent.
- Arev Guess the Country: neutral clue accent.
- Arev Map Tap: friendly map accent.

The component system must remain shared.
