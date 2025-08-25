[![Releases](https://img.shields.io/github/v/release/ItsChamal/wan22-brkn-prompt-helper?color=informational&label=Releases)](https://github.com/ItsChamal/wan22-brkn-prompt-helper/releases)

# WAN 2.2 BRKN Prompt Helper — Gemini AI for Images & Video

AI prompt generator for images, video, JSON, and scene scripts. Built with React, TypeScript, Vite and Google Gemini. Use it to prototype prompts, create data payloads, and generate structured prompt templates for multiple LLM providers.

[Download the release file from the Releases page and execute it.](https://github.com/ItsChamal/wan22-brkn-prompt-helper/releases)

![AI prompts illustration](https://images.unsplash.com/photo-1564866657315-211b4d8df3e4?ixlib=rb-4.0.3&auto=format&fit=crop&w=1400&q=80)

Table of contents
- Features
- Quick look
- Prompt types
- Providers
- UI & components
- Quick start
- Download & run (Releases)
- Local development
- Configuration
- Examples
- Testing & CI
- Contributing
- License
- Acknowledgements

Features
- Multi-format prompt generation: images, videos, clapperboard scenes, JSON, and custom formats.
- Provider-agnostic LLM selection: switch between Google Gemini, OpenAI, Anthropic, Stability AI, Perplexity.
- AI suggestions: generate prompt drafts and variants with your chosen provider.
- Responsive UI: modern interface built with Radix UI and Tailwind CSS.
- Real-time generation: progress states and cancellable requests.
- Configurable parameters: set temperature, tokens, style, and length defaults.
- Mobile friendly: layout adapts to phone and tablet sizes.
- Export options: copy, download JSON, or save as a template.

Quick look
- Core stack: React 18, TypeScript, Vite.
- Styling: Tailwind CSS, Radix UI.
- LLM integration: modular provider adapters for Gemini, OpenAI, Anthropic, Stability, Perplexity.
- Output formats: raw text, structured JSON, clapperboards, prompts for image models.
- Runs in the browser; optional local server for API keys and server-side calls.

Prompt types
- Image prompt: seed, style, camera, lighting, environment, focal length.
- Video prompt: scene beats, shot list, motion notes, duration, codec hints.
- Clapperboard: shot name, take, slate, camera, roll, director notes.
- JSON payload: schema-driven generator for structured data prompts.
- Custom templates: user-defined tokens and placeholders, saved as templates.

Providers (LLM adapters)
- Google Gemini (default): tuned defaults for Gemini text and text2image where applicable.
- OpenAI: chat and completion modes, model selection.
- Anthropic: Claude style prompts and token control.
- Stability AI: image-focused prompts, seed settings.
- Perplexity: web-aware assistant prompts.

UI & components
- Component library: Radix UI primitives for dialogs, select, toggle, and tabs.
- Layout: responsive grid, compact mobile controls.
- State: TypeScript-first store, local storage for templates.
- Accessibility: keyboard navigation and proper aria attributes.
- Visuals: syntax-highlighted generated output and copy-to-clipboard.

Quick start

Prerequisites
- Node 18+ or compatible LTS.
- Git.
- API keys for providers you plan to use (Gemini, OpenAI, etc.). Keys may run in a server proxy.

Local dev (fast)
1. git clone https://github.com/ItsChamal/wan22-brkn-prompt-helper.git
2. cd wan22-brkn-prompt-helper
3. npm ci
4. npm run dev
5. Open http://localhost:5173

Download & run (Releases)
- Download the release file from this Releases page: https://github.com/ItsChamal/wan22-brkn-prompt-helper/releases
- Look for a release asset named like wan22-brkn-prompt-helper-v2.2.x-linux.tar.gz or wan22-brkn-prompt-helper-v2.2.x-macos.zip.
- Download the asset that matches your OS.
- Example commands for a Linux tarball:
  - tar -xzf wan22-brkn-prompt-helper-v2.2.0-linux.tar.gz
  - cd wan22-brkn-prompt-helper-v2.2.0
  - chmod +x run.sh
  - ./run.sh
- Example steps for macOS zip:
  - unzip wan22-brkn-prompt-helper-v2.2.0-macos.zip
  - open the bundle or run the included run.sh

If the Releases link does not work in your environment, check the Releases section on the repository page in GitHub.

Architecture & design
- Modular provider adapters: each provider has an adapter that maps app requests to provider API calls. Add a new provider by implementing the adapter interface.
- Prompt engine: templates use a small templating language (tokens, loops, conditionals) and a formatter layer that outputs plain text or JSON.
- UI <-> Engine: React components feed template variables to the engine; engine returns variants and structured output.
- Export layer: supports save to local templates, download JSON, copy to clipboard, and webhook push.

Configuration & env
- .env.local (local development)
  - VITE_API_PROXY_URL (optional): URL for a server proxy that holds your API keys.
  - VITE_DEFAULT_PROVIDER=gemini
  - VITE_DEFAULT_TEMPERATURE=0.7
- server/.env
  - GEMINI_API_KEY
  - OPENAI_API_KEY
  - ANTHROPIC_API_KEY
  - STABILITY_API_KEY
- The app reads keys only from the server proxy by default to avoid leaking keys client-side.

Prompt examples

Image prompt (example)
- Template:
  - Subject: "urban skyline"
  - Style: "neo-noir, cinematic, film grain"
  - Camera: "35mm, shallow depth of field"
- Generated:
  - "Urban skyline at dusk, neon reflections on wet asphalt, cinematic framing, 35mm, f/2.0, film grain, volumetric lighting, high contrast."

Video prompt (example)
- Template:
  - Beats: establishing shot, close-up, reveal
  - Motion: "slow dolly in"
- Generated:
  - "Scene 1: Establishing wide of city street at dawn. Scene 2: Slow dolly in to character in doorway. Scene 3: Close-up reveal as rain hits the pavement."

JSON prompt (schema-driven)
- Input schema:
  - { title: string, duration: number, scenes: [{ id, desc, duration }] }
- Generated:
  - { "title": "Midnight Run", "duration": 120, "scenes": [ { "id": 1, "desc": "Open on street", "duration": 10 } ] }

Provider examples (pseudo)
- Gemini (text):
  - Request: send prompt with temperature 0.6, max tokens 400.
  - Response: multi-variant text with suggestions list.
- Stability (image seed):
  - Request: send prompt with seed, sampler, and steps.
  - Response: text-to-image-ready prompt string.

Extending providers
- Add adapter under src/providers.
- Follow IProvider interface: buildRequest, parseResponse, error handling.
- Update provider select UI to include the new identifier.

Export & integration
- Copy: clipboard button for generated text.
- Download: JSON/CSV/text export for templates and outputs.
- Templates: save and load from local storage; export as .prompt.json.
- Webhook: send generated output to a configured webhook URL for downstream pipelines.

Testing & CI
- Unit tests: Vitest for core logic and template engine.
- Integration: test provider adapters with mocked HTTP.
- CI pipeline: runs build, lint, and tests on push to main. See .github/workflows for example config.

Performance tips
- Cache provider responses for repeated prompts.
- Hard-limit token usage in UI to avoid large bills.
- Use server-side proxy for heavy workloads and batching.

Security
- Keep API keys on the server. The app uses a proxy pattern for calls that require secret keys.
- Do not commit .env with keys.
- Sanitize user templates that allow webhooks or shell actions.

Contributing
- Fork the repo and open a pull request.
- Follow the code style: TypeScript types, strict mode, and Tailwind utility classes.
- Include tests for logic changes.
- Add docs for new features and provider adapters.

Roadmap
- Template marketplace: share and import prompt templates.
- Live preview: visual preview for image and video prompts using Stability or Gemini image API.
- Advanced scheduler: batch generation and schedule webhook pushes.

Releases
[Download the release file from the Releases page and execute it.](https://github.com/ItsChamal/wan22-brkn-prompt-helper/releases)
- Use the release asset that matches your OS.
- The release package includes a run script and a small local server for the proxy if needed.

Screenshots
- UI: prompt editor and provider selector
  - ![Editor screenshot](https://raw.githubusercontent.com/ItsChamal/wan22-brkn-prompt-helper/main/assets/screenshot-editor.png)
- Output: generated prompt variants
  - ![Output preview](https://raw.githubusercontent.com/ItsChamal/wan22-brkn-prompt-helper/main/assets/screenshot-output.png)

Common commands
- npm run dev — start local dev server
- npm run build — produce production bundle
- npm run test — run unit tests
- npm run lint — run linter

Folder layout
- src/ — React app
  - components/ — UI components
  - providers/ — LLM adapters
  - engine/ — prompt engine and template formatter
  - styles/ — Tailwind config and global CSS
- server/ — optional proxy server (Node/Express)
  - routes/ — provider routes
  - middleware/ — auth and rate limit
- assets/ — images and icons
- scripts/ — release helpers

Roadblocks & troubleshooting
- If provider rejects a request, verify API key and rate limits.
- If generation stalls, check proxy logs and the client network tab.
- If static assets fail to load after build, ensure correct base path in Vite config.

License
- MIT License. See LICENSE file for details.

Acknowledgements
- Radix UI for primitives.
- Tailwind CSS for utility-first styling.
- Unsplash and public assets for illustrations and mock screenshots.