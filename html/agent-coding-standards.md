# AGENT.md — Project Contribution & Coding Standards (HTML5 • Bootstrap 5 • Vanilla JS • Client-Side Storage)

This AGENT.md provides clear instructions for AI assistants (OpenAI, Claude, etc.) when working in this repository. It defines conventions, structure, and commands so that generated code aligns with the project's norms and follows the PRD → Task List → Implementation workflow.

## Technology Stack
- **Frontend:** HTML5, Bootstrap 5 (always favor utilities and pre-built components), vanilla JavaScript organized as ES modules
- **Data Storage Hierarchy:**
  - **localStorage:** Small preference values, settings, UI state (<10MB)
  - **IndexedDB:** Structured data, complex objects, offline caching, large datasets
  - **SQLite (via sql.js):** Relational data requiring SQL queries, complex reporting
  - **sessionStorage:** Temporary session-specific data
- **No Backend by Default:** Avoid adding servers or remote storage unless specifically directed in the PRD

## Workflow Integration
- **PRD First:** Always refer to the PRD document in `/tasks/prd-[feature-name].md` before implementing
- **Task Management:** Follow the task list in `/tasks/tasks-prd-[feature-name].md`
- **One Sub-task at a Time:** Complete and mark each sub-task before proceeding to the next
- **Commit Protocol:** After completing all sub-tasks under a parent task, run tests and commit with conventional format

## Core Principles
- **Reuse Patterns:** Adhere to existing component and utility patterns; avoid reinventing solutions
- **Keep Changes Focused:** One feature or bug fix per pull request. Do not bundle unrelated changes
- **Simplicity Over Cleverness:** Prioritize readable, straightforward code over obscure optimizations
- **Modular Files:** Aim for files ≤ 300 lines. If a module grows larger, split it into smaller ES modules organized by feature
- **No Inline Scripts or Styles:** Keep JavaScript in .js modules and CSS in stylesheets; follow Content-Security-Policy best practices
- **Progressive Enhancement:** Build features that work without JavaScript first, then enhance with JavaScript
- **Offline-First:** Design with offline capability in mind from the start

## Code Style

### General
- **Indentation:** Use 2-space indents, UTF-8 encoding with LF line endings
- **File Organization:**
  ```
  /js
    /storage      # Storage managers
    /utils        # Utility functions
    /components   # Reusable UI components
    /features     # Feature-specific modules
  /css
    /custom       # Custom styles
  /tests
    /unit         # Unit tests
    /integration  # Integration tests
  ```

### JavaScript
- **Variables:** Use `const`/`let` (avoid `var`), semicolons required
- **Modules:** ES module syntax (`import`/`export`), prefer named exports
- **Naming:** 
  - Files: `kebab-case.js`
  - Variables/Functions: `camelCase`
  - Classes: `PascalCase`
  - Constants: `UPPER_SNAKE_CASE`
- **Comments:** JSDoc for exported functions, clear comments for non-obvious logic

### HTML
- **Semantic Tags:** Use `<header>`, `<main>`, `<section>`, `<article>`, `<nav>`, `<footer>`
- **Accessibility:** Label all form controls (`<label for="...">`), provide ARIA attributes where needed
- **Script Loading:** Use `<script type="module" src="..."></script>` at end of body
- **Data Attributes:** Use `data-*` attributes for JavaScript hooks, not classes or IDs

### CSS / Bootstrap
- **Order:** 
  1. Bootstrap utilities (e.g., `class="mb-3 text-center"`)
  2. Bootstrap components (e.g., `class="btn btn-primary"`)
  3. Custom styles (scoped with BEM or component prefixes)
- **Custom Properties:** Define theme tokens as CSS variables
- **Avoid:** Deep selectors, `!important`, inline styles
- **Responsive:** Mobile-first approach using Bootstrap breakpoints

## JavaScript Patterns

### Module Structure
```javascript
// feature-name.js
import { storageManager } from './storage/storage-manager.js';
import { validateInput } from './utils/validation.js';

// Private state
let state = {
  // feature state
};

// Private functions
function processData(data) {
  // implementation
}

// Public API
export function initFeature(config) {
  // initialization
}

export function updateFeature(data) {
  // update logic
}
```

### Storage Management
```javascript
// Storage abstraction layer
class StorageManager {
  async savePreference(key, value) {
    try {
      localStorage.setItem(key, JSON.stringify(value));
    } catch (e) {
      console.warn('localStorage full, falling back to sessionStorage');
      sessionStorage.setItem(key, JSON.stringify(value));
    }
  }
  
  async saveComplexData(storeName, data) {
    // Use IndexedDB for complex objects
    return await this.indexedDB.put(storeName, data);
  }
  
  async queryData(sql, params) {
    // Use SQLite for complex queries
    return await this.sqlite.exec(sql, params);
  }
}
```

### Event Handling
```javascript
// Event delegation pattern
document.querySelector('.container').addEventListener('click', (e) => {
  if (e.target.matches('[data-action="save"]')) {
    handleSave(e.target.dataset);
  } else if (e.target.matches('[data-action="delete"]')) {
    handleDelete(e.target.dataset);
  }
});
```

