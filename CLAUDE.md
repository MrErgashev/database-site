# CLAUDE.md

## Project Overview

Static educational website — an interactive methodological guide for teaching database management fundamentals in Uzbek language. Contains 15 practical lessons covering SQL, MySQL, database design, and administration.

- **Author:** M.U. Ergashev
- **Language:** Uzbek (uz)
- **Title:** "Ma'lumotlar bazasi — 15 ta amaliy mashg'ulot"

## Tech Stack

- **HTML5 + CSS3 + Vanilla JavaScript** — no framework, no bundler
- Everything lives in a single `index.html` file (8,985 lines, ~694 KB)
- No `package.json`, no build step, no Node.js

### External CDN Dependencies

- [html-docx-js](https://unpkg.com/html-docx-js/dist/html-docx.js) v1.0 — Word export
- [FileSaver.js](https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js) v2.0.5 — file download
- [Microsoft Fluent UI Emoji](https://github.com/microsoft/fluentui-emoji) — 3D emoji icons (loaded from GitHub raw)
- Google Fonts — Playfair Display & Inter

## Project Structure

```
index.html    — Entire application (HTML + CSS + JS, ~694 KB)
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

The app is a single-page application. Sections are `<div class="section-block">` elements shown/hidden via `showSection(id)`. The section order is defined in the `sections` array (line 8728):

```js
const sections = ['cover','t1','t2','t3','t4','t5','t6','t7','t8','t9','t10','t11','t12','t13','t14','t15'];
```

Plus `backpage` (line 8548, not in the navigation array). `cover` is active by default.

### Lesson Topics

| ID | Topic (Uzbek) |
|---|---|
| `t1` | MB dasturlarini o'rnatish (MySQL, Oracle, MSSQL) |
| `t2` | MB loyihalash, ER diagramma |
| `t3` | SQL — Jadvallar (CREATE, ALTER, DROP) |
| `t4` | AND, OR, NOT amallar |
| `t5` | WHERE, saralash |
| `t6` | GROUP BY, ORDER BY, HAVING |
| `t7` | UNION, INTERSECT, MINUS |
| `t8` | JOIN bilan ishlash |
| `t9` | Standart funksiyalar |
| `t10` | Agregat funksiyalar |
| `t11` | Murakkab so'rovlar |
| `t12` | INDEX yaratish |
| `t13` | VIEW yaratish |
| `t14` | Funksiya, protsedura, trigger |
| `t15` | Oddiy interfeys yaratish |

### Key JavaScript Functions (`<script>` starts at line 8563)

| Function | Line | Purpose |
|---|---|---|
| `showSection(id, e)` | 8565 | Navigate to a lesson/section |
| `toggleSidebar()` / `closeSidebar()` | 8579 / 8593 | Mobile sidebar toggle |
| `filterSidebar(query)` | 8604 | Search/filter lessons in sidebar |
| `getVisited()` / `markVisited(id)` | 8617 / 8622 | Track visited lessons via localStorage |
| `updateProgress()` | 8630 | Update progress bar in sidebar |
| `toggleDarkMode()` / `initDarkMode()` | 8648 / 8655 | Dark/light theme toggle |
| `updateReadingProgress()` | 8664 | Scroll-based reading progress bar |
| `updateBackToTop()` | 8674 | Show/hide back-to-top button |
| `initCopyButtons()` | 8684 | Add copy-to-clipboard on `<pre>` blocks |
| `getCurrentSection()` | 8729 | Return active section ID |
| `navigateSection(dir)` | 8733 | Arrow-key lesson navigation |
| `replaceEmojisWithFluent()` | 8813 | Replace text emojis with 3D Fluent images |
| `exportToWord()` | 8864 | Export all lessons to .docx |
| `initSelfCheck()` | 8938 | Initialize per-lesson self-assessment checkboxes |
| `updateMenuAriaState()` | 8968 | Accessibility: update aria-expanded on menu |

### DOMContentLoaded Init (line 8975)

```js
initDarkMode() → updateProgress() → initCopyButtons() → updateReadingProgress() → replaceEmojisWithFluent() → initSelfCheck()
```

### localStorage Keys

| Key | Value | Purpose |
|---|---|---|
| `mbb_visited` | JSON array of section IDs | Tracks which lessons user visited |
| `mbb_dark_mode` | `"1"` or `"0"` | Dark mode preference |
| `mbb_sc_[lesson]` | JSON array of checkbox IDs | Self-check state per lesson |

### CSS Theming

Uses CSS custom properties on `:root` (light, line 17) and `body.dark-mode` (dark, line 44):
- Gold accent: `--gold: #d4af37`, `--gold-light: #e8cc6a`, `--gold-dark: #b8960e`
- Neumorphic design with `--shadow-neu-dark` / `--shadow-neu-light`
- Sidebar: fixed 280px, dark background (`--sidebar-bg: #1a1a1a`)
- Border radii: `--radius-sm: 12px`, `--radius-md: 20px`, `--radius-lg: 30px`, `--radius-xl: 40px`
- Transitions: `--transition-fast: 0.2s ease`, `--transition-normal: 0.3s ease`

### Responsive Breakpoints

| Breakpoint | Purpose |
|---|---|
| `max-width: 1200px` | Large tablet adjustments |
| `max-width: 900px` | Tablet — sidebar collapses, layout shifts |
| `max-width: 600px` | Mobile — stacked layout |
| `max-width: 380px` | Small mobile — tighter spacing |
| `@media print` | Print/PDF-friendly styles (line 329) |

### Keyboard Shortcuts

- **Arrow Left/Right** — Navigate between lessons
- **Escape** — Close sidebar

### Content Box Classes

- `.theory-box` — Theory/definition blocks
- `.warning-box` — Warning/caution blocks
- `.tip-box` — Tips and best practices
- `.info-box` — Informational notes
- `.card` / `.card-flat` — Content cards
- `.mac-window` — macOS-styled terminal code blocks (with `.mac-titlebar`, `.mac-dots`)
- `.self-check` — Per-lesson self-assessment checkbox containers

## Common Modifications

- **Add a new lesson:** Create `<div class="section-block" id="t16">`, add `'t16'` to the `sections` array (line 8728), and add a sidebar link with `data-section="t16"`
- **Change theme colors:** Edit the CSS custom properties in `:root` (line 17) and `body.dark-mode` (line 44)
- **Modify sidebar width:** Change the `280px` value in `.sidebar` and the corresponding `.main` margin-left
- **Add emoji replacement:** Add entry to `EMOJI_MAP` object (line 8763) with Unicode → Fluent path

## Code Conventions

- All code stays in the single `index.html` — do not split into separate files
- Content is in Uzbek; function names, variables, and comments are in English
- CSS uses custom properties for all themeable values
- Code blocks use `.mac-window` wrapper with `.mac-titlebar` and `.mac-dots` for styling
- Responsive layout via media queries (breakpoints at 1200px, 900px, 600px, 380px)
- Scroll events are throttled via `requestAnimationFrame`
