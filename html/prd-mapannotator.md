# Product Requirements Document (PRD): MapAnnotator

**Version:** 1.1
**Date:** 2025-08-19
**Owner:** TBD

---

## 1. Introduction / Overview

**MapAnnotator** is a lightweight, client-side web application for adding labeled annotations to static map images (JPG/PNG). The application runs entirely in the browser using plain HTML, CSS, and vanilla JavaScript (no build step or backend).

This PRD specifies an upgrade that enables richer label metadata: users can choose an icon, set label text and style (font size and color), and add longer-form notes (tooltip content). Tooltip content appears when hovering over a label for ~1 second and does not appear in exported PNGs; it is preserved when saving/loading projects (JSON).

**Primary problem solved:** Allow users to attach contextual information to map labels without complicating export flows or requiring a server.

**Primary users:** Broad general audience needing a flexible, offline-friendly map annotation tool.

---

## 2. Goals

1. Allow users to place labels via click/tap and immediately configure them in a modal/sheet (icon, text, font size, color, tooltip notes).
2. Show tooltip notes on hover after a 1000 ms delay on desktop; provide an equivalent tap-to-toggle tooltip on touch devices; hide on mouse out or tap outside.
3. Ensure font size changes proportionally scale the chosen icon (icon sits to the left of the label text).
4. Maintain existing features: drag labels, edit/delete labels, zoom controls, keyboard shortcuts, export to PNG, save/load JSON, clear/reset.
5. Preserve all label metadata (including icon and tooltip) in JSON save/load.
6. Keep the experience fully client-side, performant, and accessible across modern browsers.
7. Provide mobile/touch ergonomics (gesture zoom/pan, larger touch targets, bottom sheet editor) and ensure parity with desktop.
8. Support export scaling options (native, scale factor, or target pixel width) while keeping text/icon rendering crisp.

---

## 3. User Stories

- As a user, I can load a JPG/PNG map image and zoom/pan/resize for comfortable labeling.
- As a user, I can click on the map to place a new label and configure it via a modal (icon, text, font size, color, tooltip notes).
- As a user, I can hover over a label for ~1 second to see its tooltip (up to 500 characters) and move the cursor away to hide it.
- As a user, I can drag labels to reposition them and click a label to re-open the modal to edit icon/text/font size/color/tooltip or delete it.
- As a user, I can export the annotated map as a PNG (icons and label text included; tooltips excluded).
- As a user, I can save my project (map + label metadata + zoom level) to JSON and load it later (including tooltips and icon selections).
- As a user, I can use keyboard shortcuts to speed up common actions (Escape, Delete, Enter; Alt/⌘ + mouse wheel for zoom; on-screen zoom buttons).

---

## 4. Functional Requirements

### 4.1 Map Loading
1. The system must allow the user to load a local JPG or PNG file.
2. The system must display the loaded image as the map background.
3. The system must maintain aspect ratio and allow zoom in/out and reset.
4. The system must store current zoom level in project state for save/load.

**Acceptance Criteria:**
- Loading a valid JPG or PNG displays the image without distortion.  
- Zoom in/out by buttons and Alt/⌘ + mouse wheel works; reset returns to 100% (1.0).  
- Zoom range: 0.25× to 4×; default 1.0; step 0.1 (or equivalent).  
- Zoom level persists in JSON and restores on load.

