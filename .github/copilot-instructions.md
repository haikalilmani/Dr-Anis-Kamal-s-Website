Purpose
This repo is a small static website (HTML/CSS/JS) for Dr. Syakira's O&G practice. The goal of this file is to give AI coding agents immediate, actionable context so they can make safe, focused edits.

Big picture
- Site type: Static, client-side. No backend code or build pipeline detected (no package.json, no server-side code).
- Primary entry points: `index.html` and `homepage.html`. Pages are plain HTML referencing `styles.css`, `styles2.css` and (optionally) `script.js`.
- Media and assets: `Media/` contains `images/` and `icons/` used by the pages.

Key files & patterns (quick references)
- `homepage.html`: The most complete page. Uses inline scripts (AOS init, `toggleMenu()`, `validateForm()`); links `styles2.css`. Form uses `onsubmit="return validateForm()"` and currently prevents real submission (returns false). Example: form validation lives inline at bottom of `homepage.html`.
- `index.html`: Simpler landing page. Contains several malformed tags (e.g. `<charset="utf-8">`, `<viewport ...>`), backslash paths like `Media\\icons\\...`, and an incorrect `a img` usage. Expect small HTML fixes here.
- `script.js`: Present but empty — the project currently uses inline <script> blocks in HTML instead. Prefer moving reusable JS (menu toggle, validation) into `script.js` when introducing changes.
- `styles.css`, `styles2.css`, `alignment reference.css`: Styling is split across files; `homepage.html` links `styles2.css` while `index.html` links `styles.css`. Search for the class names (`.hero`, `.about-section`, `.nav-links`) to find where styles live.

Concrete developer workflows
- Local preview: open `index.html` or `homepage.html` in the browser. On Windows PowerShell you can run:
  - `Invoke-Item .\\index.html`  # opens default browser at the file
  - or start a simple HTTP server (preferred for relative paths): `python -m http.server 8000` then visit `http://localhost:8000`.
- VS Code: use the Live Server extension to serve the workspace root for quicker feedback.
- Git topic branches: repo branch `new-homepage` exists — use short feature branches and open PRs. Example PowerShell sequence:
  - `git checkout -b feat/update-homepage; git add .; git commit -m "Improve homepage HTML semantics"; git push -u origin feat/update-homepage`

Project-specific conventions and gotchas
- Inline JS is used in `homepage.html` (AOS init, `toggleMenu`, `validateForm`). When refactoring, keep behavior identical and run manual browser checks — these functions are referenced directly by HTML attributes.
- Inconsistent file linking: some pages use `styles.css` and others `styles2.css`. Confirm which CSS file to update for a given page before editing.
- Path separators: `index.html` contains Windows-style backslashes (`Media\\icons\\...`). Prefer forward slashes for web paths (`Media/icons/...`) when editing HTML.
- Malformed HTML present in `index.html` (improper tags and attributes). Small fixes are okay, but avoid sweeping reformatting; make targeted corrections with tests in the browser.
- Forms are placeholder/demo only: `homepage.html` form uses `action="#"` and `validateForm()` returns `false`. Do not wire to a backend unless instructed; document any changes that alter submission behavior.

Integration points & external deps
- External CDNs used: Google Fonts, Font Awesome (cdnjs), AOS (unpkg). These are loaded directly from HTML — offline tests may require network access.
- No package manager detected; adding Node or Python tooling is allowed but must be documented in the PR.

What an AI agent should do first (checklist)
- 1) Run a quick repo search for the page(s) impacted. 2) Open the page in a browser (or start `python -m http.server`) and reproduce the issue visually. 3) Make minimal, well-tested edits (fix one HTML error or move one function to `script.js`). 4) Run the browser check again.

Small examples to copy/paste
- Open homepage: `Invoke-Item .\\homepage.html` (PowerShell)
- Serve site: `python -m http.server 8000` then visit `http://localhost:8000/homepage.html`
- Move inline JS to `script.js` safely:
  - Copy `function toggleMenu() { ... }` from bottom of `homepage.html` into `script.js`.
  - Replace the inline `<script>` block with `<script src="script.js"></script>` placed just before `</body>`.
  - Verify menu still toggles and that no global name collisions occur.

Safety & PR guidance
- Keep changes small and focused. Include screenshots or `curl`/browser-check notes in the PR description when editing visible content.
- Mention any improvements to accessibility (alt text, semantic tags) and any HTML normalization fixes you make.
- Do not remove the demo `validateForm()` behavior unless a backend is prepared. If you enable real submission, include notes about CSRF, server-side validation, and where the backend should live (none exists now).

If anything here is unclear or you'd like additional examples (automated checks, preferred branch naming, or CI steps), tell me what to expand.
