# AGENT.md — Project Contribution & Coding Standards (HTML5 • Bootstrap 5 • Vanilla JS)

This AGENT.md provides clear instructions for OpenAI Codex and similar AI agents when working in this repository. It defines conventions, structure, and commands so that generated code aligns with the project's norms.

## Scope

- **Technology stack**: the project uses HTML5, Bootstrap 5 (always favour utilities and pre‑built components), and vanilla JavaScript organised as ES modules.
- **Data storage**: prefer client‑side storage for state and persistence. Use IndexedDB for structured data (e.g. maps, images, uploads, saved states) and localStorage for small preference values. Avoid adding a server or remote storage unless specifically directed.

## Core Principles

- **Reuse patterns**: adhere to existing component and utility patterns; avoid reinventing solutions.
- **Keep changes focused**: one feature or bug fix per pull request. Do not bundle unrelated changes.
- **Simplicity over cleverness**: prioritise readable, straightforward code over obscure optimisations.
- **Modular files**: aim for files ≤ 300 lines. If a module grows larger, split it into smaller ES modules organised by feature.
- **No inline scripts or styles**: keep JavaScript in `.js` modules and CSS in stylesheets; follow Content‑Security‑Policy best practices.
- **Environment boundaries**: respect environment separation. Never modify `.env` or other secrets without approval.

## Code Style

```text
- Indentation: use 2‑space indents, UTF‑8 encoding with LF line endings.
- JavaScript: use const/let (avoid var), semicolons, and ES module syntax (import/export). Prefer named exports.
- Naming: file and CSS class names in kebab‑case; JavaScript variables and functions in camelCase; classes in PascalCase.
- Comments: write clear comments for non‑obvious logic and document exported functions.
```

## HTML

```html
<!-- Use semantic tags to structure content -->
<header>
  <main>
    <section>
      <form>
        <!-- Label all form controls -->
        <label for="username">Username</label>
        <input id="username" type="text" />
      </form>
    </section>
  </main>
</header>
```

- Keep markup free of inline JavaScript. Scripts should be linked with:

```html
<script type="module" src="script.js"></script>
```

at the end of the body for progressive rendering.

## CSS / Bootstrap

```css
/* Order CSS rules as follows: */
/* 1) Bootstrap utilities */
.d-flex.justify-content-center

/* 2) Bootstrap components */
.btn.btn-primary

/* 3) Scoped custom styles */
.custom-component {
  --theme-color: #007bff;
  color: var(--theme-color);
}
```

- Avoid deep selectors and `!important`; favour composable classes and CSS variables.
- Define theme tokens (colours, spacing) as CSS custom properties and reference them consistently.

## JavaScript

```javascript
// One feature per module: group related functions together and export them
// Example: grid.js, tokens.js, storage.js

// Event delegation example
document.addEventListener('click', (event) => {
  if (event.target.dataset.action === 'toggle') {
    // Handle toggle action
  }
});

// Avoid global state - encapsulate in modules
const GameState = {
  currentPlayer: null,
  gameBoard: [],
  // methods...
};

// Null-checks and error handling
try {
  const data = await fetchData();
  if (data?.property) {
    // Safe access
  }
} catch (error) {
  console.error('Operation failed:', error);
}

// AbortController for cleanup
const controller = new AbortController();
fetch('/api/data', { signal: controller.signal })
  .then(response => response.json())
  .catch(error => {
    if (error.name === 'AbortError') {
      console.log('Request cancelled');
    }
  });

// Cleanup
controller.abort();
```

**Key JavaScript Principles:**
- **Pure functions**: separate pure logic from DOM‑manipulation code whenever possible. Pure functions are easier to test.
- **AbortController**: for operations that may need cancellation (e.g. fetches, timers), use AbortController to clean up listeners and avoid memory leaks.
- **Persistence**: store complex objects (maps, tokens) in IndexedDB via a helper module; store small user preferences (e.g. UI settings, defaults) in localStorage. Provide asynchronous APIs that return promises.

## Accessibility (WCAG 2.1 AA)

