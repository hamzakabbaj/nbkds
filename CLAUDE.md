# Wireframing & Prototyping — NOOBA

Static HTML/CSS/JS wireframes and clickable prototypes for **NOOBA — Raconte-moi le Maroc** (an audio experience for children centered on Moroccan culture). This is a design deliverable, not a production app: plain hand-written HTML, vanilla CSS, and a little inline JS. No build step, no framework, no bundler — open any `.html` file directly in a browser.

This folder is the `wireframing_prototyping` step of the Design Thinking module (see the project root `CLAUDE.md` for the broader workflow).

## Layout

- [index.html](index.html) — landing page. A 3-card hub linking to the three prototypes below.
- [website-vitrine/](website-vitrine/) — **Site Vitrine**: marketing/conversion site (landing, manifeste, catalogue, abonnement, login, account, legal). 13 pages, `01_*`–`13_*`.
- [admin-dashboard/](admin-dashboard/) — **Back-Office Admin**: content & subscriber management (catalog CRUD, upload flows, subscribers, settings, system states). 14 screens.
- [mobile-app/](mobile-app/) — **Application Mobile**: parent-facing iOS/Android app (onboarding, home, player, favorites, downloads, profiles, plus empty/error/edge states). 26 screens.
- [archive/](archive/) — superseded explorations (old `website` and `website-mockups`). Reference only; not linked from `index.html`.
- `data.json` — design data/content used during the wireframing step.

## Conventions

- **Three prototypes, three folders**: `website-vitrine/`, `mobile-app/`, `admin-dashboard/`. Each is self-contained.
- **One `styles.css` per prototype folder** (each of the three has its own). Pages in a folder share that stylesheet; there is no global stylesheet across folders.
- **Pages are numbered** by flow order (`01_`, `02_`, …). Sub-steps of a flow use letter suffixes (e.g. `01a_onboarding_welcome.html`). Empty/error/edge states are separate numbered files.
- **Assets** live in `website-vitrine/assets/` (`logos/`, `mascots/`, `decoration/`, `univers/`, `portraits/`, `piliers/`, `pages/`) and are referenced with relative `assets/...` paths. Keep them as **real files**, not symlinks — a prior broken symlink into `website-mockups` is why images once failed to load.
- **External deps via CDN** only: Google Fonts and Font Awesome 6.5. No local font/icon bundling.
- **Interactivity** is minimal and inline: small `<script>` blocks and CSS-only patterns (e.g. checkbox toggles for nav/FAQ/accordions). No external JS files.
- `website-vitrine/design-tests/` holds in-progress layout variations (hero, nya, page A/B/C splits) — not part of the main flow.

## Working here

- French (`lang="fr"`) UI copy throughout; some Arabic words/script in content.
- When editing, match the existing folder's `styles.css` classes and the page numbering scheme rather than introducing new patterns.
- No tooling to run — verify changes by opening the HTML in a browser (or via the `run` / chrome-devtools skills).
