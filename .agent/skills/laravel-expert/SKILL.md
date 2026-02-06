---
name: laravel-expert
description: A comprehensive skill set for developing, refactoring, and optimizing Laravel API backends with Clean Architecture, best practices, and security guardrails.
---

# Laravel API Backend Expert Skill (Product-Driven)

This skill equips the agent with specialized capabilities to develop, review, refactor, optimize, and secure Laravel API applications following production-grade standards.

## 1. Capabilities & Modules

The skill is organized into functional modules.

| Module | Function | Keywords/Triggers | Input/Output |
| :--- | :--- | :--- | :--- |
| **Feature Architect** | Design & implement new features | "New feature", "Create API", "Implement logic" | input <- `docs/user-stories/` & `docs/api-spec/` |
| **Eloquent Master** | DB Schema, Migration, Query Optimization | "Migration", "Schema", "Query", "N+1", "Repository" | input <- `docs/data-model/` |
| **Security Guard** | Audit, Validation, AuthN/AuthZ | "Security", "Auth", "Vulnerability", "Review" | input <- `docs/security/` |
| **Perf Booster** | Cache, Queue, Rate Limit, Profiling | "Slow", "Performance", "Cache", "Optimize" | - |
| **Test Engineer** | Unit/Feature Testing, TDD | "Test", "Coverage", "PHPUnit" | - |
| **Refactor Pro** | Code Cleanup, Standards, Static Analysis | "Refactor", "Clean code", "Standard", "Lint" | output -> `docs/04-technical/` |

---

## 2. Module Specifications

### 2.1. Feature Architect (Core Implementation)
**Goal:** Implement robust, maintainable API features using Clean Architecture principles.

**Trigger:** "New feature", "Create API", "Implement logic"

**Guidelines:**
1.  **Architecture Layering**:
    *   **Controllers**: Thin. Only handle HTTP request (restore), validate (via FormRequest), delegate to Service/UseCase, and return response (via Resource/Json).
    *   **Domain/Service Layer**: Contains ALL business logic.
    *   **DTOs**: Use Data Transfer Objects to pass data between layers.
    *   **Repositories**: Abstract database access (Strictly separate if requested, otherwise keep Eloquent isolated from Controllers).
2.  **Implementation Steps**:
    *   Analyze requirements & define Domain Models.
    *   **Inputs**:
    *   Requirement (`docs/user-stories/`).
    *   Context: API Contract (`docs/api-spec/`), usage of `docs/04-technical/` for complex logic.
    *   **Outputs**:
    *   File code: Controller, FormRequest, DTO, Service/UseCase, Route.
    *   Update `docs/04-technical/` if implementation details change.
    *   Bind Interfaces in `ServiceProvider` (if using Dependency Injection).

**Code Constraints:**
- NO business logic in Controllers.
- NO direct Eloquent queries in Controllers (use Repository or Service scope).
- Return `JsonResource` for consistent API responses.

### 2.2. Eloquent Master (Database & Data)
**Goal:** Manage data integrity and optimize database interactions, prioritized for **MySQL**.

**Trigger:** "Migration", "Schema", "Query", "N+1", "Repository"

**Guidelines:**
1.  **Migrations**: Always include `down()` methods. Use correct types (`bigInteger`, `decimal`).
2.  **Models**: Define `$fillable`, `$casts`, and relationships explicitly.
3.  **Query Optimization**:
    *   **Avoid N+1**: ALWAYS use `with()` for eager loading relationships in lists.
    *   **Indexing**: Add indexes for foreign keys and frequently queried columns.
    *   **Pagination**: Use `paginate()` instead of `get()` for lists.
    *   **No Queries in Loops**: Batch queries outside loops.

### 2.3. Security Guard (OWASP & Best Practices)
**Goal:** Harden the application against vulnerabilities.

**Trigger:** "Security", "Auth", "Vulnerability", "Review"

**Guidelines:**
1.  **Validation**: Strict validation rules in FormRequests.
2.  **Authorization**: Use Policies/Gates (`$this->authorize()`) for all resource access.
3.  **Sanitization**: Escape outputs, validate file uploads (mime, size).
4.  **Secrets**: NEVER commit keys/secrets. Use `.env`.
5.  **Audit**: Check for Mass Assignment (ensure `$fillable` is strict).

### 2.4. Perf Booster (Performance)
**Goal:** Maximize speed and scalability.

**Trigger:** "Slow", "Performance", "Cache", "Optimize"

**Guidelines:**
1.  **Caching**: Use Redis. Cache expensive queries/computations with TTL.
    *   `Cache::remember('key', $ttl, fn() => ...)`
2.  **Queues**: Offload heavy tasks (Email, Export, 3rd party API calls) to Laravel Horizon/Queue.
3.  **Profiling**: Identify bottlenecks using logs or debug tools.

### 2.5. Test Engineer (Reliability)
**Goal:** Ensure code correctness and prevent regressions.

**Trigger:** "Test", "Coverage", "PHPUnit"

**Guidelines:**
1.  **Feature Tests**: Test API endpoints (Happy Path + Error Cases + Auth boundaries).
2.  **Unit Tests**: Test isolated business logic in Services/Domain classes.
3.  **Factories**: Use Factories for setting up test state. Avoid hardcoded IDs.
4.  **Assertions**: Verify Status Code, JSON Structure, and Database State.

---

## 3. Workflow Orchestration

Follow these standard procedures based on the task type:

### Scenario: New Feature Development
1.  **(Input)** User provides requirements (`docs/user-stories/` and `docs/api-spec/`).
2.  **(Plan)** `Feature Architect`: Outline DB changes, API signature, and Logic.
3.  **(DB)** `Eloquent Master`: Create & Run Migrations.
4.  **(Core)** `Feature Architect`: Implement Service/UseCase -> Controller -> Route.
5.  **(Security)** `Security Guard`: Add Policy checks and FormRequest validation.
6.  **(Test)** `Test Engineer`: Write & Pass Feature Tests.
7.  **(Refactor)** `Refactor Pro`: standard check (Pint/PHPStan).

### Scenario: Bug Fix
1.  **(Reproduction)** `Test Engineer`: Create a failing test case (Red).
2.  **(Analysis)** Analyze logs/code to find root cause.
3.  **(Fix)** `Refactor Pro`: Patch the code (Green).
4.  **(Verify)** Run full test suite.

### Scenario: Performance Optimization
1.  **(Audit)** `Perf Booster`: Identify hot paths/queries.
2.  **(Optimize)** Apply Caching, Indexing, or Queues.
3.  **(Verify)** Benchmark results.

---

## 4. Pre-Merge Checklist (Product-Driven)

Before considering a task complete, verify:

1.  **Clean Code**: PSR-12 compliant, no debug code (`dd`, `dump`).
2.  **Architecture**: Logic in Services, Controllers are thin.
3.  **Performance**: No N+1 queries detected.
4.  **Security**: Inputs validated, Authorization checks present.
5.  **Testing**: Relevant tests added and passing.
6.  **Response**: API returns consistent JSON structure/Status codes.
7.  **Documentation**: `docs/api-spec/` updated.
