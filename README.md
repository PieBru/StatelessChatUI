# StatelessChatUI

A single HTML file. Zero dependencies. No build tools. No server. Just open it in a browser and chat with any OpenAI-compatible LLM API.

## Philosophy

StatelessChatUI is built on a single conviction: **the simplest possible tool that does the job well**. Everything in this project flows from that principle:

- **One file.** The entire application is `chat.html` — HTML + CSS + JavaScript, all inline. No external assets, no imports, no packages.
- **No build pipeline.** No bundlers, no transpilers, no package managers. If it runs in a modern browser, it works.
- **No server.** All conversation logic lives in the browser. API calls go directly from client to LLM endpoint. There is no backend, no database, no persistent storage beyond what the browser provides.
- **Dark-mode-first.** The interface defaults to dark with minimal visual clutter. Content takes priority over chrome.

This isn't a framework or a platform. It's a tool you open, use, and close — then do it again tomorrow, on any machine, with zero setup.

## Deployment

**That's it.** Copy `chat.html` anywhere — your desktop, a USB stick, a web server — and open it in a browser. The file is self-contained and requires nothing else.

```bash
# That's the entire deployment process:
open chat.html
```

Optional: if the target API enforces CORS restrictions, you can serve the file from any static server (the app itself doesn't care):

```bash
python -m http.server 8000
# or
npx http-server -p 8000
```

But even this is unnecessary for APIs that allow cross-origin requests — which includes most OpenAI-compatible services and local inference servers.

## Features

**Chat**
- Streaming responses with delta accumulation (SSE)
- Native support for extended thinking: `<thinking>` tags, `reasoning_content`, and `thinking` content blocks
- Collapsible reasoning display — reasoning steps stay hidden until you expand them
- Markdown rendering with syntax highlighting
- Token rate metrics during streaming

**Conversation Control**
- Full message history manipulation via integrated JSON editor
- Append or replace messages directly
- Import/export conversations as JSON or JSONL
- Drag-and-drop import

**API Integration**
- OpenAI Chat Completion v1 spec (`/v1/chat/completions` with SSE)
- Dynamic model loading via `/v1/models` endpoint
- Temperature control (0.0 – 2.0), max tokens, system prompt configuration
- File/vision support for multimodal LLMs
- Auto-detection of o1 models (disables system prompts per API constraints)

## Technical Specification

| Aspect | Detail |
|--------|--------|
| **Architecture** | Single HTML file, no dependencies |
| **Size** | ~56 KB uncompressed (`chat.html`) |
| **Language** | Vanilla ES6+ JavaScript |
| **Dependencies** | None — zero external libraries |
| **Browser** | Modern ES6+ (ES2020+ recommended) |
| **API** | OpenAI Chat Completion v1 spec |

## Usage

### API Configuration

Set the base URL and optional API key in the header:

1. **Base URL** — your LLM endpoint (e.g., `https://api.openai.com/v1`, `http://localhost:11434/v1`)
2. **API Key** — required for OpenAI, often not needed for local inference
3. **Model** — type manually or click "Load Models" to fetch from `/v1/models`

### Conversation Management

- **JSON Editor:** Open "Raw JSON Editor" below the chat log to inspect or manipulate message history directly. Use "Apply (Replace)" to override or "Apply (Append)" to inject messages.
- **Import/Export:** Export downloads a timestamped JSON file. Import via drag-and-drop or file picker. Supports standard OpenAI message arrays and custom wrappers with a top-level `system` field.

## API Compatibility

**Works with:**
- OpenAI and OpenAI-compatible public APIs
- Local inference: Ollama, LM Studio, llama.cpp, vLLM

**Requirements:**
- `/v1/chat/completions` endpoint with SSE streaming
- CORS headers from the API provider, or a proxy if needed
- Optional: `/v1/models` for automatic model discovery

## Use Cases

- **Prompt engineering** — rapid test environment with zero setup
- **Conversation analysis** — surgical message manipulation for debugging context handling
- **Local inference** — direct connection to local LLM servers without cloud dependencies
- **Research** — complete conversation exports for systematic evaluation of model behavior

## Design Choices (Not Limitations)

These are intentional constraints, not missing features:

- **No server-side persistence.** Conversations exist only in browser state. This keeps the app stateless and portable.
- **No function calling.** The JSON editor lets you inject any message format manually — no need for a function-calling abstraction.
- **Approximate token counting.** Based on streaming metrics, not tokenizer-accurate. Sufficient for rate display; use a proper tokenizer if precision matters.
- **Single file only.** Every feature lives in `chat.html`. No external files, no imports, no build step. This is the architecture, not a limitation.

## Development

The project has exactly one source file: `chat.html` (~1300 lines). All code is organized inline:

- **CSS:** `<style>` block at the top of `<head>`
- **HTML:** Body structure in `<body>`
- **JavaScript:** `<script>` block at the end of `<body>`

No build pipeline. No bundling. No dependencies. Pure vanilla.

If a proposed change would require adding files, installing packages, or running a build step — it violates the architecture and should be rejected. See `.specify/memory/constitution.md` for the full set of non-negotiable principles.

## License

Apache 2.0
