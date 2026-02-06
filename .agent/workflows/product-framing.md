---
description: Workflow 1 - Product Framing [MANDATORY START]
---

# /framing  WORKFLOW 1 - PRODUCT FRAMING

$ARGUMENTS

---

This workflow ORCHESTRATES the initial "Product Framing" phase.
- **NO CODE WRITING** - This command creates Product Framing file only
- **Goal:** Define What, Who, Why, and Where (Short/Long term) before any technical work begins.
- **Prerequisite:** None. This is the entry point.
- **Roles:** PO (Owner), BA (Support), PM (Feasibility).

## Step 1: Context & Vision (PO)
The agent must verify if the **Product Vision** exists and is clear.

1.  **Action**: Use `product-owner` skill -> `Vision Architect` module.
2.  **Prompt**:
    > "PO, please provide the Project Context and Product Vision.
    > Format: 'This product helps [User] solve [Problem] by [Key Value].'"
3.  **Validation**:
    *   Is the User defined?
    *   Is the Problem specific?
    *   Is the Value clear (not just 'better/faster')?
    *   *If NO:* Reject and ask PO to refine.

## Step 2: Personas (PO)
Define who we are building for.

1.  **Action**: Use `product-owner` skill -> `Vision Architect` module.
2.  **Prompt**:
    > "Create at least 1 Primary Persona. Include: Name, Role, Goals, Pain Points, Tech Savviness."
3.  **Validation**:
    *   Is the persona valid? (Not "Everyone").
    *   *If NO:* Reject.

## Step 3: Direction & Strategy (PO + PM)
Define the roadmap strategy.

1.  **Action**: Use `product-owner` skill -> `Vision Architect`.
    > "Define Short-term (3-6m) goals and Long-term (12-24m) direction."
2.  **Action**: Use `project-manager` skill -> `Framing Partner`.
    > "PM, review the Long-term direction for high-level risks or feasibility issues."

## Step 4: Scope Boundaries (PM + PO)
Define the database of the project (High Level).

1.  **Action**: Use `project-manager` skill -> `Scope Shaper`.
    > "Define what is IN-SCOPE and OUT-OF-SCOPE for the initial phase/MVP."

## Step 5: Synthesis & Documentation
Compile all outputs into the `docs/` structure.

1.  **Generate**: `docs/00-product/Product_Vision.md` (Vision, Direction, Glossary).
2.  **Generate**: `docs/00-product/Personas.md` (Persona definitions).
3.  **Generate**: `docs/00-product/Project_Scope.md` (In/Out Scope).
4.  **Generate**: `docs/06-management/Risk_Log.md` (Initial risks from Step 3).

## Step 6: The "Framing Gate"
**CRITICAL CHECK**:
- [ ] Product Vision is clear?
- [ ] At least 1 Persona?
- [ ] Short/Long term defined?
- [ ] Scope agreed?

**Outcome**:
- If **PASS**: Mark Workflow as **COMPLETED**.
    > "Product Framing Complete. You may now proceed to Workflow 2: Change Request / Feature Definition."
- If **FAIL**: Mark as **BLOCKED**.
    > "Framing Incomplete. Please resolve missing items before proceeding."