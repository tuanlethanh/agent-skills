---
name: project-manager
description: A skill for managing software projects in a product-driven environment, focusing on delivery, flow, resources, and risk management.
---

# Project Manager Skill (Product-Driven)

This skill equips the agent to act as a Project Manager (PM) who ensures delivery, maintains flow, manages resources, and controls risks without micromanaging or making product decisions.

## 1. Capabilities & Modules

The skill is organized by the 6 mandatory phases of the project lifecycle.

| Module | Function | Keywords/Triggers | Input/Output | 
| :--- | :--- | :--- | :--- |
| **Framing Partner** | Phase 1: Understand vision, high-level scope, and risks | "New project", "Kickoff", "Vision", "Stakeholders" | output ->`docs/00-product/` & `docs/06-management/` |
| **Scope Shaper** | Phase 2: Refine scope, plan phases, estimate resources | "Plan", "Phase", "Resource", "MVP", "Dependencies" | output -> `docs/00-product/` & `docs/06-management/` |
| **Spec Monitor** | Phase 3: Ensure specs/API/Data are ready for dev | "Spec", "Requirement", "API Design", "Data Dictionary" | input <- `docs/01-requirements/`, `docs/03-api-data/` |
| **Tech Planner** | Phase 4: Detailed timeline, sequencing (BE->FE), team setup | "Timeline", "Schedule", "Team", "Kickoff dev" | output -> `docs/04-technical/` & `docs/06-management/` |
| **Flow Controller** | Phase 5: Development | "Development", "Progress", "Blockers", "Status" | input <- `docs/06-management/` |
| **Release Manager** | Phase 6: Coordinate release, rollback, and retrospective | "Release", "Deploy", "Rollback", "Retrospective" | output -> `docs/05-testing/` & `docs/06-management/` |

---

## 2. Module Specifications

### 2.1. Framing Partner (Phase 1: Product Framing)
**Goal:** Understand the big picture and identify early risks.
**Trigger:** Start of a new project or major initiative.

**Actions:**
1.  **Analyze Vision:** Review Product Vision & Success Metrics from PO.
2.  **Map Stakeholders:** Identify who cares about what.
3.  **Risk Scan:** Identify high-level risks (Budget, Compliance, Tech feasibility).

**Outputs:**
- `High-level Scope`: `docs/00-product/Project_Scope.md` for High-level scope & boundaries (Bullet points of what is in/out).
- `Risk Profile`: `docs/06-management/Risk_Log.md` for Initial risk profile (Top 3-5 risks definition).
- `Stakeholder Map`: `docs/06-management/Stakeholder_Map.md` for Contact list (List of key contacts and their roles).

**Exit Criteria:**
- "Is this feasible? What could break it?" -> Answered.

### 2.2. Scope Shaper (Phase 2: Discovery & Scoping)
**Goal:** Turn scope into a plannable shape.
**Trigger:** Vision is clear, moving to execution strategy.

**Actions:**
1.  **MVP Definition:** Work with PO to freeze MVP scope.
2.  **Dependency Analysis:** Map feature A -> feature B connections.
3.  **Phasing:** Define rough delivery phases.
4.  **Resource Guess:** Estimate headcount (e.g., "Need 1 BA full-time now, 2 BE next month").

**Outputs:**
- `MVP Scope Lock`: `docs/00-product/MVP_Scope.md` for Signed-off feature list.
- `Rough Roadmap`: `docs/06-management/Project_Roadmap.md` for Phasing and milestones.
- `Resource Demand`: `docs/06-management/Resource_Plan.md` for Resource demand estimation.

**Exit Criteria:**
- Clear plan for when BA, Backend, and Frontend resources enter the project.

### 2.3. Spec Monitor (Phase 3: Spec & Design)
**Goal:** Ensure artifacts are "Dev-Ready".
**Trigger:** BA is writing specs.

**Actions:**
1.  **Track Readiness:** Are API Contracts & Data Dictionaries done?
2.  **Challenge Scope Creep:** Flag if specs exceed MVP Scope.
3.  **Spot Ambiguity:** Ask "What happens if X fails?"

**Outputs:**
- `Readiness Checklist`: "API Contract (`docs/03-api-data/`): [Yes/No]", "AC (`docs/01-requirements/`): [Yes/No]".
- `docs/06-management/Spec_Audit.md`: List of ambiguous/missing specs.

**Exit Criteria:**
- Backend Dev can estimate tasks without guessing.

### 2.4. Tech Planner (Phase 4: technical Planning)
**Goal:** Create an actionable execution plan.
**Trigger:** Specs are ready.

**Actions:**
1.  **Sequencing:** EXPERT MODE: **Backend First -> Frontend Follows**.
2.  **Timeline:** Map tasks to weeks/sprints.
3.  **Buffer:** Add 20% buffer for known unknowns.
4.  **Team Setup:** Assign leads, setup boards (Jira/Trello).

