# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static, offline-capable PWA (Progressive Web App) for peaceful geopolitical coordination. Designed for low-bandwidth, conflict-zone environments where internet may be intermittent or blocked. Currently supports English and Farsi.

## Architecture

The entire app is three files in the repo root — no build step, no dependencies:

- **index.html** — Single-page app with all content, styles (CSS custom properties in `<style>`), and logic (vanilla JS in `<script>`). Language switching toggles visibility of `#content-en` / `#content-fa` divs.
- **sw.js** — Service Worker using cache-first strategy (`CACHE_NAME: 'transition-v1'`). Caches `./`, `./index.html`, `./manifest.json` on install. Bump `CACHE_NAME` version when updating cached assets.
- **manifest.json** — PWA manifest for standalone display.

## Development

No build tools. Open `index.html` directly in a browser, or use any static server:

```
python3 -m http.server 8000
```

A local server is needed to test Service Worker functionality (SW requires a server context).

## Deployment

Deployed via GitHub Pages: Settings > Pages > Deploy from branch `main` / root.

## Adding a New Language

1. Add a new `<button>` in `.lang-control` with an `onclick="setLang('xx')"` handler.
2. Add a new `<div id="content-xx" class="hidden" dir="...">` block with translated content.
3. Update `setLang()` in the `<script>` to handle the new language code (currently only toggles between `en` and anything else — will need refactoring for 3+ languages).
