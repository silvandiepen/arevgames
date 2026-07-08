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

Show app name, short description, mode cards, progress, and settings access. It should not be a complicated dashboard.

### Mode picker

Select mode, explain mode, show official/custom score status where relevant.

### Settings sheet

Show only relevant settings: question count, region, obscurity, map assists, sound/haptics, Game Center status.

### Round view

Present one question clearly, show answer area, show round progress, keep controls minimal.

### Result view

Show score, correct count, best score, replay, practice misses, and study where relevant.

### Study view

Browse by region, search, weak items, studied state, item details.

## Shared components

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
- Support iPhone and iPad from the start where practical.

## Typography

Use system fonts and semantic styles:

- App title.
- Mode title.
- Question prompt.
- Answer title.
- Caption/detail.
- Score number.

Support Dynamic Type.

## Color

Use semantic tokens, not one-off colors.

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

Use motion sparingly and respect Reduce Motion.

Allowed uses:

- Button press feedback.
- Correct/wrong feedback.
- Card transitions.
- Map target reveal.
- Score count-up if not distracting.

## Flag rendering

- Flags should be large, crisp, and local.
- Preserve aspect ratio.
- Do not crop main question flags.
- Accessibility label: `Flag of Malta`.
- Avoid tiny flags in answer grids.

## Map rendering

Map modes need:

- Pan and zoom where targets may be small.
- Border visibility toggle.
- Label visibility toggle.
- Clear selected tap marker.
- Correct target highlight after answer.
- Distance line for Pinpoint result feedback.

Small countries need special interaction handling.

## App-specific theming

Apps may have subtle accent differences, but not separate design systems.

- Arev Flags: flag/color accent.
- Arev Pinpoint: map/location accent.
- Arev Guess the Country: clue accent.
- Arev Map Tap: friendly map accent.
