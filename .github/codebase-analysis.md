# Codebase Analysis (2025-09-27)

## Overvie## Observations & Next Steps

1. Consider updating README with dev/build instructions tailored to Alex's personal site.
2. Ensure removed blog slugs aren't referenced externally (e.g., in `ListPosts` dataset snapshots) before deployment.
3. Document photo-management scripts prerequisites (Sharp/libvips) for smoother onboarding.
4. **Complete sponsor removal completed (2025-09-27)**: This is now Alex Yang's personal website with all sponsor functionality removed:
   - Removed pages: `sponsors-list.md`, `collective-sponsor-onetime.md`
   - Removed components: `SponsorsView.vue`, `SponsorsCircles.vue`, `SponsorButtons.vue`, `SponsorButtonCollective.vue`
   - Removed scripts: `sponsors-circles.ts`, `copy-sponsors.ts`
   - Removed assets: `sponsors.svg`, sponsor OG images, temp sponsor artifacts
   - Removed redirects: `/sponsor`, `/support` routes
   - Removed navigation: sponsor link from NavBar
   - Bundle size reduced significantly (removed 1.5MB+ sponsors-list chunk)lex Yang's personal portfolio website - Vue 3 static site powered by Vite + Vite SSG, deployed to Netlify.

- Originally based on Anthony Fu's website template, fully rebranded and customized for Alex Yang.
- Automatic file-based routing (`unplugin-vue-router`) combined with Markdown-to-Vue transform (`unplugin-vue-markdown`).
- Content organized under `pages/`, `pages/posts/`, and TypeScript data modules (`data/`, `photos/`).
- Styling via UnoCSS with Tailwind-like utilities and custom shortcuts; supplemental global CSS lives under `src/styles/`.

## Tooling & Build Pipeline

- Package manager: `pnpm` (workspace-aware) pinned to `pnpm@10.15.1`.
- Main scripts:
  - `pnpm run dev`: Vite dev server on port 3333.
  - `pnpm run build`: orchestrates `npm run static`, `vite-ssg build`, TypeScript helper scripts (`copy-fonts.ts`, `rss.ts`), and copies Netlify redirects into `dist/`.
  - `pnpm run lint`: ESLint via `@antfu/eslint-config`; lint-staged pre-commit hook auto-fixes staged files.
- Build verified today: ✅ `pnpm run build` (2025-09-27) after blog pruning.

## Application Structure

- Entry (`src/main.ts`) registers global plugins (Day.js, FloatingVue, Pinia, RouterScroller, NProgress) and imports UnoCSS + prose styles.
- `src/App.vue` wraps shared UI (navbar, footer) and handles image lightbox interactions.
- Pinia store (`src/store/shiki.ts`) lazily loads Shiki highlighter with dark/light theming derived from `useDark()` helper in `src/logics`.
- `src/components/` contains reusable atoms/molecules (navigation, media viewers, art demos).

## Content & Data

- Markdown pages drive most routes; front matter is processed in `vite.config.ts` to inject meta, auto-generate OG images (Sharp + SVG template) when absent.
- `pages/posts/` now trimmed to eight recent articles (older blogs deleted 2025-09-27). Remaining posts render via `ListPosts` component using automatically exposed front matter.
- Media datasets maintained as TypeScript exports (`data/media.ts`, `photos/data.ts`) consumed by specific views.

## Styling & Assets

- UnoCSS config (`unocss.config.ts`) enables attributify mode, icon preset, custom shortcuts (e.g., `bg-base`, `btn-*`), and local font processing.
- SVG logos (`Logo.vue`, `LogoStroke.vue`) updated to reflect Alex branding, consistent with favicon title metadata.

## Recent Modifications (2025-09-27)

- **Full rebranding**: Changed all textual references from "Anthony Fu" to "Alex Yang" across metadata (`index.html`, RSS metadata, footer, logo titles, README, license, posts index, navigation pages).
- **Content cleanup**: Deleted legacy blog posts and cleaned formatting to satisfy ESLint/Prettier.
- **Sponsor removal**: Completely removed all sponsor-related functionality including:
  - Sponsor pages (`sponsors-list.md`, `collective-sponsor-onetime.md`)
  - Sponsor components (`SponsorsView.vue`, `SponsorsCircles.vue`, `SponsorButtons.vue`, etc.)
  - Sponsor navigation links, redirects, and build scripts
  - Sponsor-related CSS classes and assets
- **Build verification**: Confirmed builds continue to succeed after all changes via `pnpm run build` and `pnpm run lint`.

## Observations & Next Steps

1. Consider updating README with dev/build instructions tailored to the new personal site.
2. Ensure removed blog slugs aren’t referenced externally (e.g., in `ListPosts` dataset snapshots) before deployment.
3. Large `sponsors-list` chunk (>1.5 MB) still triggers bundle warning—future optimization opportunity (lazy loading or chunk splitting).
4. Document photo-management scripts prerequisites (Sharp/libvips) for smoother onboarding.
