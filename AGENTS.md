# AGENTS.md

## What this is
Single-file static portfolio. **No build step, no dependencies, no package manager, no test runner, no linter, no formatter.** Everything lives in `index.html` — HTML, CSS (`<style>`), and JS (`<script>`) are all embedded.

## File map
- `index.html` — the whole site. Sections are delimited by `<!-- ===== N) NOMBRE ===== -->` comments. Edit CSS inside `<style>` and JS inside the IIFE at the bottom of the file.
- No other source files. Anything else you see is generated (e.g. `__pycache__`).

## Dev server
The site is served on **http://localhost:9190**.

Start (Windows, Python already on PATH):
```powershell
Start-Process python -ArgumentList "-m","http.server","9190" -WorkingDirectory "C:\Users\houdr\Desktop\houdpotfolio" -WindowStyle Hidden
```
- Use `python`, **not** `python3` (Windows). If `python` is not on PATH, try `py -m http.server 9190`.
- `python3 -m http.server 9190` (as in the original brief) does **not** work on stock Windows.
- The process runs hidden. Stop with: `Get-Process python | Stop-Process`.
- The server has no hot-reload: edit `index.html`, hard-refresh the browser (Ctrl+Shift+R).

## How to verify a change
There is no automated test. Verification is manual:
1. Save `index.html`.
2. Hard-refresh `http://localhost:9190` in the browser.
3. Eyeball the affected section. DevTools console must be clean.

## Editing the file
The HTML is structured in numbered, commented blocks. When the user asks for a change, locate the right block by its comment header:
- `1) RESET & VARIABLES` … CSS tokens (colors, radii, easing) live in `:root`.
- `4) NAVEGACIÓN` … nav and hamburger.
- `6) HERO` … typewriter target, hero copy, particles canvas.
- `7) ABOUT` … bio paragraphs and the GitHub CTA live in `.about-text`.
- `8) PROYECTOS` … project cards; the first one (`01 / portfolio v1`) is the pre-filled one.
- `9) SKILLS` … `data-value` on `.fill` drives the bar width.
- `10) CONTACTO` … form fields and the mailto / GitHub / LinkedIn links.

JS at the bottom is in numbered blocks `9.1` … `9.14` (cursor, nav scroll, hamburger, smooth scroll, typewriter, particles, scroll reveal, tags cascade, 3D tilt, dev log toggle, skill bars, magnetic buttons, form, initial reveal). Match the comment number to the feature.

## Gotchas
- **Custom cursor**: the body has `cursor: none` and a custom dot+ring. The ring is hidden on touch devices via `@media (hover: none) and (pointer: coarse)`. If a new interactive element is added, add it to the `hoverables` selector in `9.1` to get the hover-grow effect.
- **Magnetic buttons**: require `data-magnetic`. Apply it to any new `<a class="btn">` or `<button class="btn">` you want to feel magnetic.
- **3D tilt cards**: require `data-tilt` on the card. Inner elements use `transform: translateZ(...)` for parallax — preserve that when editing card markup.
- **Dev log expand**: height is animated with `max-height` and set to `scrollHeight` on open. Adding more dev-log lines still works, but the animation jumps if the inner content is very tall.
- **Particle canvas** (`#particles`) is inside `.hero` and resizes from `canvas.parentElement`. Don't wrap it in a transformed parent.
- **Google Fonts** (`JetBrains Mono`, `DM Sans`) are loaded from `fonts.googleapis.com`. The page degrades to system fonts if offline.
- **Form submit** is client-side only — it shows a success message and resets the form. It does **not** send email. The user must wire a backend (e.g. Formspree, Resend, a serverless function) and replace the `submit` handler in `9.13`.
- **No git**: this directory is not a git repo. Don't run `git` commands; the user manages version control themselves.

## Don't
- Don't add `package.json`, a bundler, a framework, or npm dependencies. The brief is explicit: single file, no deps.
- Don't add comments inside the file that say "TODO" or "placeholder" — the user wants a finished file.
- Don't split the file into multiple files. The whole point is one self-contained `index.html`.
