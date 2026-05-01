---
description: "Task list for collapsible left toolbar feature"
---

# Tasks: Collapsible Left Toolbar

**Input**: Design documents from `/specs/001-collapsible-left-toolbar/`
**Prerequisites**: plan.md (required), spec.md (required)

**Tests**: Manual browser testing only — no automated tests for this single-file client app.

## Format: `[ID] [P?] Description`

- **[P]**: Can run in parallel (different sections of chat.html)
- Include exact file paths and line ranges

## Phase 1: CSS Restructure

**Purpose**: Replace the horizontal header layout with a vertical left panel + grid-based layout.

- [ ] T001 [P] In `<style>` block, replace `.wrap` grid from `grid-template-rows:auto 1fr auto` to `grid-template-columns:auto 1fr; grid-template-rows:1fr` to accommodate sidebar
- [ ] T002 [P] Create `.left-panel` CSS class: fixed/relative position along left edge, ~280px width, flex column layout, dark panel background matching `--panel`, border-right with `--line`
- [ ] T003 [P] Create `.left-panel.collapsed` state: width collapses to ~40px (toggle button only), chat area expands to fill remaining space. Use CSS transition on width for smooth animation (~300ms ease).
- [ ] T004 [P] Relocate all header control styles (`header input`, `header select`, `header .row`, `header label.small`, `header button`) to `.left-panel` equivalents — preserve all existing styling, colors, focus states
- [ ] T005 [P] Create toggle button CSS: visible at left edge regardless of panel state, ~40px width, dark background, hover/active states matching existing button styles. Add aria-label="Toggle settings panel" for accessibility.
- [ ] T006 [P] Update responsive breakpoints: at <600px, left panel becomes full-height overlay (position: fixed, z-index: 100) with backdrop; at 600-980px, reduce panel width to ~240px
- [ ] T007 [P] Ensure `.wrap` grid properly accounts for left panel width — chat area (`main#scroller`) uses `min-width:0` to prevent overflow

## Phase 2: HTML Restructure

**Purpose**: Move DOM elements from `<header>` into the new left panel structure.

- [ ] T008 Move `<header>` element entirely inside a new `<nav class="left-panel">` container
- [ ] T009 Place brand/title and promo link at top of left panel (collapsed state shows only toggle + icon)
- [ ] T010 Move all control groups (API Base, Model, API Key, Import/Export/Load Models/Info buttons, token rate) into the left panel in the same visual order
- [ ] T011 Add a `<button id="panelToggle" class="panel-toggle">☰</button>` outside the panel for open/close toggle — visible at all times, positioned at left edge
- [ ] T012 Remove or replace the old `<header>` element entirely — the left panel replaces it
- [ ] T013 Verify `.wrap` grid structure: `nav.left-panel` + `main#scroller` as children; footer remains in its current position within main or moves to a new grid arrangement

## Phase 3: JavaScript Toggle Logic

**Purpose**: Implement panel open/close toggle with state persistence.

- [ ] T014 Add DOM reference for the new toggle button and left panel in the `els` object
- [ ] T015 Implement toggle click handler: toggles `.collapsed` class on `.left-panel`, updates toggle button icon (☰ ↔ ✕ or similar)
- [ ] T016 Persist panel state (open/collapsed) in localStorage, restore on page load — default to collapsed for maximum chat space
- [ ] T017 Ensure keyboard accessibility: Escape key closes panel if open; Tab navigation order is logical through panel controls
- [ ] T018 Verify that all existing event listeners continue to work — no element IDs changed, all controls functional

## Phase 4: Functional Verification & Polish

**Purpose**: Ensure every existing feature works identically from the new layout.

- [ ] T019 Test: API base URL input, model selector, API key input — all function correctly from left panel
- [ ] T020 Test: Panel width ~280px when open (chat gains ~280px vertical space when collapsed, exceeding 200px SC target)
- [ ] T021 Test: Keyboard navigation — Escape closes open panel; Tab order flows logically through controls
- [ ] T022 Test: Promo link visible in left panel when open; toggle icon visible when collapsed
- [ ] T023 Test: Scroll position preserved exactly after toggle (scrollToBottom behavior unchanged)
- [ ] T024 Test: Import/Export buttons — file picker opens, downloads work
- [ ] T025 Test: Load Models button — fetches and populates model dropdown
- [ ] T026 Test: Info dialog — opens and displays correctly
- [ ] T027 Test: Token rate display — updates during streaming in the left panel
- [ ] T028 Test: Settings panel (⚙️) in footer — still accessible, layout not broken
- [ ] T029 Test: JSON Editor — still accessible, not clipped by panel
- [ ] T030 Test: Streaming chat — messages render correctly, auto-scroll works, thinking bubbles display
- [ ] T031 Test: Attachments (file upload) — chip display and remove functionality work
- [ ] T032 Test: Responsive behavior at 980px, 720px, 480px breakpoints — no layout breakage
- [ ] T033 Test: Rapid toggle clicks during animation — no visual glitches or broken state (animation interruptible)
- [ ] T034 Verify file size remains under 50 KB uncompressed; remove any dead CSS/JS from old header layout

## Dependencies & Execution Order

### Phase Dependencies

- **CSS Restructure (Phase 1)**: No dependencies — can start immediately. All sub-tasks [P] parallel.
- **HTML Restructure (Phase 2)**: Depends on Phase 1 CSS classes being defined. Sub-tasks sequential (DOM structure must be coherent).
- **JavaScript Toggle Logic (Phase 3)**: Depends on Phase 2 DOM elements existing. Sequential.
- **Functional Verification (Phase 4)**: Depends on all phases complete. Sequential testing.

### Within Each Phase

- CSS tasks (T001-T007) can be done in parallel by different sections of the stylesheet
- HTML tasks (T008-T013) must be sequential — each builds on the previous DOM structure
- JS tasks (T014-T018) must be sequential — toggle logic depends on DOM references
