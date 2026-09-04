# RANT Website

Marketing and documentation site for [RANT (Rack And Networking Tool)](https://github.com/fluffycheese/rant), built with Astro and deployed to Cloudflare Pages.

The site explains what RANT is, how it compares to existing tools, deployment options, and hosts the project blog. A live version is available at [rant.fluffycheese.co.uk](https://rant.fluffycheese.co.uk).

## What this repo contains

- Single-page landing site with feature overview, comparison tables, and deployment guides
- Blog powered by Astro content collections (Markdown files in `src/content/blog/`)
- SEO setup: canonical URLs, sitemap, Open Graph meta tags
- Dark-themed design matching the RANT application's Slate/Sky palette

## Branch workflow

We use a three-stage branching workflow to ensure safe development and production deployments.

| Branch | Purpose | Cloudflare deployment |
| --- | --- | --- |
| `dev` | Local development branch. Work on content, styles, and layout. | No automated production deployment. Preview locally. |
| `staging` | Integration branch after local checks pass. | Cloudflare Pages preview deployment for review. |
| `main` | Production-ready branch. Merge from `staging` after approval. | Cloudflare Pages production deployment. |

Recommended flow:

1. Work on `dev`.
2. Test locally with `npm run dev`.
3. Merge `dev` -> `staging` and push.
4. Review the Cloudflare Pages staging preview.
5. Merge `staging` -> `main` and push.
6. Cloudflare deploys production.

> **Note:** All merges should be done via pull requests to maintain a clear history and allow review.

## Local development

Install dependencies:

```bash
npm install
```

Run Astro locally:

```bash
npm run dev
```

The dev server starts at `http://localhost:4321`.

### NixOS note

Cloudflare's local runtime uses `workerd`, which is a dynamically linked binary and may fail on NixOS with:

```text
NixOS cannot run dynamically linked executables intended for generic linux environments out of the box
```

For normal content/layout work, use `npm run dev` without the Cloudflare adapter. Do not use Wrangler local preview on NixOS unless the system is configured for generic dynamic binaries (e.g. `programs.nix-ld`).

## Production build

```bash
npm run build
```

Output goes to `dist/`.

## Cloudflare Pages setup

1. **Create a new Pages project** on Cloudflare and connect the GitHub repository (`https://github.com/fluffycheese/rant-website`).
2. Configure the following settings:

| Setting | Value |
| --- | --- |
| Framework preset | Astro |
| Build command | `npm run build` |
| Output directory | `dist` |
| Production branch | `main` |
| Preview branch | `staging` |

3. **Custom domain**: `rant.fluffycheese.co.uk` -> set via the Cloudflare Pages dashboard for the `main` branch.
4. **Always HTTPS**: Enable for secure connections.

## Blog content

Blog posts live in `src/content/blog/` as Markdown files with frontmatter:

```yaml
---
title: "Post Title"
pubDate: 2026-09-01
description: "Short description for listing pages."
author: "fluffycheese"
---
```

The three most recent posts appear on the homepage. All posts are listed at `/blog`.

### Writing rules

- No em dashes. Use commas, colons, parentheses, or hyphens instead.
- Write in a natural, conversational tone. First person is fine.
- Keep paragraphs short and scannable.

## Project structure

```
src/
  components/     # Reusable Astro components (Header, etc.)
  content/
    blog/         # Markdown blog posts
  layouts/        # Base page layout (Layout.astro)
  pages/
    blog/         # Blog listing and individual post routes
    index.astro   # Landing page
  styles/
    global.css    # Design system: colour tokens, layout, animations
public/           # Static assets: logos, favicons, hero image
```

## Design system

All styling is vanilla CSS in `src/styles/global.css`. No Tailwind, no CSS modules, no CSS-in-JS.

The colour palette is derived from the RANT application's [UI/UX Design Guide](https://github.com/fluffycheese/rant/blob/main/docs/UI-UX-GUIDE.md):

| Token | Value | Usage |
| --- | --- | --- |
| `--bg-deep` | `#0F172A` | Page background, deep sections |
| `--bg-surface` | `#1E293B` | Alternating section backgrounds |
| `--text-primary` | `#F1F5F9` | Headings, primary text |
| `--text-secondary` | `#CBD5E1` | Body text |
| `--blue` | `#3BB2F6` | Links, accents, CTAs |
| `--green` | `#10B981` | Success indicators |
| `--purple` | `#A78BFA` | Secondary accents |

## Useful commands

| Command | Purpose |
| --- | --- |
| `npm run dev` | Local Astro dev server at `localhost:4321`. |
| `npm run build` | Production build to `dist/`. |
| `npm run preview` | Preview the production build locally. |

## Related repositories

| Repository | Purpose |
| --- | --- |
| [fluffycheese/rant](https://github.com/fluffycheese/rant) | The RANT application (backend + frontend). |
| [fluffycheese/rant-website](https://github.com/fluffycheese/rant-website) | This repository, the marketing site. |
