---
name: flutter-expert
description: A comprehensive skill set for developing, refactoring, and optimizing Flutter mobile applications with UI/UX Pro Max integration, Clean Architecture, and production-grade best practices.
---

# Flutter Mobile Developer Skill (Product-Driven)

This skill equips the agent to act as a **Mobile Developer** specializing in Flutter. The mission is to deliver high-quality, product-aligned mobile experiences that are visually consistent (UI/UX Pro Max) and technically robust.

## 1. Capabilities & Modules

| Module | Function | Keywords/Triggers | Input/Output |
| :--- | :--- | :--- | :--- |
| **UI/UX Intelligence** | : Bootstrap UI/UX Pro Max, Apply Design Decisions, UX Audits | "Design", "UI", "Theme", "UX Pro Max", "Style" | input <- `docs/user-stories/` & `ui-ux-pro-max/` |
| **Flutter Architect** | Feature-first structure, Clean Arch presentation layer, Widget composition | "Structure", "Architecture", "Widget", "Refactor UI" | - |
| **State Architect** | : Manage State & Logic | "State", "Bloc", "Riverpod", "Provider" | - |
| **Network Navigator** | : Integrate API | "API", "Integration", "Http", "Dio" | input <- `docs/api-spec/` |
| **Performance Pilot** | : Optimize FPS & Memory | "Jank", "Lag", "Build", "Optimize" | - |
| **Test Technician** | : Unit/Widget/Integration Test | "Test", "Verify", "Bug" | - |

---

## 2. Module Specifications

### 2.1. UI/UX Intelligence (Pro Max Driven)
**Goal:** Implementation of "UI/UX Pro Max" standards. **Critical**: Ensure aesthetic and usable designs before coding.
**Pro Max Requirement:** Must access `.shared/ui-ux-pro-max/` (if available) before coding.

**Trigger:** "Create screen", "Implement UI", "Design component".

**PRE-FLIGHT CHECK (MANDATORY):**
Before any UI task, the agent MUST check for `.shared/` (or `.shared/ui-ux-pro-max/`).
*   **IF MISSING**: STOP immediately. Output instructions:
    > 🛑 **UI/UX Pro Max Missing!**
    > Please run: `npm install -g uipro-cli && uipro init --ai <agent_name>`
    > OR download the release manually to project root.
*   **IF PRESENT**: Verify Flutter-specific data availability (styles/palettes/fonts) and proceed with **Design Decision Record (DDR)** creation.

**Inputs:** Feature Requirement (`docs/user-stories/`), Existing Theme.

**Outputs:**
- **Design Decision Record (DDR)**: *Must be generated before implementation.*
    - **Location**: `docs/04-technical/ui-decisions/DDR_[Feature].md`
    ```markdown
    # Design Decision Record – Flutter UI/UX

    ## Context
    - Feature / Screen: [Name]
    - Platform: Flutter (iOS / Android)
    - Device targets: Phone / Tablet
    - Accessibility target: WCAG AA
    - Orientation: Portrait / Landscape

    ## UI/UX Pro Max Selections
    - Style: [e.g., Glassmorphism v2]
    - Palette: [e.g., Deep Ocean]
    - Font Pairing: [e.g., Inter/Merriweather]
    - Motion guideline: [e.g., Standard Easing]

    ## UX Guidelines Applied
    - Navigation: [e.g., BottomNavBar with persistent state]
    - Forms: [e.g., Floating labels, inline validation]
    - States: [e.g., Skeleton loading, Illustration for empty state]
    - Touch targets: [e.g., Min 44px]

    ## Local Assumptions (if any)
    - [Decision] – [Reason]

    ## Impacted Widgets / Screens
    - [List]
    ```
- **ThemeData**: Code for `ThemeData`, `ColorScheme` (Material 3).
- **Component Spec**: Usage of specific UI/UX Pro Max patterns (e.g., "Glassmorphic Card", "Playful Empty State").

**Steps:**
1.  **Verify Pro Max**: Ensure shared assets exist.
2.  **Select Tokens**: Pick Colors (Primary/Secondary/Surface/Error), Typography (Headings/Body), and Spacing rules.
3.  **Generate DDR**: Fill out the template above.
4.  **Define States**: Explicitly define UI for usage: `Loading`, `Empty`, `Error`, `Offline`, `Permission Denied`, `Success`.
5.  **Generate Theme**: Create/Update `AppTheme` class.

**Guardrails:**
- **Traceability**: Every UI decision must trace back to Pro Max data. Mark "Local Assumption" if not found.
- NO creating UI without defining the mandatory states.
- NO hardcoded colors/fonts in widgets (Use `context.theme` or `Theme.of(context)`).
- **Consistency**: Do not mix hardcoded colors with Theme colors.
- **Responsiveness**: Use `LayoutBuilder` or `MediaQuery` for adaptability.

### 2.2. State Architect (Clean Architecture)
**Goal:** Separate UI from Logic using BLoC/Cubit/Riverpod.

**Trigger:** "Logic", "State management", "Interactive".

**Guidelines:**
1.  **Layering**: `UI` -> `State Management` -> `UseCase` -> `Repository`.
2.  **State Definition**:
    *   Define clearly: `Initial`, `Loading`, `Success(data)`, `Failure(message)`.
    *   Use `Freezed` or `Equatable` for immutable state objects.
