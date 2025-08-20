---
title: Delivery Workflow (Canonical)
status: canonical
version: 1.0
updated: 2025-08-19
links:
  coding_standards: ./AGENT.md
  prd_reference: ./01-prd-generation-guide.md
  task_reference: ./02-task-generation-guide.md
  task_mgmt_reference: ./03-task-management-guide.md
---

# Delivery Workflow (Canonical)

This document is the canonical, end‑to‑end workflow for producing features with a doc‑first approach: **PRD → Task List → Execution**. It inlines the essentials from the PRD, Task Generation, and Task Management guides. For detailed coding rules, see **[Agent Coding Standards](./AGENT.md)**.

## Table of Contents
- [Principles](#principles)
- [Stack & Constraints](#stack--constraints)
- [File & Naming Conventions](#file--naming-conventions)
- [Workflow Overview](#workflow-overview)
- [1. PRD Generation](#1-prd-generation)
- [2. Task List Generation](#2-task-list-generation)
- [3. Task Execution & Management](#3-task-execution--management)
- [4. Definition of Done](#4-definition-of-done)
- [Appendix A — Storage Ladder](#appendix-a--storage-ladder)
- [Appendix B — Accessibility & Performance](#appendix-b--accessibility--performance)
- [References](#references)

---

## Principles
- **Document‑driven**: Write before you build. PRD first, then tasks, then code.
- **Small, reversible steps**: Work **one sub‑task at a time**; commit early and often.
- **Human‑readable outputs**: Everything is Markdown or plain text unless otherwise specified.
- **Safety first**: Follow the Agent Coding Standards for HTML/JS/CSS, security, and testing.
- **No surprises**: Explicit gates. Pause after parent tasks and wait for an explicit **“Go”** before expanding sub‑tasks.

## Stack & Constraints
- **Front‑end only** by default: **HTML5 + Bootstrap 5 + vanilla ES modules**.
- **No backend** unless the PRD explicitly calls for it.
- **Content Security Policy‑friendly** JavaScript (no inline scripts, no `eval`).
- **Storage ladder** (prefer earlier rungs): `localStorage → IndexedDB → SQLite (sql.js) → sessionStorage`.
- **Design**: Bootstrap‑first styling; semantic, accessible HTML.

## File & Naming Conventions
- PRDs live in `/tasks/` and are named: `prd-<feature-slug>.md`.
- Task lists live in `/tasks/` and are named: `tasks-<prd-file-name>.md`.
- Place artifacts (mockups, JSON, schemas) next to the PRD that birthed them.

---

## Workflow Overview
1) **Write PRD** → 2) **Draft Task List** (parent tasks only) → **Pause for “Go”** → 3) **Expand sub‑tasks** → 4) **Execute one sub‑task at a time** → 5) **QA & commit** → 6) **Repeat**.

---

## 1. PRD Generation

**Goal:** Capture the problem, boundaries, and success criteria so tasks are obvious.

### Required PRD Sections
- **Title & Summary** — one‑paragraph description of the feature.
- **User & Problem** — who benefits; what pain is solved.
- **Scope** — in/out of scope; explicit non‑goals.
- **Data & Storage** — what we store; which rung of the storage ladder; retention.
- **UX Notes** — key flows, states, and accessibility must‑haves.
- **Risks & Assumptions** — technical constraints, edge cases.
- **Acceptance Criteria** — bullet list; measurable.
- **Appendices** — sketches, links, samples.

### Clarifying Questions (ask/answer before finalizing)
- Target users and primary use case?
- Must‑have vs. nice‑to‑have?
- Storage choice and data model sketch?
- Offline behavior and sync expectations?
- A11y requirements (keyboard, color contrast, ARIA)?
- Performance budgets (load time, bundle size)?
- Browser/device support matrix?

### PRD Output
- File: `tasks/prd-<feature-slug>.md`
- Keep it brief (≈1–2 pages). Link to artifacts rather than inlining large assets.

---

## 2. Task List Generation

**Goal:** Transform the PRD into actionable work with a clear gate.

### Structure of `tasks-<prd-file>.md`
- **Context** — 2–3 lines pulling the essentials from the PRD.
- **Relevant Files** — existing/new paths affected.
- **Notes** — implementation hints, known pitfalls.
- **Parent Tasks** — numbered, end‑to‑end slices; **stop here** and request **“Go”**.
- **Sub‑tasks** *(added only after “Go”)* — break each parent task down into stepwise increments suitable for single commits.

### Rules
- Parent tasks should be shippable increments or major checkpoints.
- Sub‑tasks must be independently testable and reviewable.

---

## 3. Task Execution & Management

**Golden rule:** Work **one sub‑task at a time**.

### Update Protocol
- At the start of a sub‑task, restate scope and success criteria.
- Keep a running **Update** section in the task list; note decisions and deviations.
- If scope shifts, amend the PRD or add a **Change Log** entry.

### Validation & QA (per sub‑task)
- **Console clean** (no errors/warnings).
- **Functional checks** for the changed surface area.
- **A11y checks**: keyboard nav, focus order, labels, color contrast.
- **Performance sanity**: no unnecessary reflows; assets sized appropriately.
- **Cross‑browser/device**: at least Chrome/Edge/Firefox + one mobile viewport.
- **Storage behavior**: read/write/clear paths tested according to chosen rung.

### Commit Protocol (Conventional Commits)
- Format: `<type>(scope): brief summary`
- Common types: `feat`, `fix`, `docs`, `refactor`, `chore`, `test`.
- Example: `feat(tasks): add parent tasks and gate for prd-notes-panel`.

---

## 4. Definition of Done
A sub‑task (or parent task) is **Done** when:
- All acceptance criteria for that unit pass.
- QA checklist above is clean.
- Changes are committed and linked back to the task list.
- Documentation updated (README, PRD, or comments where applicable).

---

## Appendix A — Storage Ladder
Choose the simplest viable rung:
1. **localStorage** — small key/value, instant, persistent; no large objects or PII.
2. **IndexedDB** — structured data, async; use for collections and searches.
3. **SQLite (sql.js)** — client‑side SQL when schemas/joins matter.
4. **sessionStorage** — short‑lived session state only.

Document chosen rung and schema in the PRD.

## Appendix B — Accessibility & Performance
- **Accessibility**: semantic HTML, proper labels, roles only when needed, visible focus, ARIA thoughtfully used, motion‑reduced fallbacks.
- **Performance**: avoid layout thrash; defer non‑critical JS; media sized for viewport; no blocking fonts; limit third‑party code.

---

## References
- Detailed rules and patterns live in **[AGENT.md](./AGENT.md)**.
- Concise references:
  - **[PRD Reference](./01-prd-generation-guide.md)**
  - **[Task List Reference](./02-task-generation-guide.md)**
  - **[Task Management Reference](./03-task-management-guide.md)**
