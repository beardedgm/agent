# AGENT.md – Project Coding Guidelines for OpenAI Codex

**Tech Stack:** HTML, Bootstrap 5, and vanilla JavaScript. No frameworks unless explicitly requested.

## Purpose

This AGENT.md provides clear instructions for OpenAI Codex and similar AI agents when working in this repository. It defines conventions, structure, and commands so that generated code aligns with the project’s norms.

## Core Principles

1. Prefer Iteration Over Reinvention – Reuse existing patterns before creating new ones.
2. Keep It Simple and Maintainable – Prioritize clarity and readability.
3. Avoid Duplication – Check for existing implementations before adding new code.
4. Respect Existing Patterns – Don’t change working patterns without reason.
5. Environment Awareness – Respect dev/test/prod; never introduce test data into prod.

## Formatting & Style

- 2-space indentation, LF, UTF-8.
- JavaScript: semicolons, ES modules, `const`/`let`.
- Naming: `kebab-case` files, `camelCase` JS, `kebab-case` CSS.
- Max file length: \~600 lines.

## File & Folder Structure

```
/assets/
  css/
  js/
  img/
  icons/
/components/
/tests/
```

Componentize repeated UI into `/components`; each JS module handles one feature.

## HTML Conventions

- Semantic HTML (`header`, `nav`, `main`, `footer`).
- Bootstrap utilities/components first.
- No inline JS/CSS; use `<script type="module" defer>`.

## CSS Strategy

- Bootstrap utilities preferred.
- Minimal, scoped custom CSS; no `!important`.

## JavaScript Patterns

- One feature = one module.
- Use `data-*` hooks; event delegation.
- No globals, no `eval`; isolate DOM effects.

## Accessibility Targets

- WCAG 2.1 AA compliance.
- ≥4.5:1 contrast, visible focus, keyboard operable.

## Performance & Size

- Critical path <200 KB.
- Bootstrap via CDN; lazy-load non-critical JS; optimize images.

## Security Hygiene

- CSP-friendly; sanitize content.
- Minimal, pinned third-party scripts.

## Testing & Dependencies

- Unit + DOM-level tests.
- No jQuery; small, audited libs only.

## Change Scope & Iteration

- Focus only on relevant areas.
- If introducing a new pattern, remove the old in the same change.

## Local Server Lifecycle

- Stop running dev servers before starting new ones.
- Provide exact start/stop commands.

## Environment Handling

- Use config flags for env differences.
- Never modify `.env` without approval.
- No fake data outside tests.

## Codex-Specific Notes

- Codex reads AGENT.md before exploring the repo.
- These rules override default Codex behavior to ensure alignment with this project.
