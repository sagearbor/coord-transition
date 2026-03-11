# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static, offline-capable PWA for peaceful coordination during geopolitical transitions in Iran. Designed for low-bandwidth, conflict-zone environments where internet may be blocked. Supports Persian (default), Azerbaijani, Kurdish, Arabic, and English.

## Architecture

Three files in the repo root — no build step, no dependencies:

- **index.html** — Single-page app. All translations live in a `T` object keyed by language code (`fa`, `az`, `ku`, `ar`, `en`). The `setLang()` function rebuilds the page content using safe DOM methods (createElement/textContent, no innerHTML). Each language entry specifies its own `dir` ('rtl' or 'ltr') which is applied to `<html>` on switch. Language preference is persisted in `localStorage`.
- **sw.js** — Service Worker with cache-first strategy. The `activate` handler purges old caches when `CACHE_NAME` is bumped. **Always increment the version in `CACHE_NAME`** (e.g., `transition-v2` → `transition-v3`) when updating any cached asset.
- **manifest.json** — PWA manifest for standalone display.

## Development

No build tools. A local server is needed for Service Worker testing:

```
python3 -m http.server 8000
```

## Deployment

GitHub Pages: Settings > Pages > Deploy from branch `main` / root.

## Adding a New Language

1. Add an entry to the `T` object with all required keys (copy an existing entry as template). Set `dir` to `'rtl'` or `'ltr'`.
2. Add a `<option value="xx">` to the `#lang-dropdown` select element.
3. Bump `CACHE_NAME` in `sw.js`.

## RTL/LTR Handling

Direction is set per-language on `<html dir="...">`. CSS uses `[dir="rtl"]` selectors to flip the phase card border from left to right. English is the only LTR language currently.
