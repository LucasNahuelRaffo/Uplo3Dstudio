# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page homepage for **Uplo3D Studio** (impresión 3D — sets de puntas, tapas de rueda, piezas a medida), exported from **Claude Design** (Anthropic's design tool) in its `x-dc` runtime format. There is no build system, no `package.json`, and no test suite — this is a static export meant to be dropped onto a host as-is.

## Files

- `Uplo3D Studio.dc.html` — the entire page. Root markup lives inside a custom `<x-dc>` element; a companion `<script data-dc-script>` holds the page's JS (handlers referenced from markup via `onClick="{{ handlerName }}"` bindings, e.g. `goHero`, `heroNext`, `sendForm`, `setEn`/`setEs`).
- `support.js` — the `x-dc` runtime (React-based renderer that parses the `<x-dc>` template + script and mounts it). **Do not hand-edit this file.** Its header states it is `GENERATED from dc-runtime/src/*.ts — do not edit. Rebuild with 'cd dc-runtime && bun run build'` — the `dc-runtime` TS source isn't part of this repo, so this file is a build artifact from elsewhere; treat it as vendored/opaque.
- `assets/logos/`, `assets/brands/` — brand logos used in the "Marcas que confían" marquee section.
- `uploads/` — page images and 3D models (`.glb`) rendered via `<model-viewer>` (Google's web component, loaded from unpkg in the HTML `<head>`).
- `.thumbnail` — preview thumbnail for the design, not page content.

## Page structure (in `Uplo3D Studio.dc.html`)

Sections are marked with `data-screen-label` and appear in this order: Hero → Marcas (brand marquee) → Teaser Nosotros (`#about`) → Preview Servicios (`#services`) → Preview Proyectos (`#projects`) → Galería → Contacto + Footer (`#contact`).

- Styling is inline (`style="..."` on every element) — there is no external stylesheet besides the Google Fonts (Archivo, Inter, Space Mono) linked in `<helmet>`.
- Scroll-reveal / entrance animations are driven by `data-reveal` attributes and CSS transitions defined in the inline `<style>` block inside `<helmet>` (e.g. `@keyframes uploMarquee`, `uploFloat`, `uploSpin`), respected by a `prefers-reduced-motion` override.
- The header nav uses `onClick="{{ handlerName }}"` bindings resolved by the script inside `<script data-dc-script>` (not by `support.js` itself — `support.js` only provides the parsing/mounting runtime).
- 3D models (`mate.glb`, `set-puntas.glb`, `set-puntas-animado.glb`, `tapa-rueda.glb`) are shown via `<model-viewer>` tags pointing at files in `uploads/`.

## Working in this repo

- To preview the page, just open `Uplo3D Studio.dc.html` in a browser (or serve the folder with any static file server) — `support.js` boots the runtime and renders the `<x-dc>` template client-side.
- Edits to copy, layout, or styling happen directly in the inline HTML/CSS inside `Uplo3D Studio.dc.html`; there's no source-of-truth elsewhere to sync from.
- Image/model assets referenced by the page live under `uploads/` and `assets/` with relative paths — keep new assets there and reference them the same way.
