# UI and Design System

ArevGames should feel native, calm, and polished. The UI should be shared through ArevKitUI.

The design should be close to the Luys/Mazzi direction: clean native game UI, small focused screens, rounded cards, tactile interactions, useful popups, and no educational-dashboard clutter.

## Design direction

- Native SwiftUI.
- Simple, flat, tactile.
- Clear spacing.
- Large tappable targets.
- Calm animation.
- No clutter.
- No fake education-dashboard look.
- No timer-pressure visuals in first releases.
- Small game feel, not atlas/documentation app feel.
- Shared UI components only; no app-specific duplicated design systems.

## Shared UI location

```txt
libs/ArevKit/Sources/ArevKitUI/
```

Do not create reusable UI inside app folders.

## Required UI dependencies

### SVGKit

Use SVGKit for SVG rendering in native iOS views.

Repository:

```txt
https://github.com/SVGKit/SVGKit
```

Primary uses:

- Flag SVG rendering when bundled SVGs are used directly.
- Country shape rendering.
- World map SVG/path rendering where SVGKit is the best fit.
- Any interactive SVG rendering that should stay native rather than WebView-based.

Rules:

- Keep SVGKit usage behind ArevKit abstractions.
- App targets should not import SVGKit directly.
- Create shared wrappers such as `ArevSVGView`, `ArevFlagView`, `ArevCountryShapeView`, and `ArevWorldMapView`.
- Gameplay-critical SVGs must be bundled locally.
- Do not load flag/map SVGs from remote URLs during gameplay.
- If SVGKit cannot support a specific hit-testing requirement cleanly, implement the hit-testing layer in ArevKitMap while still using SVGKit for rendering where useful.

Preferred wrapper direction:

```swift
struct ArevSVGView: View {
    let asset: ArevSVGAsset
    let renderingMode: ArevSVGRenderingMode
}
```

Do not scatter UIKit/SVGKit bridge code across app screens.

### PopupView

Use the same popup package pattern as Luys.

Package:

```txt
https://github.com/exyte/PopupView.git
exactVersion: 5.0.2
```

Luys uses `PopupView` through a shared wrapper. ArevGames should follow the same pattern with ArevKit naming.

Create shared modifiers:

```swift
public extension View {
    func arevBottomPopup<PopupContent: View>(
        isPresented: Binding<Bool>,
        closeOnTapOutside: Bool = true,
        dragToDismiss: Bool = true,
        @ViewBuilder content: @escaping () -> PopupContent
    ) -> some View

    func arevCenterPopup<PopupContent: View>(
        isPresented: Binding<Bool>,
        closeOnTapOutside: Bool = false,
        @ViewBuilder content: @escaping () -> PopupContent
    ) -> some View
}
```

Popup behavior should mirror Luys:

- Compact width: bottom toast/sheet-style popup.
- Regular width: floating popup aligned near top-leading for navigation/settings style popups.
- Center popup: centered floater with subtle dim background.
- Use spring animation, but respect Reduce Motion.
- Use `closeOnTapOutside` and `dragToDismiss` only where appropriate.

ArevKit should own the popup wrapper. App targets should not call `popup(...)` directly.

## Splash screen

Every iOS app should use the ArevData/arev logo from arevdata.com/source assets on the splash/launch screen.

Rules:

- Use the actual ArevData/arev logo asset.
- Do not redraw, approximate, or reinterpret the logo.
- Keep the splash simple: background color + centered logo.
- Use the same family splash treatment across all ArevGames apps.
- App-specific identity can appear after launch, not by changing the family logo.
- Store the canonical splash/logo source in `data/source/brand/arev-logo.svg` or the final selected asset-catalog source path.
- Use SVGKit/shared SVG wrappers where a Swift-rendered splash/interstitial view is needed after launch.

If iOS launch screen constraints require asset catalog PNG/PDF outputs, generate them from the canonical SVG source.

## Screens

### App home

Show app name, short description, mode cards, progress, and settings access. It should not be a complicated dashboard.

### Mode picker

Select mode, explain mode, show official/custom score status where relevant.

### Settings popup

Settings should use the shared popup system, not a custom app-specific sheet.

Show only relevant settings:

- question count.
- region.
- obscurity.
- map assists.
- sound/haptics.
- Game Center status.

### Round view

Present one question clearly, show answer area, show round progress, keep controls minimal.

### Result popup/view

Results may be a full result screen or a shared popup depending on mode flow.

Show:

- score.
- correct count.
- best score.
- replay.
- practice misses.
- study where relevant.

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
- `ArevSVGView`.
- `ArevCountryShapeView`.
- `ArevWorldMapView`.
- `ArevSplashView`.
- `ArevPopupCard`.
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
- Use adaptive popup behavior for compact vs regular size classes.

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
    var popupBackground: Color
    var popupOverlay: Color
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
- Popup presentation.
- Score count-up if not distracting.

## Flag rendering

- Flags should be large, crisp, local, and rendered through shared ArevKit views.
- Preserve aspect ratio.
- Do not crop main question flags.
- Accessibility label: `Flag of Malta`.
- Avoid tiny flags in answer grids.
- Prefer bundled SVG rendered via SVGKit where possible; fallback generated PNGs are allowed if SVG rendering has performance or compatibility issues.

## Country shape rendering

- Use local map/SVG path data.
- Render through `ArevCountryShapeView`.
- Use SVGKit when it fits the rendering path.
- Keep hit testing in ArevKitMap, not in app targets.

## Map rendering

Map modes need:

- Pan and zoom where targets may be small.
- Border visibility toggle.
- Label visibility toggle.
- Clear selected tap marker.
- Correct target highlight after answer.
- Distance line for Pinpoint result feedback.

Small countries need special interaction handling.

## Popup usage

Use popups for:

- settings.
- pause/menu.
- mode options.
- short contextual help.
- result summary when a full screen is unnecessary.
- Game Center status/details.

Do not use popups for:

- every correct/wrong answer.
- heavy study browsing.
- long documents.
- anything that should be a normal screen.

## App-specific theming

Apps may have subtle accent differences, but not separate design systems.

- Arev Flags: flag/color accent.
- Arev Pinpoint: map/location accent.
- Arev Guess the Country: clue accent.
- Arev Map Tap: friendly map accent.