```html
<!-- Maintain minimum colour contrast ratio of 4.5:1 -->
<!-- Provide visible focus outlines -->
.focusable:focus {
  outline: 2px solid #007bff;
  outline-offset: 2px;
}

<!-- Use meaningful alt attributes -->
<img src="map.jpg" alt="Dungeon map showing three rooms connected by corridors" />

<!-- Use aria-live for dynamic updates -->
<div aria-live="polite" id="status-updates"></div>
```

**Accessibility Requirements:**
- Ensure full keyboard support; all interactions (revealing/hiding fog, placing tokens, navigating modals) should be possible via keyboard shortcuts and focusable controls.
- Use `aria-*` attributes only when semantic elements cannot suffice.
- Use `aria-live` regions for dynamic updates so assistive technologies can announce changes.

## Performance

```javascript
// Debounce expensive operations
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

// Use requestAnimationFrame for repeated rendering
function animate() {
  // Update canvas or DOM
  requestAnimationFrame(animate);
}
```

**Performance Guidelines:**
- Keep critical path resources (HTML, CSS, JS) under 200 KB total to minimise initial load times.
- Use Bootstrap from a CDN; defer or lazy‑load non‑critical JavaScript modules.
- Optimise and resize images; prefer modern formats like WebP.

## Security

```javascript
// Content-Security-Policy compliance
// ❌ Avoid inline handlers
// <button onclick="handleClick()">

// ✅ Use event listeners
button.addEventListener('click', handleClick);

// ❌ Avoid innerHTML with user content
// element.innerHTML = userInput;

// ✅ Use textContent for safety
element.textContent = userInput;

// Validate user input
function sanitizeInput(input) {
  return input.replace(/[<>'"]/g, '');
}
```

**Security Requirements:**
- Follow Content‑Security‑Policy guidelines. Avoid inline event handlers and `eval()`/`Function()`.
- Sanitize and validate all user‑supplied data (including imported JSON).
- Minimise third‑party scripts; pin dependency versions and audit for vulnerabilities.

## Dependencies

```json
// package.json example
{
  "dependencies": {
    "bootstrap": "^5.3.0"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
```

- Avoid heavy libraries such as jQuery; favour lightweight, well‑maintained packages.
- Document new dependencies (purpose, size, license) in the README.

## Testing

```javascript
// Unit test example
import { calculateDistance } from './utils.js';

describe('calculateDistance', () => {
  test('should calculate correct distance between two points', () => {
    expect(calculateDistance([0, 0], [3, 4])).toBe(5);
  });
});

// DOM test example
test('should toggle fog when cell is clicked', () => {
  const cell = document.createElement('div');
  cell.dataset.x = '5';
  cell.dataset.y = '5';
  
  cell.click();
  
  expect(cell.classList.contains('revealed')).toBe(true);
});
```

**Testing Guidelines:**
- Write unit tests for utility functions and logic modules. Use mocks for Canvas, IndexedDB and localStorage.
- Write DOM/interaction tests for key flows (fog revealing, token placement, undo/redo, import/export).
- Keep tests in `/tests`, separate from application code. Do not include test helpers in production modules.

## Workflow

```bash
# Development commands
npm start          # Start dev server
npm test           # Run tests
npm run lint       # Check code style
npm run build      # Build for production
```

**Pull Request Guidelines:**
- Each PR should address a single issue or feature.
- Include a brief description of what changed, why, and how to test it.
- **CI requirements**: all tests must pass before merging. Run `npm test` and lint scripts locally before pushing.
- Provide instructions to start or stop any dev server in the PR description if relevant.

## Server Lifecycle

```bash
# ❌ Don't run multiple servers
npm start &
npm run dev &  # This will cause conflicts

# ✅ Stop existing server first
# Press Ctrl+C to stop current server
npm start  # Then start new one
```

- Do not run multiple dev servers concurrently.
- Use a single `npm start` (or documented script) for local development.
- Stop it (`Ctrl+C`) before running another command.

## Checklist (per change)

Before submitting your PR, verify the following:

```markdown
- [ ] Reused existing patterns; no unnecessary duplication
- [ ] Changes are focused and contained within allowed directories
- [ ] Style and naming rules are followed
- [ ] IndexedDB and localStorage are used appropriately for persistence
- [ ] Unit/interaction tests are added or updated as needed
- [ ] Accessibility and security requirements are met
- [ ] Performance impact has been considered and minimised
- [ ] Run and test instructions are provided
```
