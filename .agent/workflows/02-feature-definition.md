---
description: Workflow 2 - Feature Definition (Epics) [MANDATORY AFTER FRAMING]
---

# WORKFLOW 2: FEATURE DEFINITION (EPICS)

This workflow ORCHESTRATES the definition of high-level features (Epics).
**Goal:** Break down the Vision/Scope into manageable Epics (large features) for the MVP.
**Prerequisite:** Workflow 1 (Product Framing) COMPLETED.
**Roles:** PO (Lead), BA (Support), SA (Review).

## Step 1: List Epics (PO)
The PO identifies the big chunks of work based on the MVP Scope.

1.  **Action**: Use `product-owner` skill -> `Feature Definer` module.
2.  **Prompt**:
    > "PO, based on the MVP Scope (Task Creation, Proof, Approval, Rewards), list the Epics.
    > Format: 'EPIC-[ID]: [Name] - [Goal]'."
3.  **Example**:
    *   EPIC-001: Task Management - Enable parents to create and assign chores.
    *   EPIC-002: Kid Dashboard - Allow kids to view tasks and submit proof.
    *   EPIC-003: Approval & Scoring - Parents review work and award points.

## Step 2: Detail Epics (PO + BA)
Flesh out the "Why" and "What" for each Epic.

1.  **Action**: Use `product-owner` skill -> `Feature Definer` module.
2.  **Prompt**:
    > "For each Epic, define:
    > - **Goal**: What value does it bring?
    > - **Key Features**: Bullets of what interacts?
    > - **Out of Scope**: What is explicitly NOT included?
    > - **Persona**: Who uses this?"
3.  **Output**: Create files in `docs/epics/EPIC-[ID].md`.

## Step 3: Feasibility & Risk (SA - MANDATORY)
Review the defined Epics for technical viability and complex constraints.

1.  **Action**: Use `solution-architect` skill -> `Product Validator`.
2.  **Prompt**:
    > "SA, review the summarized Epics. Do any carry high technical risk or violate the Vision's feasibility? Generate `docs/04-technical/evaluations/Feasibility_Epics.md`."
3.  **Check**:
    -   Are any external dependencies missing?
    -   Is the complexity aligned with MVP?

## Step 4: Prioritize (PO)
Rank Epics for development.

1.  **Action**: Use `product-owner` skill -> `Backlog Owner`.
2.  **Prompt**:
    > "Rank Epics 1 to N. Which one MUST be done first?"

## Step 5: The "Epic Gate"
**CRITICAL CHECK**:
- [ ] Are all MVP scopes covered by Epics?
- [ ] Are Epics detailed in `docs/epics/`?
- [ ] Has SA signed off on Technical Feasibility?
- [ ] Is there a clear priority?

**Outcome**:
- If **PASS**: Mark Workflow as **COMPLETED**.
    > "Epics defined. Proceed to Workflow 3: Requirements Breakdown (User Stories)."
- If **FAIL**: Blocked. Refine Epics based on SA feedback.
