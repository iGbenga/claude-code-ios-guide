# Architectural Patterns

## Single-Page Application Pattern

The entire guide lives in one `index.html` file. This is intentional — it makes the guide portable (share one file), eliminates build tooling, and ensures zero external dependencies. All CSS is in a `<style>` block, all JS is in a `<script>` block.

**Implication:** Never split this into multiple files. Adding a feature means editing `index.html`.

## CSS Custom Properties Theming

Two complete color palettes defined as CSS variables on `[data-theme="dark"]` and `[data-theme="light"]` selectors. Components reference variables exclusively — never hardcoded colors.

**Pattern:** Every new color must be added to BOTH theme blocks. The `setTheme()` JS function toggles the `data-theme` attribute on `<html>` and persists to `localStorage`.

**Variable naming convention:**
- `--bg-*` — backgrounds (primary, secondary, card, code, sidebar, hover)
- `--text-*` — text colors (primary, secondary, muted)
- `--accent*` — accent colors and their dim variants
- `--border*` — border colors
- `--code-*` — syntax highlighting colors

## Component Patterns

### Code Blocks
Structure: `.code-block` > `.code-header` (label + copy button) + `pre > code`. The `copyCode(this)` function traverses up to `.code-block`, finds the `pre`, copies its `textContent`, and shows a "Copied!" confirmation that resets after 2 seconds.

Syntax highlighting is done with `<span>` classes: `.comment`, `.string`, `.keyword`, `.flag` — all mapped to CSS variables.

### Callout Boxes
Four severity levels: `.tip` (green), `.warning` (orange), `.danger` (red), `.info` (cyan). Each uses the corresponding `--accent-*` and `--accent-*-dim` variables for border and background. In light mode, all four resolve to the same warm brown (monochromatic design).

### Collapsible FAQ Items
Pattern: `.faq-item` > `.faq-question` (click target) + `.faq-answer` > `.faq-answer-inner`. The `toggleFaq()` function closes all open items first (accordion behavior), then opens the clicked one by setting `maxHeight` to `scrollHeight`.

### Tab System
Tabs use `.tabs` container with `.tab-btn` buttons and `.tab-content` panels. Active state toggled via class. Tab switching is handled by `onclick` in the HTML.

## Sidebar Navigation

Fixed-position sidebar (`position: fixed`) with `280px` width. Main content has `margin-left: 280px` to compensate. On mobile (<900px), sidebar transforms offscreen and toggles via hamburger button.

Active link tracking uses `IntersectionObserver` with `rootMargin: '-20% 0px -70% 0px'` — this means a section is "active" when it's in the top 20-70% of the viewport.

## Responsive Design

Two breakpoints:
- `900px` — Sidebar collapses to overlay, main content fills width
- `500px` — Font sizes reduce, hero meta stacks vertically

Mobile sidebar uses `transform: translateX(-100%)` / `translateX(0)` with CSS transition for smooth slide-in.

## Event Handling

All interactivity uses inline `onclick` handlers rather than `addEventListener`. This is intentional for the single-file architecture — keeps event bindings visible next to the elements they affect, and avoids the need for DOM-ready initialization for most interactions.

Exception: The `IntersectionObserver` for sidebar tracking and the scroll listener for the back-to-top button are initialized in the `<script>` block at page load.

## Design Decisions

### Why no build tools?
Portability. The guide can be opened by dragging `index.html` into any browser. No `npm install`, no server, no compilation. This is critical for the target audience (iOS developers who may not have a web dev environment).

### Why inline everything?
Single file = single source of truth. No broken imports, no CORS issues when opening locally, no asset loading failures.

### Why two distinct theme palettes?
Dark mode uses cool blues/cyans (developer-friendly, matches terminal aesthetics). Light mode uses warm tan/brown (parchment-like, inspired by yt-dlp guide design). They are intentionally different moods, not just inverted colors.

### Why accordion FAQ instead of always-visible?
Troubleshooting content is reference material — users scan for their specific problem. Collapsed items reduce visual noise and let users find their issue faster by reading just the question headers.
