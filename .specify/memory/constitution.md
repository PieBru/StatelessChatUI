<!--
  SYNC IMPACT REPORT — v1.0.0 (initial)
  ======================================
  Version change: none → 1.0.0 (initial creation)
  Modified principles: N/A (new document)
  Added sections:
    - Core Principles (5 principles derived from project context)
    - Architecture Constraints (browser requirements, CORS policy)
    - Development Standards (single-file philosophy, inline organization)
    - Governance (amendment procedure, versioning policy)
  Removed sections: N/A
  Templates requiring updates:
    - .specify/templates/plan-template.md — Constitution Check section uses
      placeholder "[Gates determined based on constitution file]"; aligns
      with principle-based gates ✅ compatible
    - .specify/templates/spec-template.md — No constitution references;
      functional requirements use generic MUST language ✅ compatible
    - .specify/templates/tasks-template.md — No constitution references;
      task structure is generic ✅ compatible
    - .specify/templates/checklist-template.md — No constitution references ✅ compatible
  Deferred TODOs:
    - TODO(RATIFICATION_DATE): Original project adoption date unknown;
      earliest git commit is 2025-12-17. Consider updating once confirmed.
-->

# StatelessChatUI Constitution

## Core Principles

### I. Single-File Architecture

Every feature and fix MUST reside within a single HTML file (`chat.html`). No external CSS, JavaScript, or asset files are permitted. The document is fully autonomous — it requires only browser access and network connectivity to function. This eliminates build pipelines, dependency management, and deployment complexity.

Rationale: A single-file deployment reduces friction for users (open-and-run) and developers (no toolchain). It also makes the project trivially portable and auditable.

### II. Client-Side Only

All conversation logic executes entirely in the browser. No server, no database, no persistent storage beyond what the browser API provides. API calls are made directly from the client to the target LLM endpoint. This architecture eliminates backend infrastructure costs and single points of failure.

Rationale: StatelessChatUI is designed for debugging, testing, and rapid prototyping. Introducing a backend would contradict its core value proposition of simplicity and portability.

### III. Pure Vanilla JavaScript

All code MUST be written in vanilla ES6+ JavaScript with zero external dependencies or libraries. No frameworks (React, Vue, Svelte), no bundlers (Webpack, Vite), and no package managers (npm, yarn). The project uses only browser-native APIs: Fetch, DOM, Dialog, and Web Components.

Rationale: Zero dependencies guarantee the project works everywhere a modern browser runs, without install steps or version conflicts. It also keeps the file size minimal and the codebase fully self-documenting.

### IV. Dark-Mode-First UI

The interface MUST default to a dark color scheme with minimal visual redundancy. Light-mode support is not required unless explicitly requested as a feature. UI elements should be collapsible where space is limited, and the design should prioritize readability of chat content over decorative elements.

Rationale: Chat interfaces are read-heavy; dark mode reduces eye strain during extended use sessions. The minimalist aesthetic keeps focus on conversation content rather than chrome.

### V. API Compatibility with OpenAI v1 Spec

All API interactions MUST conform to the OpenAI Chat Completion v1 specification (`/v1/chat/completions` with SSE streaming). The interface MUST support the full message format (system, user, assistant, tool) and handle extended thinking tokens (`<thinking>` tags, `reasoning_content`, and `thinking` content blocks). CORS compatibility is required — if the target API does not allow cross-origin requests, a proxy is the user's responsibility.

Rationale: OpenAI v1 has become the de facto standard for LLM APIs. Supporting this spec ensures broad compatibility with OpenAI, Ollama, LM Studio, vLLM, and other compatible services.

## Architecture Constraints

### Browser Requirements

- Modern ES6+ support (ES2020+ recommended)
- Fetch API with ReadableStream support for SSE streaming
- `<dialog>` element or equivalent polyfill
- Canvas or DOM-based Markdown rendering
- File API for import/export operations

### CORS Policy

The application makes direct client-side HTTP requests. API providers must either:
1. Return appropriate CORS headers (`Access-Control-Allow-Origin`), OR
2. Be accessed through a user-configured proxy

The application MUST NOT ship with any built-in CORS proxy or backend relay. Proxy configuration is the user's responsibility.

### File Size Budget

- Target uncompressed size: under 50 KB for `chat.html`
- CSS (inline `<style>`): under 5 KB
- JavaScript (inline `<script>`): under 40 KB
- HTML structure (inline): under 5 KB

Exceeding these budgets requires explicit justification and review.

## Development Standards

### Single-File Organization

All code resides in `chat.html` with three clearly delineated sections:
1. CSS within `<style>` block at the top of `<head>`
2. HTML body structure in `<body>`
3. JavaScript within `<script>` block at the end of `<body>`

Sections must be labeled with comment markers for easy navigation.

### Code Quality

- No trailing whitespace; single blank line between logical sections
- Functions and variables use descriptive camelCase names
- Comments explain *why*, not *what* (the code should be self-documenting)
- Error handling uses try/catch with user-friendly messages
- No console.log in production code; use a dedicated logger or remove debug output

### Testing Approach

Since this is a single-file client application:
- Manual testing via browser is the primary verification method
- Import/export round-trip tests (export → import → compare) are the closest equivalent to automated tests
- Edge cases to verify: empty conversations, very long messages, malformed JSON imports, CORS failures

## Governance

### Amendment Procedure

This constitution supersedes all other development guidance for StatelessChatUI. Amendments require:
1. Documentation of the change in this file
2. Version increment per semantic versioning (see below)
3. Alignment with existing principles — changes that contradict established principles require a MAJOR version bump and explicit rationale

### Versioning Policy

- **MAJOR**: Backward-incompatible changes to core principles or architecture (e.g., introducing a build pipeline, adding server-side components)
- **MINOR**: New principles added, existing principles materially expanded, or new sections added
- **PATCH**: Clarifications, wording improvements, typo fixes, non-semantic refinements

### Compliance Review

Before committing changes to `chat.html`:
1. Verify the change does not introduce external dependencies
2. Verify the file remains under the 50 KB budget (or document justification if approaching it)
3. Verify dark-mode-first design is maintained
4. Verify OpenAI v1 spec compatibility is preserved

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): Project inception is 2025-12-17 (earliest git commit); constitution ratified on 2026-05-01 | **Last Amended**: 2026-05-01
