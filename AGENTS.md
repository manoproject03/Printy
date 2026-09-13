# Printy — Base44 dev environment

## What this is
A static PWA: a single `index.html` (Arabic RTL store-management UI) with inline
CSS/JS, plus `manifest.json`, `sw.js` (service worker), and PNG icons. No backend,
no build step, no database, no external credentials.

External resources loaded client-side (no secrets): Google Fonts and Chart.js CDN.

## How it runs
Served by Vite (dev server) for live reload. See `docker-compose.base44.yml`:
- `node:22-slim` base, repo bind-mounted at `/app`
- `npm install` then `vite --host 0.0.0.0 --port 3000` on startup
- `node_modules` kept in a named volume so installs aren't repeated
- Host port 3000 is the preview entry point

## Verify it works
`docker compose -f docker-compose.base44.yml up -d --build`, then
`curl -s localhost:3000 | head` should return the `index.html` doctype + `<title>`.

## Editing
Edits to `index.html` (or any static asset) hot-reload in the preview. No rebuild
needed. The service worker (`sw.js`) may serve a cached copy on reload; if a change
isn't visible, hard-reload or bump `CACHE_NAME` in `sw.js`.
