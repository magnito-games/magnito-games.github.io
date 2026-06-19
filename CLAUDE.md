# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

The personal portfolio site for **Symsey Cruz**, an enthusiast mobile game developer (iOS & Android). It is a single-page site, served as static files via **GitHub Pages** at https://magnito-games.github.io.

> Note on the URL: the site lives at `magnito-games.github.io` because **Magnito Games** is Symsey's own studio label (the *Squeezee* project). The site itself is a personal portfolio under Symsey's name — the repo/URL just hasn't been renamed.

The site showcases:
- **Shipped flagships** — *Web Master* and *Layer Man 3D*, developed at **Kobgames Studio** (where Symsey was employed) and published by **Azur Games**. Live on both stores with 60M+ combined downloads.
- **In the works** — *Squeezee*, built under Symsey's own studio, **Magnito Games**.

There is **no build step, no framework, and no JavaScript** — the whole site is one self-contained HTML file with inline CSS, base64-embedded screenshots/icons, and a couple of inline SVGs (the avatar and favicon).

## Repository Layout

- **`index.html`** — the entire site: markup + a single inline `<style>` block, with game icons/screenshots embedded as base64 `data:` URIs and the avatar/favicon as inline SVG. This is the only file you normally edit.
- **`google58b5a10811788e18.html`** — Google Search Console site-verification file. Do not rename, edit, or delete it; Search Console looks for it by its exact filename.
- **`README.md`** — short project description.
- **`CLAUDE.md`** — this file.
- **`.gitignore`** — OS/editor cruft, optional web-tooling output, local AI tooling. Note `*.bak` is ignored — local `index.html.*.bak` backups are safety nets and are never committed.

## Build, Test & Development Commands

- **Preview locally**: open `index.html` directly in a browser, or run a static server from the repo root:
  - `python3 -m http.server 8000` → http://localhost:8000
  - or `npx serve .`
- **Quick visual check (headless)**: render a screenshot without opening a browser window:
  - `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless --screenshot=/tmp/out.png --window-size=1200,2700 "file://$PWD/index.html"`
- **Deploy**: there is no build. Pushing to the default branch publishes via GitHub Pages — the live commit *is* the deploy. Confirm Pages is configured to serve from the repo root of that branch.
- **No tests / no CI** for this project.

## Page Structure

The single page is organized into these sections (current content):

- **Header / hero** — pixel-pal SVG avatar + "Symsey Cruz" in a `studio-mark` pill, the "I make games worth playing." headline, a personal tagline, and a `trust` line ("60M+ downloads across games I've shipped").
- **Flagships** (`main.flagship`) — two live game cards, *Web Master* and *Layer Man 3D*, each with icon, `credit` line, description, `stats` (downloads + rating), `tags`, and `cta-row` store buttons (App Store + Google Play, real links with inline SVG glyphs) plus a 2×2 `shots` screenshot grid.
- **In the works** (`section.upcoming`) — a compact `game-card mini` for *Squeezee* (Coming soon), with its Magnito Games `credit` line.
- **Contact / CTA** — "Let's talk" with a `mailto:` link.
- **Footer** — copyright (© Symsey Cruz).

Layout is driven by semantic classes such as `game-card` (+ `mini`), `game-head`, `game-icon`, `game-info`, `flagship`, `upcoming`, `section-label`, `credit`, `stats`/`stat`, `trust`, `cta`/`cta-row` (+ `primary`/`ghost`/`soon`), `tag`/`tags`, `badge`, `tagline`, `studio-mark`/`avatar`, `footer-card`, `copyright`. Reuse the existing classes when adding content rather than introducing parallel styles.

## Working Conventions

- **Keep it dependency-free.** No external scripts, fonts, CSS frameworks, or CDN links — the site loads with zero network requests beyond the HTML itself. Preserve that unless explicitly asked to change it.
- **Images are inline.** Game icons/screenshots are base64 `data:image/...;base64,...`; the avatar and favicon are inline SVG. Keep new assets inline to preserve the single-file model — *or*, if assets get large, propose moving to an `assets/` folder with linked files (a deliberate tradeoff worth flagging, since it changes the single-file model and inflates diffs).
- **Prefer SVG for marks/icons** (avatar, favicon, store glyphs) — it's tiny and scalable, unlike base64 PNGs.
- **CSS lives in the one `<style>` block** in `index.html`. Keep new rules near related ones; follow the existing naming (lowercase, hyphenated class names).
- **Mobile-first.** The site targets phones primarily; verify changes look right at narrow widths (there's a `@media (max-width: 760px)` block and a responsive `viewport` meta).
- **Don't touch the Google verification file.**
- **`index.html` is large** mostly because of embedded base64 — expect big line lengths. Use targeted edits; for inserting/replacing base64, a small script (Python) is more reliable than hand-editing huge lines.

## Adding / Updating a Game

To feature a new game or update an existing one, duplicate a `game-card` block in `index.html` and update: icon (`game-icon`), title, `credit` line, description, `stats` (downloads + rating), `tags`, store links in `cta-row`, and the `shots` screenshots. For an unreleased title, use the compact `game-card mini` pattern with a `Coming soon` `badge` and a `cta primary soon` button instead of store links. Match the existing markup so the shared CSS applies.

Store assets (icons, screenshots, downloads, ratings) can be pulled from the public listings: Apple's iTunes lookup API (`https://itunes.apple.com/lookup?bundleId=...`) returns icon/screenshot URLs, rating, and the App Store link; the Google Play listing page yields the icon, screenshots, publisher, and download count.

## Commit Guidelines

Use imperative commit subjects: `Add Layer Man store links`, `Fix hero spacing on mobile`. Since each push to the default branch is a live deploy, keep commits self-contained and deployable.

## SEO / Metadata

`index.html`'s `<head>` carries the `<title>`, `<meta name="description">`, viewport, and favicon. Update these when site content or branding changes. Verification for Google Search Console is handled by the `google*.html` file at the repo root.
