# AGENTS — StatelessChatUI Quick Reference

## What This Project Is

A single-file, stateless chat interface for LLM APIs. Everything lives in `chat.html` — no build pipeline, no dependencies, no server. Open it in a browser and it works.

**Full constitution**: `.specify/memory/constitution.md` (v1.0.0)

---

## Non-Negotiable Constraints

### 1. Single File Only
- **All code goes in `chat.html`**. No external CSS, JS, images, or fonts.
- No new files may be added without explicit permission and a constitution MAJOR bump.
- The file is the entire application — HTML + inline `<style>` + inline `<script>`.

### 2. Zero Dependencies
- Vanilla ES6+ JavaScript only. No frameworks (React, Vue, Svelte), no bundlers (Webpack, Vite), no package managers (npm, yarn).
- Only browser-native APIs: Fetch, DOM, Dialog, File API, ReadableStream.

### 3. Client-Side Only
- No backend, no database, no persistent storage beyond browser APIs.
- API calls go directly from browser to the target LLM endpoint.
- The app MUST NOT include any built-in CORS proxy or server relay.

### 4. Dark-Mode-First UI
- Default theme is dark. Minimal visual redundancy.
- Light-mode is not required unless explicitly requested as a feature.
- Collapsible elements where space is tight; content takes priority over chrome.

### 5. OpenAI v1 Spec Compatibility
- `/v1/chat/completions` with SSE streaming.
- Full message format: system, user, assistant, tool roles.
- Extended thinking support: `<thinking>` tags, `reasoning_content`, `thinking` content blocks.

---

## File Structure

```
StatelessChatUI/
├── chat.html              # The entire application (~56 KB uncompressed)
│   ├── <style> block      # CSS — top of <head> (lines ~7-129)
│   ├── <body>             # HTML structure (lines ~130-220)
│   └── <script> block     # JavaScript — end of <body> (lines ~221-1070)
├── README.md              # User-facing documentation
├── LICENSE                # Apache 2.0
├── AGENTS.md              # This file — agent quick reference
└── .specify/              # SPECKIT tooling (plans, templates, constitution)
    ├── memory/constitution.md
    └── templates/         # Plan, spec, tasks, checklist templates
```

---

## Coding Conventions

| Rule | Detail |
|------|--------|
| **Naming** | Descriptive camelCase for functions and variables |
| **Comments** | Explain *why*, not *what* — code should be self-documenting |
| **Whitespace** | No trailing whitespace; single blank line between logical sections |
| **Error handling** | try/catch with user-friendly messages; never expose raw errors to the user |
| **Debug output** | No `console.log` in production code — remove or replace with a logger |

---

## File Size Budget

| Section | Target Max | Current |
|---------|-----------|---------|
| Total `chat.html` | < 50 KB uncompressed | ~56 KB |
| CSS (`<style>`) | < 5 KB | ~1 KB |
| JS (`<script>`) | < 40 KB | ~54 KB |
| HTML structure | < 5 KB | ~1 KB |

**Note**: The file currently exceeds the budget. Any change approaching or exceeding these limits requires explicit justification. Consider refactoring before adding new features.

---

## Testing Approach

This is a single-file client app — there are no automated tests. Testing is manual:

1. **Open `chat.html` directly in a browser** (double-click or `open chat.html`)
2. **Import/export round-trip**: Export conversation as JSON → import it back → verify content matches
3. **Edge cases to verify after changes**:
   - Empty conversations
   - Very long messages / many messages
   - Malformed JSON imports (graceful error handling)
   - CORS failures (network errors)
   - Extended thinking rendering (`<thinking>` tags, `reasoning_content`)
   - Streaming behavior (SSE delta accumulation)

---

## Before You Commit — Compliance Checklist

Run through these before committing any change to `chat.html`:

- [ ] No external dependencies were introduced
- [ ] File size is within budget (or justification is documented)
- [ ] Dark-mode-first design is maintained
- [ ] OpenAI v1 spec compatibility is preserved
- [ ] New code follows the coding conventions above
- [ ] Manual testing was performed in a browser

---

## Common Tasks

### Adding a new feature
1. Find the right section in `chat.html` (CSS → HTML → JS)
2. Write minimal, self-contained code
3. Test manually in browser
4. Verify compliance checklist passes
5. Commit with a descriptive message

### Fixing a bug
1. Locate the relevant code section
2. Apply the fix inline — no new files
3. Test the specific scenario + regression check
4. Update any related comments if needed

### Refactoring
1. Ensure the refactored code produces identical behavior
2. Prefer extracting small helper functions over restructuring large blocks
3. Verify file size doesn't increase significantly

---

## What NOT To Do

- ❌ Add npm packages, CDN links, or external scripts
- ❌ Create new files (CSS, JS, images, components)
- ❌ Introduce a backend or server component
- ❌ Add a CORS proxy or relay
- ❌ Switch to a framework or build tool
- ❌ Break OpenAI v1 spec compatibility
- ❌ Ship code with `console.log` statements

---

## Related Files

- **Full constitution**: `.specify/memory/constitution.md` — all principles, governance, versioning policy
- **README.md**: User-facing documentation (features, usage, API config)
- **`.specify/templates/`**: SPECKIT plan/spec/tasks/checklist templates for feature development
- **`LICENSE`**: Apache 2.0
