# AGENT.md — Repo Rules (HTML • Bootstrap 5 • Vanilla JS)

**Purpose**
Keep this codebase simple, consistent, accessible, and secure. These rules guide any AI/code assistant working here.

**Tech Stack**

* HTML5
* Bootstrap 5 (utilities/components first)
* Vanilla JavaScript (ES modules)

---

## Core Operating Rules

1. **Iterate, don’t reinvent.** Reuse existing patterns before adding new ones.
2. **Small, focused changes.** Touch only code related to the task.
3. **Prefer simplicity & readability** over cleverness or premature optimization.
4. **No duplication.** Search before adding similar logic.
5. **Keep files small.** Target ≤300 lines; split into modules/components when larger.
6. **No inline JS/CSS.** Keep CSP-friendly by using external modules/styles.
7. **Respect environments.** Don’t leak test data into dev/prod; never change `.env` without explicit instruction.

## Formatting & Style

* Indentation: **2 spaces**; no tabs.
* Encoding/Endings: **UTF‑8**, **LF**.
* JS: **semicolons**, `const`/`let` (no `var`), **ES modules** only.
* Naming: `kebab-case` files & CSS classes; `camelCase` for JS identifiers.
* Max line length: \~100–120 chars.

## Project Layout

```
/assets/
  css/     # minimal, scoped custom CSS
  js/      # ES modules — one feature per file
  img/
  icons/
/components/  # reusable HTML snippets/partials
/tests/       # unit + lightweight DOM tests
```

## HTML Rules

* Use semantic landmarks (`header`, `nav`, `main`, `footer`).
* Label form controls; associate errors/help text with inputs.
* Load scripts as modules with `defer`:

  ```html
  <script type="module" src="/assets/js/app.js" defer></script>
  ```

## CSS / Bootstrap Rules

* **Bootstrap utilities/components first.**
* Keep custom CSS minimal & **scoped by a root class** (e.g., `.navbar-search { … }`).
* Avoid deep selectors and `!important`.
* Prefer CSS custom properties for theme tokens.

## JavaScript Rules

* One feature = one module; export named functions.
* Use `data-*` attributes for DOM hooks (avoid brittle class hooks).
* Prefer event delegation for dynamic content.
* Guard against null/undefined; no globals; no `eval`/Function constructors.
* Isolate DOM effects; keep logic pure where possible.
* Use `AbortController` to cancel fetches and listeners when appropriate.

## Accessibility (WCAG 2.1 AA)

* Contrast ≥ 4.5:1; keep visible focus outlines.
* Full keyboard support for all interactive UI.
* Provide `alt` text; use `aria-*` only when semantics aren’t enough.
* Use `aria-live` for async status/validation messages.

## Performance

* Keep **critical path < 200 KB** (HTML+CSS+JS) on first load.
* Use CDN for Bootstrap; lazy‑load non‑critical JS (`import()` when sensible).
* Optimize/resize images; prefer modern formats (with fallbacks as needed).

## Security

* CSP-friendly (no inline code/styles).
* Sanitize/encode any user-provided content before rendering.
* Keep third‑party scripts to a minimum; pin versions.

## Dependencies

* Avoid jQuery/heavy frameworks. Prefer zero‑dep or small, audited libs.
* Document any new dependency (purpose, size, license) in the PR/commit.

## Testing

* Provide unit tests for utilities and key logic.
* Add lightweight DOM interaction tests for critical flows.
* Tests own all mocks; no test artifacts in app code.

## Workflow & PRs

* Keep PRs single‑purpose and small.
* Include before/after notes or screenshots for UI changes.
* Ensure all tests pass before merge.

## Local Server Lifecycle

* Stop any running dev server before starting a new one.
* Provide exact start/stop commands in the PR/notes when relevant.

---

**Checklist (per change)**

* [ ] Reused an existing pattern where possible.
* [ ] Kept edits focused; no unrelated refactors.
* [ ] Followed formatting/naming and file-size guidance.
* [ ] Added/updated tests (including edge cases).
* [ ] Preserved accessibility & security rules.
* [ ] Verified performance impact is minimal.
* [ ] Provided clear run/test instructions if needed.
