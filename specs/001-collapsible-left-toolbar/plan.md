# Implementation Plan: Collapsible Left Toolbar

**Branch**: `001-collapsible-left-toolbar` | **Date**: 2026-05-01 | **Spec**: [spec.md](../specs/001-collapsible-left-toolbar/spec.md)
**Input**: Feature specification from `/specs/001-collapsible-left-toolbar/spec.md`

## Summary

Move all controls currently in the horizontal top `<header>` into a collapsible left-side panel, freeing vertical screen space for the chat area. The toggle control remains visible at all times. All existing functionality (API configuration, model loading, import/export, settings, token rate) is preserved identically — only the layout changes.

## Technical Context

**Language/Version**: Vanilla ES6+ JavaScript (no frameworks, no dependencies)  
**Primary Dependencies**: None — browser-native APIs only (DOM, CSS transitions, Fetch API)  
**Storage**: N/A (no data model changes)  
**Testing**: Manual browser testing; import/export round-trip verification  
**Target Platform**: Modern browsers (ES2020+), desktop and tablet viewports  
**Project Type**: Single-file client-side web application  
**Performance Goals**: Panel toggle animation ≤ 300ms, no visual flicker during reflow  
**Constraints**: All code must remain in `chat.html` — no external files. File size budget under 50 KB uncompressed. Dark-mode-first design maintained. OpenAI v1 API compatibility preserved.  
**Scale/Scope**: Single-page UI layout change affecting CSS grid/flexbox structure and DOM element positions. ~1300 lines total.

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- ✅ **Single-File Architecture**: All changes remain within `chat.html`. No new files created.
- ✅ **Client-Side Only**: No backend changes. Pure CSS + JS layout reorganization.
- ✅ **Pure Vanilla JavaScript**: Uses only DOM manipulation, CSS transitions, and native APIs.
- ✅ **Dark-Mode-First UI**: Existing CSS variables (`--bg`, `--panel`, `--ink`, etc.) reused. No new design tokens.
- ✅ **API Compatibility with OpenAI v1 Spec**: No API interaction changes. All existing control behavior preserved.

## Project Structure

### Documentation (this feature)

```text
specs/001-collapsible-left-toolbar/
├── plan.md              # This file
├── spec.md              # Feature specification
└── tasks.md             # Task breakdown (/speckit.tasks output)
```

### Source Code (repository root)

```text
chat.html                # Only source file — CSS, HTML, JS all inline
├── <style> block        # Lines ~7-250: CSS variables, header styles, responsive rules
├── <body>               # Lines ~231-360: DOM structure (header, main, footer)
└── <script>             # Lines ~361-1301: Application logic
```

**Structure Decision**: Single-file layout. The `<header>` element is replaced by a `<nav class="left-panel">` alongside the existing `.wrap` grid. CSS Grid `grid-template-columns` replaces the current `grid-template-rows:auto 1fr auto` to accommodate the sidebar. All JavaScript event listeners reference the same element IDs — no ID changes required.

## Complexity Tracking

No constitution violations. This is a straightforward layout reorganization with no architectural complexity.