### Error Handling
```javascript
// Consistent error handling
async function fetchData() {
  try {
    const data = await loadFromIndexedDB();
    return data;
  } catch (error) {
    console.error('Failed to load data:', error);
    // Fallback to cached or default data
    return getCachedData() || getDefaultData();
  }
}
```

## Storage Guidelines

### When to Use Each Storage Type
- **localStorage:**
  - User preferences (theme, language, layout)
  - Small configuration (<5MB)
  - Data that needs synchronous access
  
- **sessionStorage:**
  - Form drafts
  - Temporary UI state
  - Navigation history within session
  
- **IndexedDB:**
  - Large datasets (>5MB)
  - Complex objects with relationships
  - Binary data (images, files)
  - Offline cache
  
- **SQLite (sql.js):**
  - Relational data with foreign keys
  - Complex queries with JOINs
  - Reporting and analytics
  - Data requiring transactions

### Storage Error Handling
```javascript
// Always check storage availability
function isStorageAvailable(type) {
  try {
    const storage = window[type];
    const test = '__storage_test__';
    storage.setItem(test, test);
    storage.removeItem(test);
    return true;
  } catch {
    return false;
  }
}

// Handle quota exceeded
function handleQuotaExceeded() {
  // Clean up old data
  // Notify user
  // Fall back to alternative storage
}
```

## Accessibility (WCAG 2.1 AA)
- **Color Contrast:** Minimum 4.5:1 for normal text, 3:1 for large text
- **Focus Indicators:** Visible focus outlines on all interactive elements
- **Keyboard Support:** All functionality accessible via keyboard
- **Screen Readers:** Proper ARIA labels and live regions for dynamic content
- **Skip Links:** Provide skip-to-content links for keyboard users

## Performance
- **Bundle Size:** Keep initial JavaScript < 200KB
- **Lazy Loading:** Defer non-critical modules with dynamic imports
- **Debouncing:** Throttle expensive operations (storage writes, API calls)
- **Caching Strategy:**
  - Static assets: Long cache with versioning
  - API responses: Cache in IndexedDB with TTL
  - User data: Sync to storage periodically

## Security
- **CSP Compliance:** No inline scripts, no `eval()`
- **Input Validation:** Sanitize all user input before storage or display
- **XSS Prevention:** Use `textContent` instead of `innerHTML`
- **Storage Encryption:** Encrypt sensitive data before storing
- **HTTPS Only:** Ensure all resources loaded over HTTPS

## Testing Requirements
- **Unit Tests:** Test pure functions and utilities
- **Integration Tests:** Test storage operations and DOM interactions
- **Storage Mocks:** Mock localStorage, IndexedDB, and SQLite in tests
- **Browser Testing:** Test in Chrome, Firefox, Safari, Edge
- **Accessibility Testing:** Run axe-core or similar tools

## Checklist Before Submission

### Code Quality
- [ ] Followed naming conventions and code style
- [ ] No console.log statements in production code
- [ ] Added JSDoc comments for exported functions
- [ ] Files are under 300 lines

### Storage
- [ ] Used appropriate storage type for data
- [ ] Implemented error handling for storage operations
- [ ] Added fallback mechanisms for storage failures
- [ ] Cleaned up test data from storage

### Testing
- [ ] Added/updated unit tests
- [ ] Tested in multiple browsers
- [ ] Verified offline functionality
- [ ] Checked accessibility with keyboard navigation

### Performance
- [ ] Minimized bundle size
- [ ] Implemented lazy loading where appropriate
- [ ] Debounced expensive operations
- [ ] Tested with slow network conditions

### Documentation
- [ ] Updated relevant documentation
- [ ] Added inline comments for complex logic
- [ ] Provided clear commit messages
- [ ] Updated task list with completed items

## Git Commit Format
```bash
git commit -m "type: brief description" \
  -m "- Detailed change 1" \
  -m "- Detailed change 2" \
  -m "Related to Task X.X in PRD"
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

## Common Patterns Library

### Bootstrap Modal with Form
```javascript
// Modal management pattern
const modal = new bootstrap.Modal(document.getElementById('myModal'));
const form = document.getElementById('myForm');

form.addEventListener('submit', async (e) => {
  e.preventDefault();
  const formData = new FormData(form);
  await saveToStorage(formData);
  modal.hide();
});
```

### Responsive Table with Pagination
```javascript
// Table pagination pattern
class TablePagination {
  constructor(tableId, itemsPerPage = 10) {
    this.table = document.getElementById(tableId);
    this.itemsPerPage = itemsPerPage;
    this.currentPage = 1;
  }
  // Implementation
}
```

### Offline Sync Pattern
```javascript
// Queue changes when offline
class OfflineQueue {
  async addToQueue(action) {
    const queue = await this.getQueue();
    queue.push({ ...action, timestamp: Date.now() });
    await this.saveQueue(queue);
  }
  
  async processQueue() {
    if (!navigator.onLine) return;
    const queue = await this.getQueue();
    // Process each queued action
  }
}
```

## Final Notes
- Always refer to PRD for requirements
- Follow task list sequentially
- Test thoroughly before marking tasks complete
- Ask for clarification when requirements are ambiguous
- Consider offline-first and mobile-first approaches
- Optimize for performance and accessibility from the start