### 4.2 Label Creation (Modal)
5. On map click, a modal must open to configure a new label before it appears finalized on the map.
6. Modal fields must include:  
   - Label Text (single-line input).  
   - Font Size (numeric input; min 8px, max 96px; default 16px).  
   - Label Color (color input; default #222222).  
   - Icon Selector (a fixed set of 12 Bootstrap 5 Icons; see §6.1).  
   - Tooltip Notes (textarea; max 500 chars; recommended 400–500 chars).  
7. A preview of the label (icon + text) must update live as the user changes settings.
8. **Icon scaling:** Icon must scale proportionally with the font size so that changing font size adjusts icon size equally.
9. The user must be able to Confirm (Create) or Cancel. Cancel discards the label.
10. Validation:  
    - Tooltip length must be enforced (hard limit 500 chars; show counter).  
    - Label Text may be empty (allowed), but show a subtle warning.  
    - Font Size must be within allowed range; non-numeric resets to default.  

**Acceptance Criteria:**
- Clicking on the map opens the modal focused on Label Text.  
- Selecting any of the 12 icons updates the preview immediately.  
- Increasing font size increases icon size proportionally.  
- Submitting with Tooltip > 500 chars blocks submission with an error and character counter.  
- Canceling closes modal without creating a label.

### 4.3 Label Placement & Editing
11. When the modal is confirmed, the label must be placed at the clicked coordinates (adjusted for current zoom).  
12. Labels must be draggable to reposition; position updates must persist.  
13. Clicking an existing label must re-open the modal with current values for editing.  
14. The modal must provide a Delete action for removing the label.  
15. Label selection state must be visible (e.g., focus ring or subtle highlight).

**Acceptance Criteria:**
- Drag-and-drop is smooth at 60fps on typical hardware for maps up to ~8MP.  
- Editing updates are visible immediately.  
- Deleting removes the label and any associated tooltip.

### 4.4 Tooltip Behavior
16. Hovering over a label must display its tooltip after a 1000 ms delay.  
17. Tooltip must hide when the pointer leaves the label/tooltip area.  
18. Tooltip width should be constrained (e.g., 280–360px) and wrap text.  
19. Tooltip must be positioned intelligently to avoid clipping off-screen where possible.  
20. Tooltips must not be included in PNG export (see 4.6).

**Acceptance Criteria:**
- Hover delay is 1000 ± 100 ms.  
- Tooltip hides on mouseout within 150 ms unless re-entering the tooltip.  
- Tooltip supports up to 500 chars; longer input is prevented at save time.  
- Tooltip z-index ensures visibility above map/labels.

### 4.5 Keyboard Shortcuts
21. Escape deselects the active label or closes the modal if open.  
22. Delete removes the selected label (with confirm dialog).  
23. Enter retains focus in Label Text input when modal is open.  
24. Alt/⌘ + mouse wheel zooms in/out.  
25. Toolbar buttons provide zoom in/out and reset.

**Acceptance Criteria:**
- Shortcuts operate consistently across Chrome, Firefox, and Safari.  
- Keyboard interactions do not cause page scroll when zooming.

### 4.6 Export & Persistence
26. Export Annotated Map to PNG must include map image, icons, and label text as rendered (no tooltips).  
27. Save Project to JSON must include: map reference, all labels (text, color, font size, iconId, tooltip), positions, and zoom level.  
28. Load Project from JSON must restore the map and all labels with correct rendering, tooltip behavior, and zoom state.  
29. Clear Map must remove the map and all labels. Remove All Labels must keep the map but delete labels.

**Acceptance Criteria:**
- PNG export image matches on-screen label text/icon placement at 1× current zoom (exported at map’s native resolution; labels scaled appropriately).  
- JSON round-trip (save → load) preserves all fields without loss.  
- Clear and Remove All Labels ask for confirmation.


### 4.7 Mobile / Touch Behavior
30. Touch input must be first-class: single tap to place a label (opens modal as a bottom sheet on small screens), tap an existing label to edit, drag to move.
31. Pinch to zoom (smooth, centered around midpoint of pinch) and two-finger drag to pan; provide on-screen buttons for zoom in/out/reset as alternatives.
32. Tooltips on touch must be accessible: tapping a label reveals the tooltip as a popover; tapping outside dismisses it.
33. Hit targets for draggable labels and controls must be ≥ 40×40 px on touch devices.
34. The modal on small screens must present as a bottom sheet with full-width layout and in-view preview; the page behind should be inert (no scroll) while open.
35. Performance targets on mobile mirror desktop: smooth dragging at typical device frame rates; avoid layout thrash by using transforms.

**Acceptance Criteria:**
- Pinch zoom and two-finger pan work on iOS Safari and Android Chrome.
- Bottom sheet modal traps focus and is keyboard-accessible (external keyboards) and screen-reader-friendly.
- Tapping a label shows its tooltip; a second tap outside hides it.
- All interactive elements meet minimum touch target size (≥ 40 px) and have visible focus/active states.

### 4.8 Export Scaling Options
36. Export dialog must allow users to choose export size:
   - **Native (1×)**: export at the map's intrinsic pixel dimensions.
   - **Scale Factor**: 0.5×–4.0× in 0.25 increments (default 1×).
   - **Target Width**: user enters width in pixels (e.g., 1920); height is computed to preserve aspect ratio; clamp to a practical max (e.g., 10000 px).
37. Export must render to an offscreen canvas at the selected output size, redrawing map, icons (SVG), and text to avoid raster blurring.
38. Labels (text + icons) must scale proportionally with the export size; icon stroke details must remain crisp.
39. Tooltips must never appear in exported output.
40. The export dialog must remember the last selection for the session.

**Acceptance Criteria:**
- Visual parity: a 2× export is visibly sharper than 1× (no interpolation blur of labels/icons).
- Output PNG dimensions match user choice within ±1 px.
- Large exports (e.g., 8000 px width) succeed without crashing; if memory limits are hit, the app shows a clear error and suggests a smaller size.
---

## 5. Non-Goals (Out of Scope for v1)

- Real-time multi-user collaboration or sharing links.  
- Advanced image editing (freehand drawing, shapes, layers).  
- External basemap providers/APIs (e.g., Google Maps, Leaflet, map tiles).  
- Custom font family uploads; only system/default fonts are supported.
- Custom icon uploads (fixed set of 12 Bootstrap 5 Icons in v1.1).  
- Uploading custom icons; v1 ships with a fixed set of 12 Bootstrap 5 Icons.  
- Server-side storage or authentication (tool remains fully client-side).

---

## 6. Design Considerations (Optional)

### 6.1 Icon Set
- Provide a curated set of **12 Bootstrap 5 Icons** (SVG) packaged locally for offline use; each mapped to an `iconId` string, e.g.:  
  `pin`, `flag`, `star`, `info`, `house`, `treasure`, `skull`, `castle`, `tree`, `mountain`, `water`, `cave`.  
- Icons render to the **left** of label text with fixed gap (e.g., 6–8px).  
- Icon size scales with font size (e.g., icon height ~= font-size). Maintain crispness via SVG viewBox and `vector-effect="non-scaling-stroke"` as needed.

### 6.2 Modal UX
- Layout: two columns on desktop (left: fields; right: live preview). Single-column on small screens.  
- Inputs:  
  - Text input (label).  
  - Number input (font size) with step 1 and min/max validation.  
  - Color input (label color).  
  - Icon grid (12 items) with keyboard navigation and selected state.  
  - Tooltip textarea with char counter (0/500).  
- Buttons: **Create**, **Cancel**, **Delete** (Delete only when editing).  
- Focus management: return focus to the label after closing the modal.

### 6.3 Tooltip UX
- Delay: 1000 ms after stable hover.  
- Dismissal: mouseout or pressing Escape.  
- Appearance: readable contrast, slight shadow, arrow pointer to label, maximum width 320px, scroll if content exceeds vertical space.  
- Positioning strategy: prefer above, then below, then side, avoiding viewport clipping.

### 6.4 Accessibility
- All controls keyboard-operable; modal traps focus and respects `aria-modal="true"`.  
- Tooltips use `aria-describedby` for screen readers when feasible (note: hover tooltips may need alternative access via “inspect label” action).  
- Contrast ratio ≥ 4.5:1 for text; support high-contrast mode.  
- Provide visible focus outlines for interactive elements.

### 6.5 Zoom & Coordinate System
- Maintain a stable world coordinate system (store label positions in image coordinate space; apply transforms for view rendering).  
- On export, render at image native resolution using stored coordinates; avoid cumulative rounding errors.

---

## 7. Technical Considerations (Optional)

- **Stack:** Plain HTML, CSS, and vanilla JS. No build step.  
- **Rendering approach:**  
  - UI (toolbar, modal, selection) via DOM.  
  - Map display via `<img>` or `<canvas>`; labels as absolutely positioned DOM elements over a transformable container. For PNG export, render to an offscreen `<canvas>` by drawing the map image, then drawing icons and text at computed positions/sizes.  
- **Icon packaging:** Embed 12 SVGs locally (no CDN) to ensure offline availability and consistent licensing.  
- **Performance:**  
  - Target 60fps during dragging/zooming for images up to ~8MP and ≤ 200 labels.  
  - Debounce hover detection and throttle pointermove to avoid jank.  
- **Persistence format:** JSON schema (see §8).  
- **Browser support:** Latest stable Chrome, Firefox, Safari (desktop). Edge is expected to work via Chromium.  
- **Error handling:** Graceful messages for invalid files, oversized images, or corrupted JSON.  
- **Security:** No remote network calls; only local file APIs.  
- **Testing:** Manual test plan plus lightweight unit tests for coordinate math and serialization.

---

## 8. Data Requirements

### 8.1 JSON Project Schema (v1)
```json
{
  "version": 1,
  "map": {
    "name": "my-map.png",
    "dataUrl": "data:image/png;base64,..."
  },
  "view": {
    "zoom": 1.0
  },
  "labels": [
    {
      "id": "uuid-1",
      "x": 123.45,                 // image-space X (px)
      "y": 678.90,                 // image-space Y (px)
      "text": "Harbor District",
      "color": "#222222",
      "fontSize": 16,
      "iconId": "pin",             // one of the 12 Bootstrap 5 icons
      "tooltip": "Fisher guild HQ; busy mornings.",
      "createdAt": "2025-08-19T00:00:00Z",
      "updatedAt": "2025-08-19T00:00:00Z"
    }
  ]
}
```

**Notes:**  
- `dataUrl` enables offline persistence; optionally store a relative file hint.  
- Positions stored in image coordinates; transform applied for display/export.  
- `tooltip` string hard-limited to 500 characters at save time.

---

## 9. Success Metrics

- ≥ 95% of attempted exports succeed without visual defects (informal QA across sample projects).  
- JSON round-trip fidelity = 100% for icon, text, color, font size, tooltip, and positions.  
- Hover tooltips appear within 1.0–1.1 seconds on supported browsers.  
- Interactive performance: median drag frame time ≤ 16ms with 100 labels on a 4K image.  
- Zero blocking network requests in typical flows (fully offline).


---

## 10.5 Suggested Enhancements / Roadmap (Optional)
These are not required for v1.1 but are strong candidates for a near-term backlog:

- **Undo/Redo**: Multi-step history for position and property changes.
- **Snap & Guides**: Optional grid, smart alignment lines when moving labels; snap-to-pixel toggle for crisp exports.
- **Grouping & Multi-Select**: Select multiple labels to move or edit shared properties.
- **Label Backgrounds**: Optional rounded background with padding and opacity for readability on busy maps.
- **Legend Generator**: Auto-generate a legend (icon → label list) as a separate PNG or printable panel.
- **Search / Jump-to Label**: Filter labels by text, quickly center on selection.
- **Minimap Navigator**: Small overview map for quick panning at high zoom levels.
- **Import from CSV/JSON**: Bulk-create labels from structured data (x,y,text,icon,tooltip).
- **Template Presets**: Save and reuse style presets (font size/color + icon).
- **Localization**: Add i18n scaffolding for UI strings; start with English.
- **Theming**: Light/dark modes; respect OS preference via prefers-color-scheme.
- **Auto-numbering**: Option to add incremental numeric badges before label text.
- **Accessibility Enhancements**: Non-hover tooltip access on desktop via keyboard (e.g., focus + key), high-contrast icon pack.
- **Quality Improvements**: Debounced autosave to localStorage; project file schema versioning/migrations; integrity checks when loading JSON.
---

## 10. Open Questions

1. Confirm **icon list** (exact 12 Bootstrap 5 icons) and final names.  
2. Confirm **no custom icon uploads** in v1 (fixed set only).  
3. Should labels support **bold/italic** or font family choices in v1?  
4. Minimum mobile support? (This PRD assumes desktop-first; mobile/touch could be addressed in a follow-up.)  
5. Export scaling: keep export at **native map resolution** or allow user-selectable DPI/scaling?  
6. Do we want a **legend export** (mapping icons to text) in a future version?

---

## 11. Appendix: Existing Features (Unchanged from current app)

- Drag labels to reposition.  
- Click label to edit or delete.  
- Keyboard shortcuts: Escape (deselect/close modal), Delete (remove selected label), Enter (keep focus in label input).  
- Zoom controls: Alt/⌘ + mouse wheel; on-screen zoom in/out/reset buttons.  
- Export map as PNG (labels included, tooltips excluded).  
- Save project (map, labels, zoom) to JSON and load later.  
- Clear map or remove all labels with a single click.  
- Entirely client-side: open `index.html` in a modern browser to use.
