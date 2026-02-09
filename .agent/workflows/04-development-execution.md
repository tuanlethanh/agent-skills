---
description: Workflow 4 - Development Execution (Backend & Mobile) [MANDATORY AFTER REQUIREMENTS]
---

# WORKFLOW 4: DEVELOPMENT EXECUTION

This workflow ORCHESTRATES the technical implementation of specific Epics/Features, ensuring alignment with the Solution Architecture.
**Goal**: Build, Test, and Deliver working software based on the Docs/Specs.
**Prerequisite**: Workflow 3 (Requirements Breakdown) COMPLETED for the target Epic.
**Roles**: Backend Lead, Mobile Lead, SA (Gatekeeper), QA.

## Step 1: Database Implementation (DB Architect + Backend)
Create the foundation.

1.  **Action**: Use `database-architect` skill -> `Schema Builder`.
2.  **Prompt**:
    > "Generate Flyway/Liquibase migration scripts for EPIC-[ID] based on `docs/data-model/`."
3.  **Validation**: Run migration locally to verify syntax.

## Step 2: Backend API Implementation (Spring/Laravel - BackEnd Dev)
Build the logic and endpoints.

1.  **Action**: Use `spring-boot-expert` (or `laravel-expert`) -> `Architect Core`.
2.  **Prompt**:
    > "Implement API for EPIC-[ID] based on `docs/api-spec/` and `docs/user-stories/`.
    > Follow Clean Architecture. Create Entities, Repository, Service, Controller."
3.  **Check**: Run Unit Tests (`Test Engineer` module).
4.  **Signal**: "API Implementation Ready."

## Step 3: API & Screen Mapping Audit (Solution Architect - MANDATORY)
Perform a detailed audit to verify if the Backend matches product needs. **NO FRONTEND WORK UNTIL THIS PASSES.**

1.  **Action Check**: Use `solution-architect` skill -> `Gap Analyzer`.
2.  **Prompt**:
    > "SA, audit the API implementation against User Stories. Identify Missing Fields, Extra Fields, or Misaligned Types. Generate `docs/04-technical/audits/Gap_Analysis_[Epic].md`."
3.  **Action Map**: Use `solution-architect` skill -> `Mapping Master`.
    > "Generate `docs/04-technical/audits/Screen_API_Map_[Epic].md` needed for Frontend. Detail every screen's API calls."
4.  **Outcome**:
    *   **PASS**: Proceed with Map.
    *   **FAIL**: Return to Step 2 (Backend Fix).

## Step 4: Frontend UI Implementation (Flutter/Angular - Frontend Dev)
Build the user interface using the Mapping Doc.

1.  **Pre-Flight Identity Check**: `flutter-expert` -> `UI/UX Intelligence`.
    > "Create DDR (Design Decision Record) for EPIC-[ID] features."
2.  **Action**: Use `flutter-expert` (or `angular-expert`) -> `Flutter Architect`.
    > "Implement Screens for EPIC-[ID] based on DDR and the `Screen_API_Map_[Epic].md` from SA. Integrate with API."
3.  **Check**: Run Widget Tests.

## Step 5: Integration & Verification (QA)
Ensure pieces work together based on US scenarios.

1.  **Action**: Manual or Automated Integration Test.
    > "Verify E2E flows: Register Parent -> Add Kid -> Login Kid."

## Step 6: The "Feature Gate"
**CRITICAL CHECK**:
- [ ] Code compiles and passes tests?
- [ ] Features match Acceptance Criteria?
- [ ] API Gap Analysis is CLEAN?
- [ ] Screen Mapping document exists and was followed?
- [ ] UI matches Pro Max guidelines (DDR)?
- [ ] No PII leaks?

**Outcome**:
- If **PASS**: Mark Epic as **DEV-COMPLETE**.
    > "Development finished. Ready for QA/Release."
