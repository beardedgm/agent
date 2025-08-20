# Rule: Generating a Product Requirements Document (PRD)

## Goal

To guide an AI assistant in creating a detailed Product Requirements Document (PRD) in Markdown format for features built with Vanilla JavaScript, HTML, Bootstrap CSS, and client-side storage solutions (localStorage, IndexedDB, SQLite). The PRD should be clear, actionable, and suitable for a junior developer to understand and implement the feature.

## Process

1.  **Receive Initial Prompt:** The user provides a brief description or request for a new feature or functionality.
2.  **Ask Clarifying Questions:** Before writing the PRD, the AI *must* ask clarifying questions to gather sufficient detail. The goal is to understand the "what" and "why" of the feature, not necessarily the "how" (which the developer will figure out). Make sure to provide options in letter/number lists so I can respond easily with my selections.
3.  **Generate PRD:** Based on the initial prompt and the user's answers to the clarifying questions, generate a PRD using the structure outlined below.
4.  **Save PRD:** Save the generated document as `prd-[feature-name].md` inside the `/tasks` directory.

## Clarifying Questions (Examples)

The AI should adapt its questions based on the prompt, but here are some common areas to explore for web applications:

### General Questions
*   **Problem/Goal:** "What problem does this feature solve for the user?" or "What is the main goal we want to achieve with this feature?"
*   **Target User:** "Who is the primary user of this feature?"
*   **Core Functionality:** "Can you describe the key actions a user should be able to perform with this feature?"
*   **User Stories:** "Could you provide a few user stories? (e.g., As a [type of user], I want to [perform an action] so that [benefit].)"
*   **Acceptance Criteria:** "How will we know when this feature is successfully implemented? What are the key success criteria?"
*   **Scope/Boundaries:** "Are there any specific things this feature *should not* do (non-goals)?"

### Web-Specific Questions
*   **Page Structure:** "Will this be a new page, part of an existing page, or a reusable component?"
*   **Bootstrap Components:** "Which Bootstrap components would be appropriate? (e.g., a. Modal, b. Cards, c. Forms, d. Tables, e. Navbar, f. Accordion, g. Other)"
*   **Responsive Design:** "What are the responsive requirements? Should it work on: a. Mobile (xs/sm), b. Tablet (md/lg), c. Desktop (xl/xxl), d. All devices"
*   **Browser Support:** "Which browsers must be supported? a. Modern browsers only, b. Include IE11, c. Specific browser versions"
*   **JavaScript Interactions:** "What user interactions are needed? (e.g., a. Form validation, b. Dynamic content loading, c. Drag and drop, d. Real-time updates, e. Animations)"

### Data Storage Questions
*   **Storage Requirements:** "What type of data needs to be stored? a. User preferences/settings, b. Form drafts, c. Large datasets, d. Structured relational data, e. Files/media"
*   **Storage Solution:** "Which storage solution fits best? a. localStorage (simple key-value, <10MB), b. IndexedDB (complex objects, large data), c. SQLite (relational queries), d. Combination of multiple"
*   **Data Persistence:** "How long should data persist? a. Current session only (sessionStorage), b. Until manually cleared, c. With expiration dates, d. Permanent storage"
*   **Offline Capability:** "Should the feature work offline? a. Full offline functionality, b. Read-only offline, c. Queue changes for sync, d. Online-only"
*   **Data Sync:** "If offline capable, how should data sync? a. Automatic background sync, b. Manual sync button, c. Sync on reconnection, d. Conflict resolution needed"
*   **Storage Limits:** "Expected data size? a. < 5MB (localStorage suitable), b. 5-50MB (IndexedDB recommended), c. > 50MB (IndexedDB required), d. Unknown/variable"
*   **Data Security:** "Are there sensitive data considerations? a. Encrypt before storing, b. No sensitive data, c. Clear on logout, d. Compliance requirements (GDPR, etc.)"
*   **Accessibility:** "Are there specific accessibility requirements? a. WCAG 2.1 AA compliance, b. Keyboard navigation, c. Screen reader support, d. Basic accessibility"

### Design/UI Questions
*   **Visual Style:** "Should this follow: a. Standard Bootstrap theme, b. Custom Bootstrap theme, c. Additional custom CSS needed"
*   **Layout:** "What type of layout is preferred? a. Single column, b. Two-column sidebar, c. Grid layout, d. Hero section with content below"
*   **Color Scheme:** "Use: a. Default Bootstrap colors, b. Custom brand colors (please specify), c. Dark mode support needed"
*   **Forms:** "If forms are involved, what validation is needed? a. Client-side only, b. Server-side with client feedback, c. Real-time validation as user types"

### Technical Questions
*   **API Integration:** "Will this feature need to: a. Call REST APIs, b. Submit forms to backend, c. Work offline-only, d. Handle file uploads"
*   **Performance:** "Are there performance considerations? a. Large datasets to display, b. Heavy DOM manipulation, c. Frequent updates, d. Standard performance is fine"
*   **Error Handling:** "How should errors be displayed? a. Bootstrap alerts, b. Toast notifications, c. Modal dialogs, d. Inline error messages"

