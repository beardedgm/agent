# Rule: Generating a Task List from a PRD

## Goal

To guide an AI assistant in creating a detailed, step-by-step task list in Markdown format based on an existing Product Requirements Document (PRD). The task list should guide a developer through implementation using Vanilla JavaScript, HTML, and Bootstrap CSS.

## Output

- **Format:** Markdown (`.md`)
- **Location:** `/tasks/`
- **Filename:** `tasks-[prd-file-name].md` (e.g., `tasks-prd-user-profile-editing.md`)

## Process

1.  **Receive PRD Reference:** The user points the AI to a specific PRD file
2.  **Analyze PRD:** The AI reads and analyzes the functional requirements, user stories, and other sections of the specified PRD.
3.  **Assess Current State:** Review the existing codebase to understand existing infrastructure, architectural patterns and conventions. Also, identify any existing components or features that already exist and could be relevant to the PRD requirements. Then, identify existing related files, HTML pages, JavaScript modules, and utilities that can be leveraged or need modification.
4.  **Phase 1: Generate Parent Tasks:** Based on the PRD analysis and current state assessment, create the file and generate the main, high-level tasks required to implement the feature. Use your judgement on how many high-level tasks to use. It's likely to be about 3-7 parent tasks.
5. **Inform the user:** Present these tasks to the user in the specified format (without sub-tasks yet) For example, say "I have generated the high-level tasks based on the PRD. Ready to generate the sub-tasks? Respond with 'Go' to proceed."
6.  **Wait for Confirmation:** Pause and wait for the user to respond with "Go".
7.  **Phase 2: Generate Sub-Tasks:** Once the user confirms, break down each parent task into smaller, actionable sub-tasks necessary to complete the parent task. Ensure sub-tasks logically follow from the parent task, cover the implementation details implied by the PRD, and consider existing codebase patterns where relevant without being constrained by them.
8.  **Identify Relevant Files:** Based on the tasks and PRD, identify potential files that will need to be created or modified. List these under the `Relevant Files` section, including corresponding test files if applicable.
9.  **Generate Final Output:** Combine the parent tasks, sub-tasks, relevant files, and notes into the final Markdown structure.
10.  **Save Task List:** Save the generated document in the `/tasks/` directory with the filename `tasks-[prd-file-name].md`, where `[prd-file-name]` matches the base name of the input PRD file (e.g., if the input was `prd-user-profile-editing.md`, the output is `tasks-prd-user-profile-editing.md`).

## Output Format

The generated task list _must_ follow this structure:

```markdown
## Relevant Files

- `index.html` - Main HTML page for the feature (contains Bootstrap layout and structure).
- `pages/feature-name.html` - Dedicated HTML page for this feature if needed.
- `js/feature-name.js` - Main JavaScript module for feature functionality.
- `js/feature-name.test.js` - Unit tests for feature JavaScript module.
- `js/utils/helpers.js` - Utility functions needed for the feature.
- `js/utils/helpers.test.js` - Unit tests for utility functions.
- `css/custom-styles.css` - Custom CSS styles beyond Bootstrap defaults.
- `api/endpoints.js` - API integration functions if backend calls are needed.
- `api/endpoints.test.js` - Unit tests for API functions.

### Notes

- HTML files should leverage Bootstrap's grid system and components for responsive layouts.
- JavaScript files should use ES6+ module syntax where appropriate (import/export).
- Unit tests can be run using a test runner like Jest or Mocha (specify which is used in the project).
- Use `npm test` or `npx jest [optional/path/to/test/file]` to run tests if Jest is configured.
- Custom CSS should be minimal, preferring Bootstrap utility classes where possible.
- Consider browser compatibility requirements for JavaScript features used.

## Tasks

- [ ] 1.0 Set up HTML structure and Bootstrap layout
  - [ ] 1.1 [Create base HTML page with Bootstrap CDN links]
  - [ ] 1.2 [Define responsive grid layout using Bootstrap classes]
  - [ ] 1.3 [Add Bootstrap components as per design requirements]
- [ ] 2.0 Implement JavaScript functionality
  - [ ] 2.1 [Create main JavaScript module structure]
  - [ ] 2.2 [Implement DOM manipulation and event listeners]
  - [ ] 2.3 [Add form validation if applicable]
- [ ] 3.0 Integrate API endpoints (if backend interaction required)
  - [ ] 3.1 [Create API service functions]
  - [ ] 3.2 [Implement error handling for API calls]
- [ ] 4.0 Apply custom styling and responsive design
  - [ ] 4.1 [Add custom CSS for non-Bootstrap styling needs]
  - [ ] 4.2 [Ensure mobile responsiveness]
- [ ] 5.0 Write unit tests
  - [ ] 5.1 [Write tests for JavaScript functions]
  - [ ] 5.2 [Write tests for API integration if applicable]
```

## Technology Stack Considerations

When generating tasks for Vanilla JavaScript/HTML/Bootstrap projects:

- **HTML Structure:** Tasks should include creating semantic HTML5 markup with proper Bootstrap classes for layout and components.
- **JavaScript Organization:** Consider whether to use module pattern, ES6 modules, or simple script files based on project conventions.
- **Bootstrap Integration:** Leverage Bootstrap's built-in components (modals, dropdowns, forms, etc.) rather than building custom solutions.
- **Event Handling:** Use vanilla JavaScript event listeners and delegation patterns instead of framework-specific approaches.
- **State Management:** Consider how to manage application state using vanilla JavaScript (localStorage, sessionStorage, or in-memory objects).
- **Testing Approach:** Specify whether to use browser-based testing, Jest for unit tests, or other testing frameworks appropriate for vanilla JavaScript.

## Interaction Model

The process explicitly requires a pause after generating parent tasks to get user confirmation ("Go") before proceeding to generate the detailed sub-tasks. This ensures the high-level plan aligns with user expectations before diving into details.

## Target Audience

Assume the primary reader of the task list is a **junior developer** who will implement the feature with awareness of the existing codebase context, HTML/CSS/JavaScript fundamentals, and Bootstrap framework knowledge.
