# Task List Management

Guidelines for managing task lists in markdown files to track progress on completing a PRD for Vanilla JavaScript, HTML, and Bootstrap CSS projects

## Task Implementation

- **One sub-task at a time:** Do **NOT** start the next sub‑task until you ask the user for permission and they say "yes" or "y"
- **Completion protocol:**  
  1. When you finish a **sub‑task**, immediately mark it as completed by changing `[ ]` to `[x]`.
  2. If **all** subtasks underneath a parent task are now `[x]`, follow this sequence:
    - **First**: Run the test suite if configured:
      - For Jest: `npm test` or `npx jest`
      - For Mocha: `npm test` or `npx mocha`
      - For browser-based testing: Open test HTML files in browser
      - For no test framework: Manually verify functionality in browser
    - **Validate HTML**: Check HTML validation using W3C validator or browser dev tools
    - **Check Console**: Ensure no JavaScript errors in browser console
    - **Only if all tests/validations pass**: Stage changes (`git add .`)
    - **Clean up**: Remove any temporary files, console.log statements, and commented-out code before committing
    - **Commit**: Use a descriptive commit message that:
      - Uses conventional commit format (`feat:`, `fix:`, `refactor:`, `style:`, `docs:`, etc.)
      - Summarizes what was accomplished in the parent task
      - Lists key changes and additions
      - References the task number and PRD context
      - **Formats the message as a single-line command using `-m` flags**, e.g.:
        ```
        git commit -m "feat: add user profile form with Bootstrap components" -m "- Creates responsive form layout using Bootstrap grid" -m "- Implements client-side validation in vanilla JS" -m "- Adds custom CSS for non-Bootstrap styling" -m "Related to Task 2.0 in PRD"
        ```
        or
        ```
        git commit -m "fix: resolve modal dismiss event handling" -m "- Fixes Bootstrap modal event listeners" -m "- Prevents form submission on modal close" -m "Related to Task 3.2 in PRD"
        ```
  3. Once all the subtasks are marked completed and changes have been committed, mark the **parent task** as completed.
- Stop after each sub‑task and wait for the user's go‑ahead.

## Task List Maintenance

1. **Update the task list as you work:**
   - Mark tasks and subtasks as completed (`[x]`) per the protocol above.
   - Add new tasks as they emerge (e.g., discovered need for additional Bootstrap components or JavaScript utilities).

2. **Maintain the "Relevant Files" section:**
   - List every file created or modified.
   - Give each file a one‑line description of its purpose.
   - Include:
     - HTML files (pages, components, templates)
     - JavaScript files (modules, utilities, API handlers)
     - CSS files (custom styles beyond Bootstrap)
     - Test files (if applicable)
     - Configuration files (if modified)

## Browser Testing Checklist

Before marking tasks complete, verify:
- **Cross-browser compatibility**: Test in Chrome, Firefox, Safari, Edge
- **Responsive design**: Check all Bootstrap breakpoints (xs, sm, md, lg, xl, xxl)
- **JavaScript functionality**: Ensure all event handlers and DOM manipulations work
- **Bootstrap components**: Verify modals, dropdowns, tooltips, etc. function correctly
- **Accessibility**: Check keyboard navigation and ARIA attributes
- **Performance**: Monitor for memory leaks or performance issues in dev tools

## AI Instructions

When working with task lists, the AI must:

1. Regularly update the task list file after finishing any significant work.

2. Follow the completion protocol:
   - Mark each finished **sub‑task** `[x]`.
   - Mark the **parent task** `[x]` once **all** its subtasks are `[x]`.
   - Remember to validate HTML and check browser console before committing.

3. Add newly discovered tasks, especially when:
   - Additional Bootstrap components are needed
   - Browser compatibility issues are found
   - Custom JavaScript utilities are required
   - Responsive design adjustments are needed

4. Keep "Relevant Files" accurate and up to date, including:
   - All HTML pages modified
   - JavaScript modules created or updated
   - CSS files for custom styling
   - Any configuration or build files

5. Before starting work, check which sub‑task is next.

6. After implementing a sub‑task:
   - Test functionality in browser
   - Update the task list file
   - Pause for user approval before proceeding

## Technology-Specific Notes

- **HTML Changes**: Always validate HTML structure and ensure semantic markup
- **JavaScript**: Remove console.log statements and use appropriate error handling
- **Bootstrap**: Prefer Bootstrap utility classes over custom CSS when possible
- **Testing**: If no automated testing is set up, document manual testing steps performed
- **Browser Support**: Note any browser-specific workarounds or polyfills used
