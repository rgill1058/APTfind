# CLAUDE.md — AptFind

## Project Overview

**AptFind** is a static HTML landing page for an apartment directory marketplace. It serves two audiences:

- **Renters** — browse and search apartment listings by location, price, and size
- **Property Managers** — list communities via three paid tiers ($49 / $149 / $349 per month)

The entire codebase lives in a single file: `site`. It is a self-contained HTML document with embedded CSS and JavaScript — no build tools, no dependencies, no backend.

---

## Repository Structure

```
APTfind/
├── CLAUDE.md   # This file
└── site        # The complete application (HTML + CSS + JS, ~1,303 lines)
```

There is intentionally **no** `package.json`, `requirements.txt`, Dockerfile, `.env`, or any other configuration file. The project is dependency-free by design.

---

## The `site` File

### Structure

The file is organized top-to-bottom as:

| Section | Description |
|---|---|
| `<head>` | Meta tags, Google Fonts (`Playfair Display`, `DM Sans`), all inline `<style>` |
| `<nav>` | Fixed navigation bar with logo, links, and CTA button |
| `.hero` | Dark hero with search input and headline |
| `.filter-bar` | Sticky tab bar for filtering (All, Studios, 1BR, 2BR, 3BR+, Pet Friendly, Luxury) |
| `.featured-communities` | Highlighted 3-card grid |
| `.all-listings` | Main listing grid (~12 example cards) |
| `.city-section` | Browse-by-city grid (12 cities) |
| `.pricing-section` | Three pricing tier cards |
| `.cta-section` | Bottom call-to-action banner |
| `<footer>` | Four-column footer with links |
| `#modal` | Lead capture modal overlay |
| `<script>` | All JavaScript (~45 lines) |

### CSS Architecture

CSS is written inline in a single `<style>` block. Key patterns:

- **CSS custom properties** defined on `:root`:
  ```css
  --sand: #f5f0e8
  --cream: #faf8f4
  --charcoal: #1c1c1e
  --warm-gray: #6b6460
  --gold: #c9a84c
  --gold-light: #e8d5a3
  --rust: #b85c38
  --white: #ffffff
  --border: #e2ddd6
  --card-shadow: 0 2px 20px rgba(28,28,30,0.08)
  ```
- **Responsive design** via a single breakpoint at `max-width: 900px`
- **Scroll animation** using a `@keyframes fadeUp` animation applied by the Intersection Observer
- **Typography**: `Playfair Display` (serif) for headings/logo, `DM Sans` (sans-serif) for body text

### JavaScript

All JS is in a single `<script>` block at the bottom of the file. Functions:

| Function | Purpose |
|---|---|
| `setTab(el)` | Activates a filter tab by toggling `.active` class |
| `showModal()` | Opens the lead capture modal; locks scroll |
| `closeModal()` | Closes the modal; restores scroll |
| `handleOverlayClick(e)` | Closes modal when clicking the overlay background |
| `submitLead()` | Replaces modal content with a success message |
| Intersection Observer | Applies `fadeUp` animation to `.listing-card`, `.city-card`, `.pricing-card` on scroll |

> **Note**: `submitLead()` currently shows a success state but does **not** send data anywhere. Backend integration is required for real lead capture.

---

## Development Workflow

### Viewing the Site

Open the `site` file directly in a browser — no server required:

```bash
# macOS
open site

# Linux
xdg-open site

# Or serve locally (optional, for testing link behavior)
python3 -m http.server 8080
# then visit http://localhost:8080/site
```

### Editing

All changes go into the single `site` file. Edit CSS in the `<style>` block, HTML in the body, and JS in the `<script>` block.

There are no linters, formatters, or pre-commit hooks configured.

### Testing

There is no automated test suite. Manual browser testing is the current process:

1. Open `site` in a browser
2. Verify responsive layout at 900px breakpoint (use browser DevTools)
3. Test modal open/close behavior
4. Test filter tab switching
5. Test scroll animations
6. Test the lead capture form flow

---

## Git Conventions

- **Branch**: `claude/claude-md-mlvftst6qoichyhx-BsEiS`
- **Remote**: `http://local_proxy@127.0.0.1:49080/git/rgill1058/APTfind`
- Single commit history; no branch strategy established yet

Commit messages should be clear and descriptive (e.g., `Add mobile nav hamburger menu`, not `fix stuff`).

---

## Key Conventions to Follow

### When editing the `site` file

1. **Preserve variable usage** — always use the CSS custom properties (e.g., `var(--gold)`) instead of hardcoding hex values
2. **Maintain the single-breakpoint pattern** — responsive overrides go inside `@media (max-width: 900px)` only
3. **Keep JS minimal** — this is not a framework-based project; add vanilla JS only
4. **External resources** — only Google Fonts and Unsplash images are used; avoid adding new third-party dependencies without discussion
5. **No build step** — the file must remain browser-openable directly; do not introduce anything requiring compilation or bundling

### When adding new sections

- Follow the existing pattern: semantic HTML5 element, a descriptive class name, CSS in the `<style>` block scoped to that class
- Add scroll animation support by including the new cards in the Intersection Observer's `querySelectorAll` selector

### When adding backend integration

- The `submitLead()` function is the integration point for form submission
- Modal inputs: name (`type="text"`), email (`type="email"`), phone (`type="tel"`), move-in timeframe (`<select>`)
- Replace the `innerHTML` success state with a real API call before showing the confirmation

---

## What Does Not Exist Yet (Future Work)

- No actual form submission / lead routing
- No user authentication or accounts
- No real listing data (all content is hardcoded examples)
- No payment processing for the pricing tiers
- No analytics or tracking
- No SEO metadata beyond basic `<title>`
- No accessibility audit (ARIA attributes, keyboard navigation)
- No CI/CD pipeline
- No `README.md`
