# CLAUDE.md

## Project Overview

Static educational website — an interactive methodological guide for teaching database management fundamentals in Uzbek language. Contains 15 practical lessons covering SQL, MySQL, database design, and administration.

- **Author:** M.U. Ergashev
- **Language:** Uzbek (uz)
- **Title:** "Ma'lumotlar bazasi — 15 ta amaliy mashg'ulot"

## Tech Stack

- **HTML5 + CSS3 + Vanilla JavaScript** — no framework, no bundler
- Everything lives in a single `index.html` file (~8,985 lines)
- No `package.json`, no build step, no Node.js

### External CDN Dependencies

- [html-docx-js](https://unpkg.com/html-docx-js/dist/html-docx.js) v1.0 — Word export
- [FileSaver.js](https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js) v2.0.5 — file download
- Google Fonts — Playfair Display & Inter

## Project Structure

```
index.html    — Entire application (HTML + CSS + JS, ~695 KB)
cover.png     — Front cover image (~2.5 MB)
back.png      — Back cover image (~1.9 MB)
CLAUDE.md     — This file
.gitignore    — Ignores .claude directory
```

## How to Run

Open `index.html` directly in a browser. No server or build step required.

## No Build / No Tests / No Linter

This project has no build step, no test suite, and no linting or formatting tools.

## Architecture

### Section/Lesson System

The app is a single-page application. Sections are `<div class="section-block">` elements shown/hidden via `showSection(id)`. The section order is defined in the `sections` array:

```
['cover', 't1', 't2', ... 't15']
```

Plus `backpage` (not in the navigation array). `cover` is active by default.

### Key JavaScript Functions (line ~8563)

| Function | Purpose |
|---|---|
| `showSection(id, e)` | Navigate to a lesson/section |
| `toggleSidebar()` / `closeSidebar()` | Mobile sidebar toggle |
| `filterSidebar(query)` | Search/filter lessons in sidebar |
| `getVisited()` / `markVisited(id)` | Track visited lessons |
| `updateProgress()` | Update progress bar in sidebar |
| `toggleDarkMode()` / `initDarkMode()` | Dark/light theme toggle |
| `updateReadingProgress()` | Scroll-based reading progress bar |
| `updateBackToTop()` | Show/hide back-to-top button |
| `initCopyButtons()` | Add copy-to-clipboard on `<pre>` blocks |
| `navigateSection(dir)` | Arrow-key lesson navigation |
| `replaceEmojisWithFluent()` | Replace text emojis with 3D Fluent images |
| `exportToWord()` | Export all lessons to .docx |
| `initSelfCheck()` | Initialize per-lesson self-assessment checkboxes |
| `updateMenuAriaState()` | Accessibility: update aria-expanded on menu |

### localStorage Keys

| Key | Value | Purpose |
|---|---|---|
| `mbb_visited` | JSON array of section IDs | Tracks which lessons user visited |
| `mbb_dark_mode` | `"1"` or `"0"` | Dark mode preference |
| `mbb_sc_[lesson]` | JSON array of checkbox IDs | Self-check state per lesson |

### CSS Theming

Uses CSS custom properties on `:root` (light) and `body.dark-mode` (dark):
- Gold accent: `--gold: #d4af37`, `--gold-light: #e8cc6a`, `--gold-dark: #b8960e`
- Neumorphic design with `--shadow-neu-dark` / `--shadow-neu-light`
- Sidebar: fixed 280px, dark background (`--sidebar-bg: #1a1a1a`)

### Keyboard Shortcuts

- **Arrow Left/Right** — Navigate between lessons
- **Escape** — Close sidebar

### Content Box Classes

- `.theory-box` — Theory/definition blocks
- `.warning-box` — Warning/caution blocks
- `.tip-box` — Tips and best practices
- `.info-box` — Informational notes
- `.card` / `.card-flat` — Content cards
- `.mac-window` — macOS-styled terminal code blocks

## Common Modifications

- **Add a new lesson:** Create `<div class="section-block" id="t16">`, add `'t16'` to the `sections` array, and add a sidebar link with `data-section="t16"`
- **Change theme colors:** Edit the CSS custom properties in `:root` and `body.dark-mode`
- **Modify sidebar width:** Change the `280px` value in `.sidebar` and the corresponding `.main` margin-left

## Code Conventions

- All code stays in the single `index.html` — do not split into separate files
- Content is in Uzbek; function names, variables, and comments are in English
- CSS uses custom properties for all themeable values
- Code blocks use `.mac-window` wrapper with `.mac-titlebar` and `.mac-dots` for styling
- Responsive layout via media queries (breakpoints at 768px for mobile)
