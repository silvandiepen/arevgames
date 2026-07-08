# Product Scope

ArevGames is a family of small native iOS geography-learning games powered by ArevData.

This is not one large atlas product. It is a clean monorepo with focused apps, a shared ArevKit library, generated local data, tools, a static marketing website, CI, and one documentation root.

## Product rules

- One app = one learning objective.
- Modes may vary the mechanic, but must support the same objective.
- Shared functionality belongs in ArevKit.
- App targets should be thin.
- Gameplay must work offline.
- Game Center is optional and only for official score modes.
- The static website belongs in `site/`, not in native app targets.
- GitHub CI should build and test as much as practical because the repo is open source.

## Principles

- Native iOS first.
- Small apps.
- Shared implementation.
- Paid once by default.
- No ads.
- No subscriptions.
- No account requirement.
- No tracking.
- No timer pressure in first releases.
- Calm learning, not noisy gamification.
- Useful progress and practice misses.
- Public/open-source friendly build setup.

## Planned apps

### 1. Arev Flags

Objective: learn flags.

Modes: Flag to Country, Country to Flag, Flag Grid, Flag Match, Study Flags, Practice Misses.

This is the first vertical slice.

### 2. Arev Pinpoint

Objective: learn where countries, capitals, and cities are.

Modes: Find the Country, Find the Capital, Find the City, No Borders, Obscure Places, Practice Misses.

This is the distinctive distance-scored map game.

### 3. Arev Guess the Country

Objective: identify countries from different clues.

Modes: By Flag, By Shape, By Capital, By Map Clue, Mixed.

### 4. Arev Map Tap

Objective: simple country recognition on a map.

Modes: Find Country, Continent Focus, No Borders Lite, Practice Misses.

This is simpler and more kid-friendly than Pinpoint.

## Website

ArevGames also needs a small static marketing website.

The site should:

- Present the ArevGames family.
- List the apps.
- Link to App Store pages when apps are live.
- Provide privacy pages for App Store listings.
- Explain the no ads / no subscriptions / no accounts / no tracking position.
- Link to ArevData as the source data project.

The website stack and requirements are documented in `docs/WEBSITE.md`.

## Branding

Use the actual ArevData/arev logo from arevdata.com/source assets as the family mark and app splash mark.

Do not redraw or approximate the logo.

## Non-goals for first phase

- One giant atlas app.
- Android.
- Multiplayer.
- Accounts.
- Analytics dashboards.
- Subscriptions.
- School/LMS features.
- User-generated content.
- Timed competitive modes.
- Full localization infrastructure.
- Backend/CMS for the website.

## Future candidates

Only build later if the foundation and first apps work well:

- Arev Capitals.
- Arev Shapes.
- Arev Neighbours.
- Arev Space.

## First release scope

First public release should target:

- Clean monorepo.
- GitHub CI.
- ArevKit foundation.
- ArevData export pipeline.
- Bundled local data/assets.
- ArevData logo copied/exported for splash and site.
- Static website scaffold.
- Arev Flags app.
- Local scores/progress.
- Game Center adapter behind a protocol.
- At least one official leaderboard-ready preset.
- App Store metadata and review notes.

Pinpoint should not block the first Arev Flags release.