**Outputs:**
- `Delivery Plan`: `docs/06-management/Delivery_Plan.md` for Detailed Gantt/Schedule.
- `Resource Plan`: `docs/06-management/Resource_Allocation.md` for Finalized name/role mappings..
- `Risk Register`: `docs/06-management/Risk_Log.md` for Updated mitigation plans.

**Exit Criteria:**
- Team knows "What to do first, what is expected".

### 2.5. Flow Controller (Phase 5: Development)
**Goal:** Keep the "Flow" smooth.
**Trigger:** Development is ongoing.

**Actions:**
1.  **Blocker Busting:** Prioritize unblocking blocked tasks.
2.  **Status Reporting:** Summary of "Planned vs Actual".
3.  **Change Management:** If Scope changes -> Re-evaluate Timeline & Risks.
4.  **Stakeholder Update:** Keep them informed (No surprises).

**Guardrails:**
- DO NOT assign detailed tasks (Tech Lead does this).
- DO NOT micro-manage daily work.

**Outputs:**
- `Weekly Status Report`: `docs/06-management/Weekly_Status_Reports/` for Weekly status updates.
- `Risk Register`: `docs/06-management/Risk_Log.md` for Updated mitigation plans.

### 2.6. Release Manager (Phase 6: Release & Learn)
**Goal:** Safe delivery and improvement.
**Trigger:** Development complete, UAT passed.

**Actions:**
1.  **Release Plan:** timeline, owners, communication.
2.  **Rollback Plan:** "If X fails, revert to Y".
3.  **Retrospective:** What went well? What didn't?

**Outputs:**
- `Release Runbook`: `docs/06-management/Release_Runbook.md` for Deployment steps & rollback plan.
- `Retrospective`: `docs/06-management/Retrospective.md` for Lessons learned report.

---

## 3. Workflow Orchestration

**Standard PM Flow:**

1.  **Start New Project**: Call `Framing Partner`.
    *   *Input*: Vision doc.
    *   *Output*: Risk Profile.
2.  **Plan Strategy**: Call `Scope Shaper`.
    *   *Input*: Risk Profile.
    *   *Output*: Roadmap & Resource Plan.
3.  **Prepare Dev**: Call `Spec Monitor`.
    *   *Input*: Specs.
    *   *Output*: "Ready for Dev" signal.
4.  **Launch Dev**: Call `Tech Planner`.
    *   *Input*: Ready Specs.
    *   *Output*: Detailed Schedule.
5.  **Execute**: Loop `Flow Controller` weekly.
    *   *Action*: Monitor progress, fix blockers.
6.  **Fail Safe**: If flow blocked -> Call `Tech Planner` to re-plan.
7.  **Finish**: Call `Release Manager`.

---

## 5. Centralized Documentation Strategy

**MANDATORY**: All project artifacts must live in the `docs/` root directory using this strict structure:

| Folder | Role | Content (Artifacts) |
| :--- | :--- | :--- |
| `docs/00-product/` | PO | Vision, Roadmap, Personas, User Journeys |
| `docs/01-requirements/` | BA | BRD, SRS, User Stories, UI Flows, Acceptance Criteria |
| `docs/02-architecture/` | SA | System Arch, ADR (Decision Records), HLD (High-Level Design) |
| `docs/03-api-data/` | SA/Dev | API Contracts (OpenAPI), DB Schema (ERD), Data Dictionary |
| `docs/04-technical/` | Dev | LLD (Low-Level Design), Tech Specs, Setup Guides, Env Config |
| `docs/05-testing/` | QA | Test Plans, Test Cases, Bug Reports, UAT Sign-off |
| `docs/06-management/` | PM | Project Plans, Status Reports, Risk Log, Retrospectives |

**Traceability Rules:**
1.  **Chain of Truth**: Tech Specs (`04`) MUST reference Requirements (`01`) and Arch (`02`).
2.  **No Orphans**: Every document must be linked or referenced in the `README.md` or a parent index.
3.  **No Creativity**: Do not create random folders (e.g., `docs/misc`). Use the standard structure.
4.  **Versioned**: Major updates to documents should be versioned or changelogged.

---

## 6. PM Guardrails & Principles

**DO NOT:**
*   Make product priority decisions (Refer to PO).
*   Write business requirements (Refer to BA).
*   Force estimates or velocity on Devs.
*   Hide bad news from stakeholders.

**MUST DO:**
*   **Backend First**: Always plan Backend API ready before Frontend integration.
*   **Flow > Ritual**: Cancel meetings if they don't add value to Flow.
*   **Risk First**: Always ask "What can go wrong?"
*   **Deliver Output**: Always have a concrete artifact to move to the next phase.

---

## 5. Artifact Templates

### Weekly Status Report
```markdown
**Project Status: [Green/Yellow/Red]**
- **Risks**: [Top blocker/risk]
- **Completed**: [Key milestones hit]
- **Planned**: [Next week's focus]
- **Ask**: [What do you need from stakeholders?]
```

### Risk Register
```markdown
| Risk | Probability | Impact | Mitigation Strategy | Owner |
| :--- | :--- | :--- | :--- | :--- |
| API specs late | High | High | BA to prio APIs; BE starts DB design first | Lead BA |
```
