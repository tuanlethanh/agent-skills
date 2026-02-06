---
name: database-architect
description: A comprehensive skill for designing, reviewing, and optimizing production-grade database schemas, tailored for PostgreSQL (Spring Boot) and MySQL (Laravel).
---

# Database Architect Skill

This skill equips the agent to design scalable, secure, and performant database schemas. It is **Stack-Aware**, enforcing specific best practices for Java/Spring Boot (PostgreSQL) and Laravel/PHP (MySQL).

## 1. Capabilities & Modules

| Module | Function | Keywords/Triggers |
| :--- | :--- | :--- |
| **Schema Designer** | Design ERD, Tables, Relationships, Data Types | "Database", "Schema", "Migration", "Table" |
| **Index Strategist** | Plan Indexes based on Query Patterns | "Slow query", "Index", "Optimize", "Search" |
| **Stack Advisor** | **Critial**: Select DB rules based on backend stack | "Java", "Spring", "Laravel", "Postgres", "MySQL" |
| **Migration Master** | Safe schema evolution & rollback plans | "Alter", "Add column", "Rollback", "Version" |
| **Consistency Guard** | **Critical**: Check DB ↔ Domain ↔ API ↔ UI alignment | "Consistency", "Mapping", "Breaking change", "Field check" |
| **DDR Generator** | Create mandatory Design Decision Records | "Design", "Plan", "Architecture" |

---

## 2. Module Specifications

### 2.1. Stack Advisor (Stack-Aware Design)
**Goal:** Enforce DB choice and practices based on the backend technology.

**Trigger:** "New project", "Init DB", "Design Schema".

**Algorithm:**
1.  **Identify Stack**: Check project files (`pom.xml`/`build.gradle` vs `composer.json`).
2.  **Select Strategy**:
    *   **IF Java/Spring Boot**: -> **Target: PostgreSQL**.
        *   Use `BIGSERIAL` / `UUIDv7`.
        *   Leverage `JSONB`, `Partial Index`.
        *   JPA Mapping focus (Lazy/Eager, `@Version`).
    *   **IF Laravel/PHP**: -> **Target: MySQL (InnoDB)**.
        *   Use `BIGINT UNSIGNED`.
        *   Use `utf8mb4`.
        *   Eloquent Mapping focus (Eager Loading, Avoid Loop Queries).
    *   **Else**: Ask user or assume based on context.

### 2.2. Schema Designer & DDR Generator
**Goal:** Create robust physical schemas with a documented decision record.

**Trigger:** "Create table", "Add feature", "Data model".

**Mandatory Output: DB-DDR**
The agent MUST generate this record before writing DDL/Migration code.
**Location**: `docs/03-api-data/DB_DDR_[FeatureName].md`

```markdown
# DB Design Decision Record

## Context
- Feature: [Name]
- Backend: [Java Spring / Laravel]
- Database: [PostgreSQL / MySQL]
- Workload: [e.g. Read-heavy 80/20]
- Scale: [e.g. 1M Users, 100 QPS]

## Design Choices
- Normalization: [e.g. 3NF]
- ID Strategy: [e.g. UUIDv7 for security, BIGINT for performance]
- Auditing: [e.g. created_at, updated_at, deleted_at]

## Index Strategy
- Primary: [PK_id]
- Secondary: [idx_user_email, idx_status_created]
- Unique: [uk_email]

## Trade-offs
- Gain: [e.g. Fast lookups by Email]
- Sacrifice: [e.g. Slightly slower writes]

## Migration
- Forward: [Flyway V2__Add_Users.sql / Artisan migrate]
- Rollback: [DROP TABLE / down() method]
```

### 2.3. Index Strategist
**Goal:** Optimize query performance without over-indexing.

**Guidelines:**
*   **PostgreSQL**:
    *   Use **Partial Indexes** for queues/soft-deletes (`WHERE deleted_at IS NULL`).
    *   Use **GIN** for JSONB or Text Search.
    *   Use **Composite Indexes** strictly matching Left-to-Right query usage.
*   **MySQL**:
    *   **Covering Indexes**: Include selected columns in index to avoid checking heap.
    *   **Composite Indexes**: Strictly follow Left-Most Prefix rule.

### 2.4. Migration Master
**Goal:** Zero-downtime schema evolution.

