---
description: Workflow 3 - Requirements Breakdown (User Stories & Specs) [MANDATORY AFTER EPICS]
---

# WORKFLOW 3: REQUIREMENTS BREAKDOWN (USER STORIES & SPECS)

This workflow ORCHESTRATES the detailed breakdown of Epics into implementable Requirements.
**Goal:** Create clear User Stories (US), Visual Mockups, API Specs, and Data Models so Developers can code without ambiguity.
**Prerequisite:** Workflow 2 (Epics) COMPLETED.
**Roles:** BA (Lead), UI Designer (Layout), SA (Tech Validation), PO (Approval).

## Step 1: User Stories & Data Requirements (BA - CRITICAL)
The BA decomposes the Epic into small stories AND defines the DATA.

1.  **Action**: Use `business-analyst` skill -> `Story Teller` & `Data Definer`.
2.  **Prompt**:
    > "BA, break down EPIC-[ID] into User Stories.
    > For each US, provide:
    > 1. Description, Context/Rules, Acceptance Criteria (Gherkin).
    > 2. **Data Requirements Table**: List every Field, Type, and Validation Rule (e.g., username: string, unique, required)."
3.  **Output**: Create files in `docs/user-stories/[Epic-ID]/US-[ID].md`.

## Step 2: UI Mockups & Flows (UI Designer)
Visualize the requirement before technical design.

1.  **Action (Layout)**: Use `ui-designer` skill -> `Wireframer`.
    > "Create a visual Wireframe (Markdown/Text) for US-[ID] based on the Data Requirements. Ensure all fields are present."
2.  **Action (Flow)**: Use `ui-designer` skill -> `Flow Charter`.
    > "Create a Mermaid diagram showing the user journey for [Epic]."
3.  **Output**: `docs/ui-mockups/SCREEN_[ID].md` or `FLOW_[ID].md`.

## Step 3: Define API Contract (BA + SA - MANDATORY)
Define how Frontend and Backend communicate, based on the approved Data & Mockups.

1.  **Action**: Use `business-analyst` skill (Draft) -> `Contract Designer`.
2.  **Action**: Use `solution-architect` skill (Review) -> `Gap Analyzer`.
    > "SA, verify the API Spec for [Epic]. Does it cover all US Acceptance Criteria AND the UI Mockup fields? Are fields missing or redundant?"
3.  **Output**: Create/Update `docs/api-spec/[Epic-ID].yaml`.

## Step 4: Define Data Model (DB Arch - REVIEWED)
Define the logical data structure.

1.  **Action**: Use `database-architect` skill.
    > "Define the Data Model for EPIC-[ID]. Entities, Attributes, Relationships, Constraints."
2.  **Output**: Create `docs/data-model/[Epic-ID]-data.md`.

## Step 5: Traceability & Completeness Check (SA - MANDATORY)
Ensure everything links back to value.

1.  **Action**: Use `solution-architect` skill -> `Gap Analyzer`.
2.  **Check**:
    *   Do all US link to the Epic?
    *   Does the Mockup match the Data Requirements table?
    *   Does the API support all UI fields?
    *   Is the Data Model sufficient?
3.  **Output**: `docs/04-technical/audits/Gap_Analysis_[Epic].md`.

## Step 6: The "Ready for Dev" Gate
**CRITICAL CHECK**:
- [ ] User Stories contain **Data Requirements Table**?
- [ ] UI Mockups exist and match Data?
- [ ] API Spec is Audit-PASS?
- [ ] Data Model is defined?

**Outcome**:
- If **PASS**: Mark Workflow as **COMPLETED**.
    > "Requirements Ready. Proceed to Workflow 4: Technical Planning / Development."
- If **FAIL**: Blocked. Refine Specs.
