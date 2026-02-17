# Claude Code Xcode Safety Guide

## Project Overview

A single-page static documentation website teaching Apple platform developers to safely use Claude Code with `--dangerously-skip-permissions` for autonomous Xcode development across iOS, watchOS, tvOS, visionOS, and macOS. The guide implements an 8-layer defense strategy (git checkpoints, permission whitelists, safety hooks, CLAUDE.md guardrails, XcodeBuildMCP, slash commands/skills, simulator tools, daily workflow) to prevent file destruction and project corruption.

**Live repo:** `github.com/iGbenga/claude-code-ios-guide`

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Markup | HTML5 (semantic, single-file) |
| Styling | CSS3 with custom properties (CSS variables for theming) |
| Interactivity | Vanilla JavaScript (no frameworks) |
| Typography | Google Fonts: DM Sans (body), JetBrains Mono (code), Instrument Serif (headings) |
| Version Control | Git → GitHub |
| Deployment | Static hosting (any CDN, Firebase Hosting, Cloudflare Pages) |

**No build process.** No bundler, no npm dependencies, no compilation. `index.html` is the entire application.

## Key Files

| File | Purpose |
|------|---------|
| `index.html` | Entire application — markup, styles, scripts, content |
| `.claude/settings.local.json` | Project-specific Claude Code permission overrides |
| `CLAUDE.md` | This file — project rules for Claude Code |
| `.claude/docs/` | Extended documentation for specialized topics |

## Project Structure

```
claude-code-ios-guide/
├── index.html                      # The complete guide (single-page app)
├── CLAUDE.md                       # Project rules (this file)
└── .claude/
    ├── settings.local.json         # Local permission config
    └── docs/
        └── architectural_patterns.md   # Design patterns reference
```

## Content Structure (index.html sections)

The guide is organized into sequential steps with supporting sections:

1. **Prerequisites** (`#prereqs`) — Install Claude Code
2. **Safety Overview** (`#overview`) — Layered defense table
3. **Choose Your Path** (`#choose`) — Native vs container decision
4. **Step 1: Git Checkpoint** (`#step1`) — Shell alias for auto-commit
5. **Step 2: Permission Whitelist** (`#step2`) — settings.json config
6. **Step 2b: Safety Hooks** (`#step2b`) — PreToolUse/PostToolUse hooks
7. **Step 3: CLAUDE.md Guardrails** (`#step3`) — Project rules template
8. **Step 4: XcodeBuildMCP v2** (`#step4`) — MCP server for Xcode builds (Sentry)
9. **Step 5: Skills & Commands** (`#step5`) — Custom Claude skills/commands
10. **Step 6: Simulator Skill** (`#step6`) — iOS Simulator interaction
11. **Step 7: Daily Workflow** (`#step7`) — Branch isolation, code review, context mgmt
12. **DevContainer** (`#devcontainer`) — Docker-based alternatives (tabbed)
13. **Config Builders** (`#builders`) — Interactive CLAUDE.md generator
14. **Quick Reference** (`#reference`) — Searchable command cheatsheet
15. **Troubleshooting** (`#troubleshoot`) — Collapsible FAQ
16. **Xcode 26.3** (`#xcode26`) — Agentic coding integration

## Critical Rules

- **NEVER delete `index.html`** — it is the entire application
- **NEVER add external JS/CSS frameworks** — the project is intentionally dependency-free
- **NEVER split into multiple HTML files** — single-page architecture is by design
- **Preserve both dark and light theme variables** — dark mode uses cool blues/cyans, light mode uses warm tan/brown (inspired by yt-dlp guide)
- **Keep all CSS inline** in the `<style>` block — no external stylesheets
- **Keep all JS inline** in the `<script>` block — no external scripts
- **Test both themes** after any color/styling changes
- **Maintain mobile responsiveness** — test at 900px and 500px breakpoints

## Working With This File

### Editing content sections
Each section follows this HTML pattern:
```
<section class="section" id="sectionId">
  <div class="step-label">Step N <span class="step-time">· X minutes</span></div>
  <span class="section-icon">EMOJI</span>
  <h2>Title <em>Accent</em></h2>
  <!-- content -->
</section>
```

### Adding code blocks
```html
<div class="code-block">
  <div class="code-header">
    <span class="code-label">Description</span>
    <button class="code-copy" onclick="copyCode(this)">Copy</button>
  </div>
  <pre><code>command here</code></pre>
</div>
```

### Adding callout boxes
Types: `tip`, `warning`, `danger`, `info`
```html
<div class="callout tip">
  <span class="callout-icon">EMOJI</span>
  <strong>Label:</strong> Message text.
</div>
```

### Updating sidebar
Add/remove links in the `<nav class="sidebar">` element. Each link targets a section `id`.

## Theming

CSS variables are defined in two blocks:
- `[data-theme="dark"]` — index.html:18-51
- `[data-theme="light"]` — index.html:53-86

All colors must use CSS variables (`var(--name)`), never hardcoded hex values in component styles.

## Testing

No automated tests. Manual verification:
```bash
# Open in browser
open index.html

# Test both themes (click toggle in top-right)
# Test mobile view (resize to <900px)
# Test all copy buttons
# Test all FAQ collapsibles
# Test all sidebar links scroll to correct sections
```

## Additional Documentation

Check these files when working on specialized topics:

| File | When to Check |
|------|---------------|
| `.claude/docs/architectural_patterns.md` | Modifying component patterns, theme system, interactivity, or responsive behavior |