**Guidelines:**
1.  **Non-Locking**: In Postgres, creating indexes concurrently. In MySQL, careful with large tables (consider `gh-ost` or online schema change tools if huge, otherwise standard migration).
2.  **Backwards Compatible**:
    *   Don't rename columns directly (Add new -> Copy -> Drop old).
    *   Don't change types destructively without temporary columns.
3.  **Tooling**:
    *   Spring: **Flyway** or **Liquibase**.
    *   Laravel: **Artisan Migrations**.

### 2.5. Consistency Guard (Cross-Layer Alignment)
**Goal:** Ensure end-to-end alignment between Database, Domain, API, and UI layers.

**Trigger:** "Change schema", "Rename field", "Update validation", "Consistency check".

**Outputs:**
1.  **Consistency Report**: Pass/Fail status (`docs/03-api-data/Consistency_Report.md`).
2.  **Mapping Table**: `docs/03-api-data/Data_Flow_Map.md` for clear matrix showing data flow naming.
    *   `DB (users.email)` ↔ `Domain (User.email)` ↔ `API (UserResponse.emailAddress)` ↔ `UI (form.email)`
3.  **Validation Matrix**:
    *   `DB (VARCHAR(50))` ↔ `Backend (@Size(max=50))` ↔ `UI (maxlength="50")`

**Checklist:**
- [ ] **Naming**: Are field names consistent (or explicitly mapped)?
- [ ] **Types**: Does DB type (e.g., `DECIMAL`) match Domain/API type (e.g., `BigDecimal`, `string`) to prevent precision loss?
- [ ] **Constraints**: Are DB constraints (`NOT NULL`, `UNIQUE`) reflected in Backend/UI validation messages?
- [ ] **Leakage**: Are DB Entities separate from API DTOs?

**Guardrails:**
- **No Breaking API Changes** without explicit versioning or warnings.
- **Validation Sync**: Backend validation must be *at least* as strict as UI validation.
- **No Leaky Abstractions**: Never expose raw DB errors to the UI/API.

---

## 3. Workflow Orchestration

### Scenario: New Feature (e.g., "Order System")
1.  **(Analyze)** `Stack Advisor`: Detect Backend (e.g., Spring Boot) -> Select Postgres Profile.
2.  **(Plan)** `Schema Designer`: Define `orders`, `order_items` tables.
3.  **(Doc)** `DDR Generator`: Create DB-DDR. Define UUID strategy, `JSONB` for snapshot data.
4.  **(Code)** `Migration Master`: Write Flyway SQL script (`V1__create_orders.sql`).
5.  **(Refine)** `Index Strategist`: Add index on `customer_id` and `created_at` for history queries.

### Scenario: Performance Fix (Slow Query)
1.  **(Analyze)** Check `EXPLAIN ANALYZE` (PG) or `EXPLAIN` (MySQL).
2.  **(Strategy)** `Index Strategist`: Identify missing composite index or need for Covering Index.
3.  **(Doc)** Update DB-DDR with new index rationale.
4.  **(Apply)** Write non-blocking migration to add index.

### Scenario: Cross-Layer Refactor (Rename Column)
1.  **(Analyze)** `Consistency Guard`: Trace usage of column `cust_name` -> Domain `customerName` -> API `name`.
2.  **(Plan)** `Migration Master`: Create additive migration (Add `name`, copy data).
3.  **(Code)** Update Domain mapping & API DTOs.
4.  **(Verify)** `Consistency Guard`: Check if UI forms need generic update or if API contract preserved.

---

## 4. DB Design Checklist (Pre-Merge)

- [ ] **Consistency Checked**: DB ↔ Domain ↔ API ↔ UI names and types aligned.

- [ ] **DB-DDR Created**: Choices documented.
- [ ] **Stack Aligned**:
    - [ ] If Spring: PG features used correctly, JPA N+1 checked.
    - [ ] If Laravel: MySQL types used correctly, Eloquent N+1 checked.
- [ ] **Keys & Constraints**:
    - [ ] PK defined (BIGINT/UUID).
    - [ ] FK defined (with indexing!).
    - [ ] Unique constraints enforced at DB level.
- [ ] **Indexing**:
    - [ ] No duplicate indexes.
    - [ ] Composite indexes cover common filters.
- [ ] **Migration**:
    - [ ] Reversible (Down script/method).
    - [ ] Idempotent (Safe to run twice).
