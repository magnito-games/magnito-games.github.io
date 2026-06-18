# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

The official website for **Magnito Games** — a studio crafting playful mobile games for iOS & Android. It is a single-page marketing/landing site, served as static files via **GitHub Pages** at https://magnito-games.github.io.

There is **no build step, no framework, and no JavaScript** — the whole site is one self-contained HTML file with inline CSS and base64-embedded images.

## Repository Layout

- **`index.html`** — the entire site: markup + a single inline `<style>` block + images embedded as base64 `data:` URIs (favicon and ~5 `<img>` elements). This is the only file you normally edit.
- **`google58b5a10811788e18.html`** — Google Search Console site-verification file. Do not rename, edit, or delete it; Search Console looks for it by its exact filename.
- **`README.md`** — one-line project description.
- **`CLAUDE.md`** — this file.

## Build, Test & Development Commands

- **Preview locally**: open `index.html` directly in a browser, or run a static server from the repo root:
  - `python3 -m http.server 8000` → http://localhost:8000
  - or `npx serve .`
- **Deploy**: there is no build. Pushing to the default branch publishes via GitHub Pages — the live commit *is* the deploy. Confirm Pages is configured to serve from the repo root of that branch.
- **No tests / no CI** for this project.

## Page Structure

The single page is organized into a few sections (current content):

- **Hero** — "We make games" tagline + studio mark.
- **Flagship game card** — currently *Squeezee* (icon, info, screenshots, store badge / "soon").
- **Contact / CTA** — "Let's talk" with a `mailto:` link.
- **Footer** — copyright.

Layout is driven by semantic classes such as `game-card`, `game-head`, `game-icon`, `game-info`, `flagship`, `cta`, `cta-row`, `tag`/`tags`, `tagline`, `studio-mark`, `footer-card`, `copyright`. Reuse the existing classes when adding content rather than introducing parallel styles.

## Working Conventions

- **Keep it dependency-free.** No external scripts, fonts, CSS frameworks, or CDN links — the site loads with zero network requests beyond the HTML itself. Preserve that unless explicitly asked to change it.
- **Images are inline base64.** New images should be embedded as `data:image/...;base64,...` to keep the site single-file, *or* — if assets get large — propose moving to an `assets/` folder with linked files (this is a deliberate tradeoff worth flagging, since it changes the single-file model and inflates diffs).
- **CSS lives in the one `<style>` block** in `index.html`. Keep new rules near related ones; follow the existing naming (lowercase, hyphenated class names).
- **Mobile-first.** The site targets phones primarily; verify changes look right at narrow widths (the page uses a responsive `viewport` meta).
- **Don't touch the Google verification file.**
- **`index.html` is large** mostly because of embedded base64 — expect big line lengths and use targeted edits rather than reformatting the whole file.

## Adding / Updating a Game

To feature a new game or update an existing one, duplicate the existing `game-card` block in `index.html` and update: icon (`game-icon` base64), title, tagline/`tags`, screenshots (`shots`), and the store badge / availability (`badge`, `soon`). Match the existing markup structure so the shared CSS applies.

## Commit Guidelines

Use imperative commit subjects: `Add Squeezee screenshots`, `Fix hero spacing on mobile`. Since each push to the default branch is a live deploy, keep commits self-contained and deployable.

## SEO / Metadata

`index.html`'s `<head>` carries the `<title>`, `<meta name="description">`, viewport, and favicon. Update these when site content or branding changes. Verification for Google Search Console is handled by the `google*.html` file at the repo root.
