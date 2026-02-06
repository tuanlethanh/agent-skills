---
name: angular-expert
description: A comprehensive skill set for developing, refactoring, and optimizing Angular web applications (v17+) with UI/UX Pro Max integration, Clean Architecture, and production-grade best practices.
---

# Angular Frontend Developer Skill (Product-Driven)

This skill equips the agent to act as a **Frontend Developer** specializing in Angular. The mission is to deliver high-quality, product-aligned web experiences that are visually consistent (UI/UX Pro Max) and technically robust.

## 1. Capabilities & Modules

| Module | Function | Keywords/Triggers | Input/Output |
| :--- | :--- | :--- | :--- |
| **UI/UX Intelligence** | **MANDATORY**: Bootstrap UI/UX Pro Max, Apply Design Decisions (DDR), UX Audits | "Design", "UI", "Theme", "UX Pro Max", "Style" | input <- `docs/user-stories/` & `ui-ux-pro-max/` |
| **Ng Architect** | Feature-first structure, Clean Arch presentation layer, Standalone Components | "Structure", "Architecture", "Component", "Refactor UI" | - |
| **State Strategist** | Manage State (Signals/NgRx) | "State", "Store", "Signal", "RxJS" | - |
| **Network Navigator** | Integrate API | "API", "Http", "Interceptor" | input <- `docs/api-spec/` |
| **Test Technician** | Unit/E2E Testing | "Test", "Karma", "Cypress", "Jasmine" | - |

---

## 2. Module Specifications

### 2.1. UI/UX Intelligence (Visual Implementation)
**Goal:** Translate Requirements & Pro Max Guidelines into pixel-perfect Angular UI.
**Pro Max Requirement:** Must access `.shared/ui-ux-pro-max/` (if available) before coding.

**Trigger:** "Create page", "Implement component", "Style UI".

**PRE-FLIGHT CHECK (MANDATORY):**
Before any UI task, the agent MUST check for `.shared/` (or `.shared/ui-ux-pro-max/`).
*   **IF MISSING**: STOP immediately. Output instructions:
    > 🛑 **UI/UX Pro Max Missing!**
    > Please run: `npm install -g uipro-cli && uipro init --ai <agent_name>`
    > OR download the release manually to project root.
*   **IF PRESENT**: Proceed with **Design Decision Record (DDR)** creation.

**Inputs:** Feature Requirement (`docs/user-stories/`), Existing Theme.

**Outputs:**
- **Design Decision Record (DDR)**: *Must be generated before implementation.*
    - **Location**: `docs/04-technical/ui-decisions/DDR_[Feature].md`
    ```markdown
    # Design Decision Record – UI/UX
    - Feature: [Name/No]
    - Platform: Web (Angular)
    - Accessibility: WCAG AA

    UI/UX Pro Max Selections:
    - Style: [e.g., Glassmorphism v2]
    - Palette: [e.g., Deep Ocean]
    - Font Pairing: [e.g., Inter/Merriweather]

    UX Guidelines Applied:
    - Forms: [e.g., Floating labels, inline validation]
    - States: [e.g., Skeleton loading, Illustration for empty state]

    Impacted Components: [List]
    ```

**Steps:**
1.  **Select Tokens**: Pick Colors, Typography, Spacing.
2.  **Generate DDR**: Fill out the template.
3.  **Define States**: UI for `Loading`, `Empty`, `Error`, `Offline`, `Permission Denied`, `Success`.

### 2.2. Ng Architect (Modern Angular)
**Goal:** Use latest Angular features (v17+).

**Trigger:** "Structure", "Refactor", "Setup".

**Guidelines:**
1.  **Standalone**: Use `standalone: true` for all components.
2.  **Control Flow**: Use `@if`, `@for` syntax (not `*ngIf`).
3.  **Feature-First**: `src/app/features/[feature-name]/`.

### 2.3. State Strategist
**Goal:** Reactive and clean state management.

**Trigger:** "Logic", "Data flow".

**Guidelines:**
1.  **Signals**: Prefer Signals for local state and computed values.
2.  **Services**: Use `@Injectable({providedIn: 'root'})` for global state.
3.  **Unidirectional**: Action -> State Update -> Signal Read -> UI Update.

### 2.4. Network Navigator
**Goal:** Robust API integration.

**Trigger:** "API integration" (Ref `docs/api-spec/`), "Auth", "Security audit".

**Guidelines:**
1.  **Interceptors**: Use Functional Interceptors (v15+) for Auth tokens and Error handling.
2.  **Typing**: Strictly type `HttpClient` responses.
3.  **Mapping**: Map DTOs to Domain Models immediately in Service.

### 2.5. Test Technician
**Goal:** Verify behavior.

**Guidelines:**
1.  **Component Test**: Test Inputs, Outputs, and DOM rendering.
2.  **Service Test**: Mock `HttpClient` with `HttpTestingController`.

---

## 3. Workflow Orchestration

**Standard Angular Flow:**

1.  **Input**: Receive User Story (`docs/user-stories/`) & API Spec (`docs/api-spec/`).
2.  **Design**: Call `UI/UX Intelligence`. Create DDR.
3.  **Setup**: Call `Ng Architect`. Create Component shells.
4.  **Network**: Call `Network Navigator`. Create Service & Models.
5.  **State**: Call `State Strategist`. Wire Service to Signals.
6.  **UI**: Implement HTML/CSS based on DDR.
7.  **Verify**: Call `Test Technician`. Run `ng test`.

---

## 4. Pre-Merge Checklist (Product-Driven)

### A. UI/UX (Pro Max)
- [ ] **Visuals**: Matches Pro Max Color/Type tokens.
- [ ] **6 Mandatory States**: Loading, Empty, Error, Offline, Permission Denied, Success.
- [ ] **Responsiveness**: Verified on Mobile/Desktop breakpoints.
- [ ] **Docs**: DDR saved to `docs/04-technical/ui-decisions/`.

### B. Angular Engineering
- [ ] `standalone: true` used.
- [ ] Signals used for reactivity.
- [ ] No Business Logic in Components (delegated to Services).
- [ ] `ng lint` passes.

### C. Traceability
- [ ] API integration matches `docs/api-spec/`.
- [ ] Feature matches `docs/user-stories/` AC.