## PRD Structure

The generated PRD should include the following sections:

1.  **Introduction/Overview:** Briefly describe the feature and the problem it solves. State the goal.
2.  **Goals:** List the specific, measurable objectives for this feature.
3.  **User Stories:** Detail the user narratives describing feature usage and benefits.
4.  **Functional Requirements:** List the specific functionalities the feature must have. Use clear, concise language (e.g., "The system must allow users to upload a profile picture."). Number these requirements.
5.  **Non-Goals (Out of Scope):** Clearly state what this feature will *not* include to manage scope.
6.  **Design Considerations:** 
    - Specify Bootstrap components to be used
    - Describe responsive breakpoints and behavior
    - Link to mockups if available
    - List any custom CSS requirements beyond Bootstrap
    - Specify color scheme and typography preferences
7.  **Technical Considerations:** 
    - **HTML Structure:** Semantic HTML5 requirements
    - **JavaScript Approach:** Module pattern, ES6 features to use, event handling strategy
    - **Bootstrap Version:** Specify Bootstrap version (e.g., Bootstrap 5.3)
    - **Browser Compatibility:** List supported browsers
    - **Performance:** Any specific performance targets
    - **Dependencies:** List any additional libraries needed beyond Bootstrap
    - **Data Storage Architecture:**
      - **localStorage:** For user preferences, settings, small data (<10MB)
      - **sessionStorage:** For temporary session data
      - **IndexedDB:** For complex objects, large datasets, offline caching
      - **SQLite (via sql.js):** For relational data and complex queries
      - **Storage Abstraction Layer:** Whether to create unified storage API
    - **API Integration:** Describe any backend endpoints needed
    - **Offline Strategy:** Approach for offline functionality and data sync
    - **Data Migration:** Strategy for updating storage schemas
    - **Testing Approach:** Unit testing framework if applicable
8.  **Success Metrics:** How will the success of this feature be measured? (e.g., "Increase user engagement by 10%", "Reduce page load time to under 2 seconds", "Achieve 100% mobile responsiveness").
9.  **Accessibility Requirements:** 
    - WCAG compliance level
    - Keyboard navigation requirements
    - ARIA labels and roles needed
    - Screen reader compatibility
10. **Open Questions:** List any remaining questions or areas needing further clarification.

## Example Technical Specifications Section

```markdown
### Technical Specifications

**Frontend Stack:**
- HTML5 (Semantic markup)
- CSS3 with Bootstrap 5.3
- Vanilla JavaScript (ES6+)

**Data Storage:**
- localStorage: User preferences and settings (theme, language)
- IndexedDB: Product catalog, shopping cart, order history
- SQLite (via sql.js): Complex reporting queries and analytics
- sessionStorage: Temporary form data and navigation state

**Key Bootstrap Components:**
- Responsive grid system (container, row, col-*)
- Form components with validation states
- Modal for confirmations
- Alert components for notifications

**JavaScript Requirements:**
- Use event delegation for dynamic content
- Implement form validation before submission
- Handle API responses with proper error states
- Use async/await for asynchronous operations
- Implement storage fallback chain: localStorage → IndexedDB → memory

**Storage Schema Example:**
```javascript
// IndexedDB Structure
const dbSchema = {
  name: 'AppDatabase',
  version: 1,
  stores: {
    users: { keyPath: 'id', indexes: ['email', 'createdAt'] },
    products: { keyPath: 'sku', indexes: ['category', 'price'] },
    orders: { keyPath: 'orderId', indexes: ['userId', 'status', 'date'] }
  }
};

// localStorage Keys
const storageKeys = {
  USER_PREFS: 'app_user_preferences',
  CACHE_VERSION: 'app_cache_version',
  LAST_SYNC: 'app_last_sync_timestamp'
};
```

**Browser Support:**
- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)

**Offline Capabilities:**
- Cache static assets using browser cache
- Store application data in IndexedDB
- Queue API calls for retry when online
- Display cached data with "offline" indicator

## Target Audience

Assume the primary reader of the PRD is a **junior developer** familiar with HTML, CSS, JavaScript fundamentals, Bootstrap framework, and basic understanding of browser storage APIs (localStorage, IndexedDB). Therefore, requirements should be explicit, unambiguous, and avoid advanced architectural patterns unless necessary. Provide enough detail for them to understand the feature's purpose, data storage requirements, and implementation approach using vanilla web technologies.

## Output

*   **Format:** Markdown (`.md`)
*   **Location:** `/tasks/`
*   **Filename:** `prd-[feature-name].md`

## Final Instructions

1. Do NOT start implementing the PRD
2. Make sure to ask the user clarifying questions appropriate for web development with HTML/JS/Bootstrap and storage
3. Take the user's answers to the clarifying questions and improve the PRD
4. Include specific Bootstrap components and utilities that would be helpful
5. Consider responsive design and accessibility from the start
6. Specify any custom JavaScript utilities that might need to be created
7. Define clear data storage strategies and schemas
8. Plan for offline functionality if appropriate
9. Consider data migration and versioning strategies for storage schemas
