# CLAUDE.md — AI Assistant Guide for Mealtime

## Project Overview

**Mealtime** is a minimal, single-page static website that displays the current time and tells you what "mealtime" it is (breakfast, lunch, dinner, etc.) based on predefined time boundaries. Hosted on GitHub Pages at `mealtime.fyi`.

## Tech Stack

- **HTML5** / **CSS3** / **Vanilla JavaScript**
- **Moment.js 2.24.0** — embedded directly in `time.js` (not installed via npm)
- **GitHub Pages** — static hosting with custom domain via `CNAME`
- No build tools, no package manager, no bundler, no framework

## Repository Structure

```
mealtime/
├── index.html          # Main HTML entry point
├── time.js             # Application logic + bundled moment.js (~152KB)
├── darkmode.css        # Dark mode styles (via prefers-color-scheme media query)
├── fonts.css           # Font definitions (Staatliches)
├── staatliches.woff2   # Custom font file
├── favicon.ico         # Site icon
├── CNAME               # GitHub Pages custom domain (mealtime.fyi)
└── README.md           # Minimal readme
```

## Key Application Logic

All application code lives at the end of `time.js` (after the bundled moment.js library).

### Meal Time Boundaries

Defined as an array of objects with `name`, `upper` (start time), and `lower` (end time):

| Meal             | Start    | End      |
|------------------|----------|----------|
| breakfast        | 03:00:00 | 10:29:59 |
| brunch           | 10:30:00 | 11:59:59 |
| lunch            | 12:00:00 | 12:59:59 |
| late lunch       | 13:00:00 | 14:29:59 |
| afternoon snack  | 14:30:00 | 15:59:59 |
| early dinner     | 16:00:00 | 17:29:59 |
| dinner           | 17:30:00 | 19:59:59 |
| late dinner      | 20:00:00 | 21:29:59 |
| midnight snack   | 21:30:00 | 02:59:59 |

### Core Functions

- `currentTime()` — Updates the clock display every 500ms
- `timeBoundary()` — Checks current time against boundaries using `moment().isBetween()` and updates the meal label every 1000ms

Both use `document.getElementById()` for direct DOM manipulation.

## Development Workflow

### Running Locally

No build step required. Open `index.html` in a browser, or serve with any static file server:

```sh
python3 -m http.server 8000
# or
npx serve .
```

### Deployment

Push to the default branch — GitHub Pages serves the files automatically.

### No Build / Test / Lint Commands

There are no `npm` scripts, test suites, linters, or CI pipelines configured for this project.

## Known Issues

- `index.html` references `<script src="darkmode.js"></script>` but no `darkmode.js` file exists in the repo. This is dead code — dark mode is handled entirely by CSS media queries in `darkmode.css`.

## Conventions

- All JavaScript is vanilla — no modules, no imports, no transpilation
- Moment.js is bundled inline in `time.js` rather than loaded from a CDN or installed via npm
- Styles are split into `fonts.css` (typography) and `darkmode.css` (dark theme)
- Dark mode is automatic via `prefers-color-scheme` CSS media query — no user toggle
