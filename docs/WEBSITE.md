# Website

ArevGames needs a small static marketing website, following the same broad requirements and stack as the Luys site.

## Goal

Build a small, fast, static website for the ArevGames family.

The website should:

- Present ArevGames as a clean, native, no-ads geography game family.
- Link to each app when available.
- Explain the no ads / no subscriptions / no tracking position.
- Provide privacy pages for App Store listings.
- Link to ArevData as the source data project.
- Be easy to update by editing one app/game data file.

## Location

Use a top-level `site/` folder, matching Luys.

```txt
site/
  index.html
  package.json
  tsconfig.json
  tsconfig.node.json
  vite.config.ts
  public/
    favicon.svg
    logo.svg
    robots.txt
  src/
    main.ts
    App.vue
    router.ts
    style.scss
    assets/
      logo.svg
    components/
      SiteHeader.vue
      SiteFooter.vue
      GameCard.vue
      AppStoreButton.vue
      HeroMark.vue
    views/
      HomeView.vue
      GamesIndexView.vue
      GameView.vue
      PrivacyView.vue
      NotFoundView.vue
    data/
      games.ts
    types/
      game.ts
```

Do not put the website under `apps/`. `apps/` is for native app targets.

## Stack

Use the same style of stack as Luys:

| Concern | Choice |
| --- | --- |
| Framework | Vue 3 with `<script setup>` and TypeScript |
| Build tool | Vite |
| Routing | Vue Router 4, history mode |
| Styles | Sass + `@sil/ui` shared styles and theme tokens |
| UI components | `@sil/ui` with its Vite plugin / `defineTheme` |
| Type checking | `vue-tsc` |
| Package alias | `@` → `src/` |
| Fonts | Inter body, Manrope headings, unless ArevData has a stronger existing font direction |
| Hosting | Cloudflare Pages, static output |

Scripts:

```jsonc
{
  "dev": "vite",
  "build": "vue-tsc && vite build",
  "preview": "vite preview",
  "typecheck": "vue-tsc --noEmit"
}
```

## Routes

```txt
/                  HomeView         family intro + featured apps
/apps              GamesIndexView   list of ArevGames apps
/apps/:app         GameView         per-app page
/privacy           PrivacyView      privacy reference for App Store
:catchAll(.*)      NotFoundView
```

## App data model

One file should drive the home page, apps index, and app pages.

```ts
export interface ArevGameSiteEntry {
  slug: string
  name: string
  tagline: string
  status: 'planned' | 'development' | 'testflight' | 'live'
  appStoreUrl?: string
  appStoreId?: string
  accent: string
  blurb: string
  highlights: string[]
  modes: string[]
}

export const games: ArevGameSiteEntry[] = [
  // arev-flags, arev-pinpoint, arev-guess-the-country, arev-map-tap
]
```

Adding an app page should be a data edit, not a new hardcoded route.

## Initial app entries

Seed the site with:

- Arev Flags.
- Arev Pinpoint.
- Arev Guess the Country.
- Arev Map Tap.

Statuses should start as `planned` or `development` until apps are real.

## Content direction

Core message:

```txt
Small native geography games. Learn flags, countries, capitals, cities, and maps without ads, subscriptions, accounts, or tracking.
```

Tone:

- Calm.
- Native.
- Clean.
- Premium but simple.
- Not school software.
- Not a giant atlas.

## Privacy page

The privacy page should state:

- No ads.
- No third-party analytics.
- No tracking.
- No account.
- Local progress stays on device.
- Game Center is optional and handled by Apple when enabled.
- The apps do not need location, camera, microphone, contacts, or photos.

## ArevData relation

The site should clearly mention that ArevGames is powered by ArevData.

Link to:

- `https://arevdata.com`
- `https://github.com/silvandiepen/arev`

Do not make the game site look like developer documentation. ArevData can be the technical source; ArevGames should be the consumer-facing game family.

## Branding and logo

Use the ArevData/arev logo as the family mark.

Required asset flow:

1. Copy/export the logo from ArevData / arevdata.com source assets.
2. Store the canonical copy in `site/public/logo.svg` and `site/src/assets/logo.svg`.
3. Store the iOS splash/source copy in `data/source/brand/arev-logo.svg` or the final chosen native asset catalog source path.
4. Do not redraw or approximate the logo manually.

If the source asset location is unclear, the agent should inspect the ArevData repo/site output and copy the actual logo asset from there.

## Theme

Use an ArevData-compatible theme, not Luys colors.

Suggested first direction:

- neutral off-white background.
- deep dark mode background.
- geographic/map accent.
- clean cards.
- app-specific accents for each game.

Use `@sil/ui` theme wiring with `defineTheme`, matching the Luys site pattern.

## Build and deploy

Static output from `vite build`.

Target host: Cloudflare Pages.

Suggested Cloudflare Pages config:

```txt
Build command: npm run build
Output dir: dist
Root dir: site
SPA fallback: /* -> /index.html
```

Domain can be decided later. Do not block implementation on the final domain.

## CI requirements

CI should run the site checks when `site/package.json` exists:

```bash
cd site
npm ci
npm run typecheck
npm run build
```

If a lockfile/package manager is chosen differently, document it and keep it consistent.

## Out of scope for v1

- Accounts.
- Backend.
- CMS.
- Blog.
- Analytics.
- Forms that store data.
- i18n.

## Execution plan

1. Scaffold `site/` with Vite + Vue 3 + TypeScript.
2. Add Vue Router.
3. Add Sass and `@sil/ui`.
4. Wire theme tokens.
5. Copy ArevData logo asset.
6. Add shell components.
7. Add app data file.
8. Add home, apps index, app detail, privacy, not found routes.
9. Add SEO/meta defaults.
10. Add robots/favicons.
11. Add CI site build.
12. Configure Cloudflare Pages.