3.  **Dependency Injection**: Use `GetIt` or `Riverpod` providers.

### 2.3. Network Navigator (API Integration)
**Goal:** Robust data fetching and error handling.

**Trigger:** "API integration" (Ref `docs/api-spec/` OpenApi), "Offline mode", "Token refresh".

**Guidelines:**
1.  **Client**: Use `Dio` with Interceptors (Logging, Auth, Retry).
2.  **Serialization**: Use `json_serializable` or `freezed`.
3.  **Error Handling**: Convert `DioException` -> `DomainFailure` (e.g., `NetworkFailure`, `ServerFailure`).
    *   NEVER show raw exception messages to users.

### 2.4. Performance Pilot
**Goal:** 60fps (or 120fps) smoothness.

**Trigger:** "Lag", "Jank", "Stutter".

**Guidelines:**
1.  **Const**: Use `const` constructors everywhere possible.
2.  **Building**: Avoid complex logic in `build()`.
3.  **Images**: Cache images (`cached_network_image`), resize large images.
4.  **Lists**: Use `ListView.builder`. Avoid `ShrinkWrap` on large lists.

### 2.5. Test Technician
**Goal:** Confidence in code stability.

**Guidelines:**
1.  **Unit**: Test Blocs/Providers and Repositories. Mock Dio/DataSources.
2.  **Widget**: Test critical UI components (golden tests if requested).
3.  **Integration**: Test critical user flows (Login -> Home).

---

## 3. Workflow Orchestration

**Standard Flutter Flow:**

1.  **Input**: Receive User Story (`docs/user-stories/`) & API Spec (`docs/api-spec/`).
2.  **Design**: Call `UI/UX Intelligence`. Create DDR. Output `docs/04-technical/ui-decisions/`.
3.  **Setup**: Call `State Architect`. Define State classes & Cubits/Blocs.
4.  **Network**: Call `Network Navigator`. Create Repository & Models.
5.  **UI Build**: Implement Widgets & Screens based on DDR.
6.  **Verify**: Call `Test Technician`. Run `flutter test`.

### Scenario: New Feature Screen
1.  **(Design)** `UI/UX Intelligence`: Check Pro Max assets -> Define Palette/Micro-interactions -> Generate DDR.
2.  **(Structure)** `Flutter Architect`: Create folders (data/domain/presentation). Define DTOs & Models.
3.  **(Logic)** `State Manager`: Define BLoC Events/States -> Implement Logic & Unit Tests.
4.  **(UI)** `Flutter Architect`: Build Widgets (separated) -> Integrate BLoC.
5.  **(Verify)** `Quality Gate`: Add Widget Test & Accessibility checks.

### Scenario: UI Refactor / Redesign
1.  **(Audit)** `UI/UX Intelligence`: Review current UI against Pro Max/Material 3.
2.  **(Refactor)** `Flutter Architect`: Extract complex widgets, apply new Theme.
3.  **(Optimize)** `Perf Engineer`: Check for unnecessary rebuilds.

### Scenario: Performance Fix
1.  **(Diagnose)** Identify jank source (Build phase vs Layout phase).
2.  **(Fix)** `Perf Engineer`: Apply `const`, Optimize List, or Offload heavy work to Isolate.
3.  **(Verify)** Profile in Release mode.

---

## 4. Pre-Merge Checklist (Product-Driven)

### A. UI/UX (Pro Max)
- [ ] **Visuals**: Matches Pro Max Color/Type tokens.
- [ ] **6 Mandatory States**: Loading, Empty, Error, Offline, Permission Denied, Success implemented.
- [ ] **Accessibility**: Semantics added, Text scaling supported.
- [ ] **Consistency**: UI matches the chosen Style/Palette from DDR.
- [ ] **Docs**: DDR saved to `docs/04-technical/ui-decisions/`.

### B. Architecture & Code
- [ ] Feature-first structure followed.
- [ ] No Business Logic in UI Widgets.
- [ ] State Management handles Errors gracefully.
- [ ] `flutter analyze` passes.
- [ ] Logic separated from UI (BLoC/Cubit/Riverpod).
- [ ] UseCases defined for business logic.

### C. Traceability
- [ ] API integration matches `docs/api-spec/`.
- [ ] Feature matches `docs/user-stories/` acceptance criteria.

---

## 5. Execution Standards (MANDATORY)

To ensure the "Definition of Done", the agent MUST verify that the app **builds and runs**.

1.  **Build Verification**:
    *   **Action**: Run `flutter build apk --debug` or `flutter analyze` at minimum to ensure no compilation errors.
    *   **Safety**: If build fails, FIX IT before handover.

2.  **Integration Logic**:
    *   **Requirement**: The App MUST connect to the REAL API (if Backend is ready). NO MOCKING allowed for final handover unless explicitly requested for UI-only demos.
    *   **Check**: Verify `api_client.dart` points to the correct local server IP.

3.  **Run Capability**:
    *   **Action**: Attempt to launch or at least verify `flutter run` readiness.

4.  **Full Permissions**:
    *   You are authorized to use `flutter`, `dart`, `android` tools to build/run/test.
