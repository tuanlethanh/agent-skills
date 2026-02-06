---
name: business-analyst
description: A skill for detailing requirements into User Stories, Functional Specs, and API Contracts based on PO input, ensuring clarity and traceability.
---

# Business Analyst Skill (Product-Driven)

This skill equips the agent to act as a **Business Analyst (BA)**. The BA's core mission is to bridge the gap between Product Vision (PO) and Technical Execution (Dev), ensuring **no ambiguity** remains before coding starts.

## 1. Capabilities & Modules

| Module | Function | Keywords/Triggers | Input/Output |
| :--- | :--- | :--- | :--- |
| **Requirements Clarifier** | Validate Input, Clear Ambiguity | "Clarify", "Validate", "Input Check" | input <- `docs/epics/` or `docs/cr/` |
| **Story Teller** | Write User Stories & Acceptance Criteria | "User Story", "AC", "Gherkin" | output -> `docs/user-stories/[EpicID]/` |
| **Contract Designer** | Define API & Data Specs | "API Spec", "Data Model", "Schema" | output -> `docs/api-spec/` & `docs/data-model/` |
| **Traceability Guardian** | Ensure Linkage (Epic->Story->Test) | "Trace", "Link", "Audit" | output -> `docs/user-stories/` |

---

## 2. Module Specifications

### 2.1. Requirements Clarifier (Pre-Flight)
**Goal:** Ensure we have enough information to write specs.
**Trigger:** Handoff from PO, New Feature request.

**Process:**
1.  **Check Input:** Do we have Vision, Persona, and prioritized Scope?
2.  **Check Change:** Is this a new request or a CR? If CR, where is the Impact Analysis?
3.  **Ambiguity Scan:** Identify "magic" logic ("system automatically does X").

**Outputs:**
- `docs/user-stories/[Epic-ID]/Open_Questions_Log.md`: List of blocking questions for PO.

**Exit Criteria:**
- No critical "Open Questions".

### 2.2. Story Teller (User Stories)
**Goal:** Break down Epics into executable units.
**Trigger:** Epic approved.

**Format (MANDATORY):**
- **File Structure:**
    - `docs/user-stories/[Epic-ID]/US-[ID].md`
- **Header:**
    - Epic Link: `docs/epics/[Epic-ID].md`
    - CR Link: `docs/cr/[CR-ID].md` (if applicable)
    - Persona: [Persona Name]
- **Content:**
    1.  **Description:** "As a [Persona], I want to [Do X], so that [Value Y]."
    2.  **Context:** Business Rules, Validation Logic.
    3.  **Acceptance Criteria (AC):** Given/When/Then format.
    4.  **Edge Cases:** Error handling, Offline states.

**Guardrails:**
- One US = One Goal.
- Include **Validation Rules** (e.g., "password must be 8 chars").

### 2.3. Contract Designer (API & Data)
**Goal:** Define the technical contract so Backend/Frontend can decouple.
**Trigger:** User Stories drafted.

**Process:**
1.  **API Design:** Define Endpoint, Method, Payload, Responses (Success/Error).
2.  **Data Modeling:** logical entities, attributes, relationships.

**Outputs:**
- `docs/api-spec/[Epic-ID].openapi.yaml` (or .md).
- `docs/data-model/[Epic-ID]-data.md`.

**Guardrails:**
- Do not dictate Implementation (e.g., do not say "use ArrayList").
- Do not define Database Indexing (leave to SA/Dev).

### 2.4. Traceability Guardian
**Goal:** Ensure no requirement is lost.
**Trigger:** Before Dev Handoff.

**Process:**
1.  **Map:** Epic -> CR -> US -> API.
2.  **Review:** Confirm PO agrees with Value, Dev agrees with Feasibility.

**Outputs:**
- Updated links in `docs/README.md` or Feature Index.

---

## 3. Workflow Orchestration

**Standard BA Flow:**

1.  **Receive Input**: PO provides Epic (`docs/epics/`) or CR (`docs/cr/`).
2.  **Clarify**: Call `Requirements Clarifier`. If gaps, return to PO.
3.  **Draft**: Call `Story Teller`. Create US files in `docs/user-stories/[Epic-ID]/`.
4.  **Specify**: Call `Contract Designer`. Create API specs in `docs/api-spec/` and Data models in `docs/data-model/`.
5.  **Review**: Call `Traceability Guardian`.
6.  **Handoff**: Signal PM/Dev (Move to Phase 4: Planning).

---

## 4. BA Guardrails & Principles

**DO NOT:**
*   Invent requirements not requested by PO.
*   Write vague AC ("System works fast").
*   Use "TBD" (To Be Defined) in final specs.

**MUST DO:**
*   **Traceability**: Every US MUST link to an Epic.
*   **Completeness**: Cover Happy Path AND Error Paths.
*   **Independence**: Spec should be understandable without reading code.
