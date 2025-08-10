# AGENT.md — Condensed AI Rules (HTML • Bootstrap 5 • Vanilla JS)

**Scope:** Only modify `/assets`, `/components`, `/tests`. Never change `/public` or configs without instruction.  
**Stack:** HTML5, Bootstrap 5 (utilities/components first), Vanilla JS (ES modules).  

## Core Rules
- Reuse existing patterns; avoid duplication.
- Small, focused changes only.
- Prioritize simplicity/readability over cleverness.
- Keep files ≤300 lines; split into modules if larger.
- No inline JS/CSS; CSP-friendly.
- Respect env separation; no `.env` edits without approval.

## Style
- Indent 2 spaces, UTF-8 LF.
- JS: semicolons, `const`/`let` (no `var`), ES modules.
- Names: kebab-case (files/classes), camelCase (JS vars).
- Max line length ~100–120 chars.

## HTML
- Use semantic tags; label all inputs.
- Scripts: `<script type="module" src="..." defer></script>`.

## CSS / Bootstrap
- Order: 1) Utilities, 2) Components, 3) Scoped custom CSS.
- Avoid deep selectors/`!important`.
- Use CSS vars for theme tokens.

## JavaScript
- One feature = one module; export named funcs.
- Use `data-*` for DOM hooks; prefer event delegation.
- Null-check; no globals, `eval`, or Function().
- Keep logic pure; isolate DOM effects.
- Use `AbortController` when cancelable.

## Accessibility (WCAG 2.1 AA)
- Contrast ≥ 4.5:1; visible focus outlines.
- Full keyboard support.
- Provide `alt`; use `aria-*` only when needed.
- `aria-live` for async updates.

## Performance
- Critical path < 200 KB.
- Use CDN Bootstrap; lazy-load non-critical JS.
- Optimize/resize images; prefer modern formats.

## Security
- CSP-friendly; sanitize user content.
- Minimize third-party scripts; pin versions.

## Dependencies
- Avoid jQuery/heavy libs; prefer small, audited deps.
- Document new deps (purpose, size, license).

## Testing
- Unit tests for utilities; DOM tests for key flows.
- Tests own mocks; no test code in app files.

## Workflow
- PRs: single-purpose, small, with before/after notes.
- All tests must pass pre-merge.

## Server Lifecycle
- Stop any running dev server before starting new.
- Include start/stop commands in PR when relevant.

## Checklist (per change)
[ ] Reused pattern.  
[ ] Focused edits.  
[ ] Style/naming rules followed.  
[ ] Tests added/updated.  
[ ] Accessibility/security preserved.  
[ ] Performance impact minimal.  
[ ] Run/test instructions provided.  

**Self-Check:** ✅ if met, ❌ if not. ≤1 ❌ allowed per PR.
