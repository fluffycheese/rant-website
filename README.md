# RANT Website

Marketing and documentation site for [RANT (Rack And Networking Tool)](https://github.com/fluffycheese/rant), built with Astro and deployed to Cloudflare Pages.

The site explains what RANT is, how it compares to existing tools, deployment options, and hosts the project blog.

## Branch workflow

Use the same three-stage workflow as the related Astro/static projects.

| Branch | Purpose | Cloudflare deployment |
| --- | --- | --- |
| `dev` | Local development branch. Work on content, styles, and layout. | No automated production deployment. Preview locally. |
| `staging` | Integration branch after local checks pass. | Cloudflare Pages preview deployment for review. |
| `main` | Production-ready branch. Merge from `staging` after approval. | Cloudflare Pages production deployment. |

Recommended flow:

1. Work on `dev`.
2. Test locally.
3. Merge `dev` → `staging`.
4. Review the Cloudflare Pages staging preview.
5. Merge `staging` → `main`.
6. Cloudflare deploys production.

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

Cloudflare's local runtime uses `workerd`, which is a dynamically linked binary and may fail on NixOS. For normal content/layout work, use `npm run dev` without the Cloudflare adapter. Do not use Wrangler local preview on NixOS unless the system is configured for generic dynamic binaries (e.g. `programs.nix-ld`).

## Production build

```bash
npm run build
```

Output goes to `dist/`.

## Cloudflare Pages setup

Create a Cloudflare Pages project connected to the Git repository.

| Setting | Value |
| --- | --- |
| Framework preset | Astro |
| Build command | `npm run build` |
| Output directory | `dist` |
| Production branch | `main` |
| Preview branch | `staging` |

## Content

Blog posts live in `src/content/blog/` as Markdown files with frontmatter:

```yaml
---
title: "Post Title"
pubDate: 2026-09-01
description: "Short description for listing pages."
author: "Author Name"
---
```

The three most recent posts appear on the homepage. All posts are listed at `/blog`.

## Useful commands

| Command | Purpose |
| --- | --- |
| `npm run dev` | Local Astro dev server. |
| `npm run build` | Production build to `dist/`. |
| `npm run preview` | Preview the production build locally. |
