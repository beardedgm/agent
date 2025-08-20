# AGENT.md — Project Contribution & Coding Standards (HTML5 • Bootstrap 5 • Vanilla JS)
This AGENT.md provides clear instructions for OpenAI Codex and similar AI agents when working in this repository. It defines conventions, structure, and commands so that generated code aligns with the project’s norms.

## Scope
- Technology stack: the project uses HTML5, Bootstrap 5 (always favour utilities and pre‑built components), and vanilla JavaScript organised as ES modules.
- Data storage: prefer client‑side storage for state and persistence. Use IndexedDB for structured data (e.g. maps, images, uploads, saved states) and localStorage for small preference values. Avoid adding a server or remote storage unless specifically directed.

## Core Principles
- Reuse patterns: adhere to existing component and utility patterns; avoid reinventing solutions.
- Keep changes focused: one feature or bug fix per pull request. Do not bundle unrelated changes.
- Simplicity over cleverness: prioritise readable, straightforward code over obscure optimisations.
- Modular files: aim for files ≤ 300 lines. If a module grows larger, split it into smaller ES modules organised by feature.
- No inline scripts or styles: keep JavaScript in .js modules and CSS in stylesheets; follow Content‑Security‑Policy best practices.
- Environment boundaries: respect environment separation. Never modify .env or other secrets without approval.

## Code Style
-Indentation: use 2‑space indents, UTF‑8 encoding with LF line endings.
-JavaScript: use const/let (avoid var), semicolons, and ES module syntax (import/export). Prefer named exports.
-Naming: file and CSS class names in kebab‑case; JavaScript variables and functions in camelCase; classes in PascalCase.
-Comments: write clear comments for non‑obvious logic and document exported functions.

## HTML
-Use semantic tags (<header>, <main>, <section>, <form>, etc.) to structure content.
-Label all form controls (<label for="…">), ensuring accessible names for inputs.
-Keep markup free of inline JavaScript. Scripts should be linked with <script type="module" src="…"></script> at the end of the body for progressive rendering.

## CSS / Bootstrap
- Order CSS rules as follows: 1) Bootstrap utilities, 2) Bootstrap components, 3) scoped custom styles.
- Avoid deep selectors and !important; favour composable classes and CSS variables.
- Define theme tokens (colours, spacing) as CSS custom properties and reference them consistently.

## JavaScript
- One feature per module: group related functions together and export them. For example, put grid drawing in grid.js, token logic in tokens.js and persistence helpers in storage.js.
- Event delegation: add as few event listeners as possible by attaching them to parent elements and using event.target.dataset or class checks to handle specific controls.
- Avoid global state: encapsulate mutable state in modules or class instances. Do not attach variables to the window object.
- Null‑checks and error handling: always check for null/undefined values before accessing properties. Handle asynchronous errors with try…catch or .catch().
- Pure functions: separate pure logic from DOM‑manipulation code whenever possible. Pure functions are easier to test.
- AbortController: for operations that may need cancellation (e.g. fetches, timers), use AbortController to clean up listeners and avoid memory leaks.
- Persistence: store complex objects (maps, tokens) in IndexedDB via a helper module; store small user preferences (e.g. UI settings, defaults) in localStorage. Provide asynchronous APIs that return promises.

## Accessibility (WCAG 2.1 AA)
- Maintain a minimum colour contrast ratio of 4.5:1 for text and interactive elements.
- Provide visible focus outlines on all interactive controls; do not remove outlines without replacing them with another visible indicator.
- Ensure full keyboard support; all interactions (revealing/hiding fog, placing tokens, navigating modals) should be possible via keyboard shortcuts and focusable controls.
- Use alt attributes on images and meaningful aria-* attributes only when semantic elements cannot suffice.
- Use aria-live regions for dynamic updates (e.g. status messages) so assistive technologies can announce changes.

## Performance
- Keep critical path resources (HTML, CSS, JS) under 200 KB total to minimise initial load times.
- Use Bootstrap from a CDN; defer or lazy‑load non‑critical JavaScript modules.
- Optimise and resize images; prefer modern formats like WebP.
- Debounce expensive operations (e.g. canvas redraws, IndexedDB writes). For repeated rendering, use requestAnimationFrame.

## Security
- Follow Content‑Security‑Policy guidelines. Avoid inline event handlers and eval()/Function().
- Sanitize and validate all user‑supplied data (including imported JSON). When rendering user content, write to textContent rather than innerHTML.
- Minimise third‑party scripts; pin dependency versions and audit for vulnerabilities.

## Dependencies
- Avoid heavy libraries such as jQuery; favour lightweight, well‑maintained packages. Document new dependencies (purpose, size, license) in the README.

## Testing
- Write unit tests for utility functions and logic modules. Use mocks for Canvas, IndexedDB and localStorage.
- Write DOM/interaction tests for key flows (fog revealing, token placement, undo/redo, import/export).
- Keep tests in /tests, separate from application code. Do not include test helpers in production modules.

## Workflow
- Pull requests: each PR should address a single issue or feature. Include a brief description of what changed, why, and how to test it.
- CI requirements: all tests must pass before merging. Run npm test and lint scripts locally before pushing.
- Provide instructions to start or stop any dev server in the PR description if relevant. Always stop a running server before starting a new one.

## Server Lifecycle
- Do not run multiple dev servers concurrently. Use a single npm start (or documented script) for local development, and stop it (Ctrl+C) before running another command.

## Checklist (per change)
Before submitting your PR, verify the following:
- Reused existing patterns; no unnecessary duplication.
- Changes are focused and contained within allowed directories.
- Style and naming rules are followed.
- IndexedDB and localStorage are used appropriately for persistence.
- Unit/interaction tests are added or updated as needed.
- Accessibility and security requirements are met.
- Performance impact has been considered and minimised.
- Run and test instructions are provided.
