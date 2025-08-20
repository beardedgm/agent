# Development Workflow - Master Index

This repository follows a structured AI-assisted development workflow for building features with HTML5, Bootstrap 5, Vanilla JavaScript, and client-side storage (localStorage, IndexedDB, SQLite).

## Workflow Overview

```mermaid
graph LR
    A[Feature Request] --> B[PRD Generation]
    B --> C[Task List Generation]
    C --> D[Implementation]
    D --> E[Task Management]
    E --> F[Testing & Commit]
    F --> G[Feature Complete]
```

## Process Documents

### 📋 1. [PRD Generation](./prd-generation.md)
**When:** At the start of any new feature  
**Purpose:** Convert feature ideas into detailed requirements  
**Process:**
1. User provides feature description
2. AI asks clarifying questions
3. AI generates structured PRD
4. Saves as `prd-[feature-name].md`

### 📝 2. [Task List Generation](./task-generation.md)
**When:** After PRD is approved  
**Purpose:** Break down PRD into actionable tasks  
**Process:**
1. AI analyzes PRD and codebase
2. Generates parent tasks
3. User confirms with "Go"
4. AI generates detailed sub-tasks
5. Saves as `tasks-prd-[feature-name].md`

### ✅ 3. [Task Management](./task-management.md)
**When:** During implementation  
**Purpose:** Track progress and maintain code quality  
**Process:**
1. Complete one sub-task at a time
2. Mark tasks as complete `[x]`
3. Test and commit when parent task done
4. Follow conventional commit format

### 🔧 4. [AGENT.md](./AGENT.md)
**When:** Throughout all coding  
**Purpose:** Coding standards and technical guidelines  
**Covers:**
- Code style and organization
- Storage strategy (localStorage, IndexedDB, SQLite)
- Bootstrap patterns
- Security and performance
- Testing requirements

## Quick Start for AI Assistants

```markdown
1. **For New Features:**
   - Start with PRD Generation
   - Ask clarifying questions
   - Generate PRD → Task List → Begin Implementation

2. **For Existing Features:**
   - Check `/tasks/` for PRD and task list
   - Follow Task Management protocol
   - Reference AGENT.md for coding standards

3. **For Bug Fixes:**
   - Reference AGENT.md for standards
   - Create minimal task list if needed
   - Follow commit conventions
```

## Technology Stack

- **Frontend:** HTML5, Bootstrap 5, Vanilla JavaScript (ES6+)
- **Storage Hierarchy:**
  - `localStorage`: Settings & preferences (<10MB)
  - `IndexedDB`: Complex objects & offline data
  - `SQLite`: Relational data & SQL queries
  - `sessionStorage`: Temporary session data
- **Testing:** Jest/Mocha for unit tests
- **No Backend by Default:** Client-side first approach

## File Structure

```
/project-root
├── /tasks
│   ├── prd-[feature].md           # Product Requirements
│   └── tasks-prd-[feature].md     # Task Lists
├── /js
│   ├── /storage                   # Storage managers
│   ├── /utils                     # Utilities
│   ├── /components                # UI components
│   └── /features                  # Feature modules
├── /css
│   └── /custom                    # Custom styles
├── /tests
│   ├── /unit                      # Unit tests
│   └── /integration               # Integration tests
└── /docs
    ├── WORKFLOW.md                # This file
    ├── prd-generation.md
    ├── task-generation.md
    ├── task-management.md
    └── AGENT.md

```

## Key Principles

1. **One sub-task at a time** - Never jump ahead
2. **Test before commit** - Always run tests
3. **Offline-first** - Design for offline capability
4. **Progressive enhancement** - HTML first, enhance with JS
5. **Modular code** - Small, focused modules
6. **Clear documentation** - Comment non-obvious logic

## Common Commands

```bash
# Development
npm start          # Start dev server
npm test           # Run all tests
npm run lint       # Check code style

# Git Workflow
git add .
git commit -m "type: description" -m "- Detail 1" -m "- Detail 2"
git push

# Storage Management (in browser console)
localStorage.clear()           # Clear localStorage
indexedDB.deleteDatabase('db') # Clear IndexedDB
```

## When Things Go Wrong

- **Storage quota exceeded:** Implement cleanup in storage manager
- **Offline sync conflicts:** Use timestamp-based resolution
- **Browser compatibility:** Check caniuse.com, add polyfills
- **Performance issues:** Profile with DevTools, implement lazy loading

## Additional Resources

- [Bootstrap 5 Docs](https://getbootstrap.com/docs/5.3/)
- [MDN Web Docs](https://developer.mozilla.org/)
- [IndexedDB Guide](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [sql.js Documentation](https://sql.js.org/)

---

**Remember:** Follow the workflow sequentially. PRD → Tasks → Implementation → Testing → Commit. When in doubt, refer to the appropriate process document above.