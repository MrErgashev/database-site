# CLAUDE.md

## Project Overview

Static educational website — an interactive methodological guide for teaching database management fundamentals in Uzbek language. Contains 15 practical lessons covering SQL, MySQL, database design, and administration. Author: M.U. Ergashev.

## Tech Stack

- **HTML5 + CSS3 + Vanilla JavaScript** (no framework)
- Single-page application in one `index.html` file (~8,985 lines)
- No build system, no package.json, no bundler
- External CDN dependencies: html-docx-js, FileSaver.js, Google Fonts (Playfair Display, Inter)

## Project Structure

```
index.html    — Entire application (HTML, CSS, JS)
cover.png     — Front cover image
back.png      — Back cover image
.gitignore    — Git ignore rules
```

## How to Run

Open `index.html` directly in a browser, or serve via any static HTTP server.

## No Build / No Tests / No Linter

This project has no build step, no test suite, and no linting or formatting tools configured.

## Key Architecture

- **Navigation:** `showSection(id)` switches between 15 lessons + cover + back page
- **Dark mode:** Toggled via `toggleDarkMode()`, persisted in `localStorage` (`mbb_dark_mode`)
- **Progress tracking:** Visited lessons stored in `localStorage` (`mbb_visited`)
- **Self-check:** Per-lesson checkbox state stored in `localStorage` (`mbb_sc_[lesson_id]`)
- **Export:** Word export via html-docx-js; PDF via browser print with custom print CSS
- **Design:** Neumorphic style with gold accent (#d4af37), full dark/light theme support

## Lesson IDs

`t1` through `t15`, plus `cover` and `backpage`. Used as section IDs and in `showSection()` calls.

## Code Style

- All code lives in a single file — keep it that way
- Uzbek-language content throughout; comments and variable names in English
- CSS custom properties for theming; media queries for responsive layout
- Code blocks use macOS-terminal styling with copy-to-clipboard buttons
