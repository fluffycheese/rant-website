# RANT Website — Agent Guidelines

Welcome to the RANT website repository. This file provides context and constraints for any AI agent or coding assistant working on this repository.

## Purpose

This repository contains the marketing and documentation site for [RANT (Rack And Networking Tool)](https://github.com/fluffycheese/rant). It is a static Astro site deployed to Cloudflare Pages.

The site explains what RANT is, how it compares to existing tools, deployment options, and hosts the project blog. It is not the application itself — the app lives in the [main RANT repo](https://github.com/fluffycheese/rant).

## Working assumptions

- This is a static marketing site, not a web application. Keep it simple.
- Prefer vanilla CSS over utility frameworks. No Tailwind, no CSS-in-JS.
- Keep the page count small and the build fast.
- Blog posts are Markdown files in `src/content/blog/` — no CMS, no external data sources.
- All external links to the RANT project use the canonical GitHub URL: `https://github.com/fluffycheese/rant`.

## Design guidance

- The site uses a dark Slate theme matching the RANT application. Colour tokens are defined as CSS custom properties in `src/styles/global.css`. Do not introduce colours outside this palette.
- Read the RANT application's [UI/UX Design Guide](https://github.com/fluffycheese/rant/blob/main/docs/UI-UX-GUIDE.md) for the canonical colour values and design philosophy. The website's palette is derived from it.
- The hero image (`public/hero01.jpg`) is a miniature-figures-on-a-patch-panel photograph. Use it as a background image, not a content image.
- Brand assets live in `public/`: logo variants (`primary-horizontal.png`, `primary-stacked.png`, `primary-horizontal-bg.png`), favicons, and app manifest icons. Use existing assets — do not generate new logo variants.

## Architecture

- **Framework:** Astro 7 (static output, no SSR).
- **Styling:** Vanilla CSS in `src/styles/global.css`. All layout uses CSS Grid and Flexbox.
- **Components:** Astro components in `src/components/`. Extract reusable markup into components rather than inlining in layouts or pages.
- **Layouts:** `src/layouts/Layout.astro` is the base layout wrapping all pages.
- **Content:** Blog posts use Astro's content collections with a glob loader. Schema: `title` (string), `pubDate` (date), `description` (string), `author` (string, defaults to 'Admin').
- **Deployment:** Cloudflare Pages. Three-stage branch workflow: `dev` → `staging` → `main`.

## Implementation guidance

- Follow the existing vanilla CSS structure. Do not introduce CSS modules, Sass, or utility class libraries.
- Keep changes small and easy to reason about.
- When adding new sections to the landing page, follow the existing pattern: `<section class="section section--surface|section--deep">` with a `.container` wrapper and `.section-label` eyebrow text.
- The mobile navigation uses a hamburger toggle (`#menuToggle`) with JavaScript in `Layout.astro`. The menu state is managed via the `.is-open` class on `#navLinks`.
- Responsive breakpoint is `768px`. Test layout changes at both desktop and mobile widths.

## Content writing rules

- **No em dashes (—).** Use commas, colons, parentheses, or hyphens instead. Em dashes are a strong indicator of AI-generated text.
- Write in a natural, conversational tone. First person is fine for blog posts.
- Keep paragraphs short and scannable.

## Domain knowledge

This site documents the RANT application. Before writing or editing content about RANT's features, architecture, or terminology, read `CONTEXT.md` in the [main RANT repo](https://github.com/fluffycheese/rant/blob/main/CONTEXT.md). Use the canonical glossary terms (Site, Comms Rack, Device, Port, Cable Link) consistently.

## References

- `README.md` for local development and deployment workflow
- `src/styles/global.css` for the design system and colour tokens
- [RANT CONTEXT.md](https://github.com/fluffycheese/rant/blob/main/CONTEXT.md) for the domain glossary
- [RANT UI/UX Guide](https://github.com/fluffycheese/rant/blob/main/docs/UI-UX-GUIDE.md) for the canonical colour palette
