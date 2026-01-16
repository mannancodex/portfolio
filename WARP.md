# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project overview

This repository is a static personal portfolio site for Mannan Lodaya, implemented with plain HTML, CSS, and vanilla JavaScript. There are no build tools, package managers, or test frameworks configured; the site can be served directly as static files.

Directory layout (top level only):
- `index.html` – main document and content for all sections.
- `styles/main.css` – global design system and layout.
- `scripts/main.js` – small interactive behaviors and animations.

## Commands and local development

There is no build step; you can open `index.html` directly in a browser or run a simple static file server from the repo root.

From the repository root (`C:\Users\manna\mannan-lodaya-portfolio`):

### Preview in a browser without a server
- Open `index.html` directly in your browser (e.g., double-click in a file explorer or use `code`/IDE to reveal in OS and open).

### Run a simple local HTTP server
Use whichever runtime is available in the environment:

- Python (if installed):
  - `python -m http.server 8000`
  - Then visit `http://localhost:8000/` in your browser.

- Node.js (if installed):
  - `npx serve .`
  - Then visit the URL shown in the terminal (commonly `http://localhost:3000` or `http://localhost:5000`).

There are currently **no** configured tasks for building, linting, or running tests, and no test files in the repository. If you add tooling (e.g., ESLint, a bundler, or a test runner), update this file with the relevant commands.

## High-level architecture

### HTML structure (`index.html`)

The page is a single HTML document containing all sections:
- **Header / navigation** – Sticky header with brand, in-page anchor navigation, and a "View Work" call-to-action.
- **Hero** – Introductory copy and a visual "orbit" graphic.
- **About** – Two-column layout describing strengths, approach, and stats.
- **Skills** – Grid of skill cards for AI/ML, ML engineering, front-end, and product/UX.
- **Projects** – Grid of three "project cards" that expand to show additional details.
- **Experience** – Vertical timeline of experience entries.
- **Contact** – Contact form using a `mailto:` action and additional contact links.
- **Footer** – Copyright line that is populated dynamically with the current year.

Key attributes and hooks:
- Sections are identified via `id` attributes (e.g., `#hero`, `#about`, `#projects`) to support smooth in-page navigation and linking.
- Elements with `data-animate` attributes are animated into view via the IntersectionObserver logic in `scripts/main.js`.
- Project cards use a `data-project-card` attribute, `.project-card__inner` button, and `.project-card__details` content area for the open/close behavior.

### Styling system (`styles/main.css`)

The CSS file defines a small design system and layout primitives:
- CSS custom properties (`:root`) for colors, typography, radii, shadows, and layout constants (e.g., `--content-width`, `--accent`, `--text-soft`).
- Global page styles for background gradients, typography, and antialiasing.
- Components and sections:
  - Header (`.site-header`, `.nav`, `.brand`) – sticky, blurred background, subtle border.
  - Buttons (`.button`, modifier classes `--primary`, `--secondary`, `--ghost`).
  - Sections (`.section`, `.section--bordered`, `.section__header`, `.section__body`) – shared spacing and typography.
  - Hero layout and orbit graphic (`.hero`, `.hero-orbit`, rings, particles) with keyframe animations (`orbit`, `float`).
  - Cards (`.stat-card`, `.skill-card`, `.project-card`) – shared elevation, hover states, and inner content structure.
  - Timeline (`.timeline`, `.timeline-item`) – left border with glowing markers.
  - Contact area (`.contact`, `.contact-form`, `.contact-meta`) – form styling, focus states, and supporting text.
  - Footer (`.site-footer`, `.site-footer__inner`).
- Animation and motion:
  - `[data-animate]` elements fade/translate into place when `.is-visible` is added.
  - Media query for `prefers-reduced-motion: reduce` disables most animations and transitions for accessibility.
- Responsiveness:
  - Breakpoints at `max-width: 960px` and `max-width: 720px` adjust grids, collapse multi-column layouts to single-column, hide the nav on smaller screens, and reorder hero visual/content.

When modifying layout or visuals, prefer reusing and extending existing CSS variables and component patterns instead of introducing entirely new ad-hoc styles.

### JavaScript behavior (`scripts/main.js`)

The JavaScript file is a small, self-contained script that enhances the static markup:

1. **Dynamic year in footer**
   - On `DOMContentLoaded`, finds the element with `id="year"` and sets its text content to the current year.

2. **Smooth in-page navigation**
   - Attaches click handlers to anchors whose `href` starts with `#`.
   - Prevents default navigation for non-`#` IDs that exist in the DOM and calls `element.scrollIntoView({ behavior: "smooth", block: "start" })`.

3. **Scroll-based reveal animations**
   - Collects all elements with `[data-animate]`.
   - If `IntersectionObserver` is available, observes each element and adds the `is-visible` class when it enters the viewport (then unobserves it).
   - If `IntersectionObserver` is not available (older browsers), falls back to adding `is-visible` immediately to all `[data-animate]` elements.

4. **Project card expand/collapse**
   - For each `[data-project-card]` container:
     - Grabs the `.project-card__inner` button and `.project-card__details` content.
     - Toggles `aria-expanded` on the button, the `hidden` state on the details, and the `.project-card--open` class on the card when clicked.

The script is written in plain ES5/ES6-compatible JavaScript and is loaded with `defer` in `index.html`, so there is no module system or bundling in place.

## Future changes

If you introduce a build system, component framework, or testing setup (e.g., React + Vite, Jest, ESLint), update this file to document:
- How to install dependencies.
- How to run the dev server, build, lint, and run tests (including single-test or watch commands).
- Any new architectural conventions (e.g., component directories, state management patterns).