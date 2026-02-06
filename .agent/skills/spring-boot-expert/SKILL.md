---
name: spring-boot-expert
description: A comprehensive skill set for developing, refactoring, and optimizing Spring Boot API backends (Java 17/21+) with Clean Architecture, best practices, and security guardrails.
---

# Spring Boot API Backend Expert Skill (Product-Driven)

This skill equips the agent with specialized capabilities to develop, review, refactor, optimize, and harden Spring Boot API applications following production-grade standards.

## 1. Capabilities & Modules

The skill is organized into functional modules.

| Module | Function | Keywords/Triggers | Input/Output |
| :--- | :--- | :--- | :--- |
| **Architect Core** | Clean Arch/Hexagonal Design, Feature Implementation | "New feature", "Create API", "Architecture", "Structure" | input <- `docs/user-stories/` & `docs/api-spec/` |
| **JPA Optimizer** | Data Persistence, Migration, Query Tuning | "JPA", "Hibernate", "Entity", "Migration", "N+1" | input <- `docs/data-model/` |
| **Security Sentinel** | AuthN/AuthZ, OWASP, Validation | "Security", "Auth", "OAuth2", "JWT", "Vulnerability" | input <- `docs/security/` |
| **Perf Tuner** | JVM, Cache, Async, Threading | "Performance", "Slow", "Cache", "Memory", "CPU" | - |
| **Reliability Ops** | Observability, Resilience, Error Handling | "Log", "Metric", "Trace", "Circuit Breaker", "Error" | - |
| **Test Engineer** | Unit/Integration Testing, Testcontainers | "Test", "Coverage", "JUnit", "Mockito", "Testcontainers" | - |
| **Refactor Pro** | Code Quality, Standards, Static Analysis | "Refactor", "Clean code", "Sonar", "Checkstyle" | output -> `docs/04-technical/` |

---

## 2. Module Specifications

### 2.1. Architect Core (Clean Architecture / Hexagonal)
**Goal:** Implement maintainable features separating Domain, Application, and Infrastructure layers.

**Trigger:** "Create user API", "Implement order feature", "Design service".

**Inputs:**
- User Story: `docs/user-stories/[Epic]/US-XXX.md`
- API Spec: `docs/api-spec/[Epic].yaml`

**Outputs:**
- **Domain**: Entities, Value Objects (Records), Repository Interfaces (Ports).
- **Application**: Use Cases/Services, DTOs, Mappers.
- **Infrastructure**: Repository Impl, External Adapters.
- **Interface**: REST Controller, Request/Response DTOs.

**Steps/Algorithm:**
1.  **Domain Definition**: Define robust Entities and Value Objects. Ensure business invariants are checked here.
2.  **API Contract**: Define Request/Response DTOs (Records in Java 17+).
3.  **Port Definition**: Create interfaces for data access (Repository) or external services in the Domain/Application layer.
4.  **Application Service**: Implement the Use Case.
    *   `@Transactional(readOnly = true)` by default, `readOnly = false` for write methods.
    *   Map DTOs to Domain objects (use MapperStruct or manual mapping).
5.  **Adapter Implementation**: Implement Repository interfaces using Spring Data JPA.
6.  **Controller Implementation**:
    *   Inject Application Service.
    *   Receive Request DTO -> Validate (`@Valid`) -> Call Service -> Map to Response DTO -> Return `ResponseEntity`.
    *   Handle exceptions via `@ControllerAdvice`.

**Examples:**
*   **Structure**:
    ```text
    com.example.project
    ├── domain
    │   ├── model (Entity, VO)
    │   └── port (Repository Interfaces)
    ├── application
    │   ├── usecase/service
    │   └── dto
    ├── infrastructure
    │   └── adapter (JPA Repository, Redis Client)
    └── interface
        └── controller (REST)
    ```
*   **Controller**:
    ```java
    @RestController
    @RequestMapping("/api/v1/users")
    @RequiredArgsConstructor
    class UserController {
        private final CreateUserUseCase createUserUseCase;

        @PostMapping
        public ResponseEntity<UserResponse> create(@RequestBody @Valid CreateUserRequest request) {
            var result = createUserUseCase.execute(request.toCommand());
            return ResponseEntity.status(HttpStatus.CREATED).body(UserResponse.from(result));
        }
    }
    ```

**Guardrails:**
- NO business logic in Controllers.
- NO JPA Entities returned directly in API responses (Always map to DTOs).
- NO `autoconfigure` magic without understanding; prefer explicit configuration for complex beans.

### 2.2. JPA Optimizer (Data & Persistence)
**Goal:** Manage data efficiently, prioritized for **PostgreSQL** (if Java stack).

**Trigger:** "Create entity", "Fix slow query", "Add migration".

**Inputs:** Data Model (`docs/data-model/`).

**Guidelines:**
1.  **N+1 Prevention**:
    *   Use `@EntityGraph` or `JOIN FETCH` in JPQL for loading relationships.
    *   AVOID `EAGER` fetching in `@OneToMany` or `@ManyToMany`.
2.  **Indexing**: Analyze query patterns and add indexes via Liquibase/Flyway (not just `@Index`).
3.  **Transactions**: Keep transactions short. process data in batches if large.
4.  **Migration**: Always usage Flyway/Liquibase. No auto-ddl.
5.  **Pagination**: Always use `Pageable` for list endpoints.

### 2.3. Security Sentinel (OWASP & Spring Security)
**Goal:** Harden API and manage AuthN/AuthZ.

**Trigger:** "Secure access", "Add role", "Validate input".

**Guidelines:**
1.  **Config**: Use standard Spring Security `SecurityFilterChain`.
2.  **Method Security**: Use `@PreAuthorize("hasRole('ADMIN')")` or `@PreAuthorize("#userId == authentication.principal.id")` to enforce ownership.
3.  **Input Validation**: Use Bean Validation (`@NotNull`, `@Size`, `@Pattern`) on DTOs.
4.  **Secrets**: Externalize via `@ConfigurationProperties` or Vault. Never hardcode.

### 2.4. Perf Tuner (Performance)
**Goal:** Optimize JVM, Database, and Network interaction.

**Trigger:** "High latency", "Memory leak", "Timeout".

**Strategy:**
1.  **Caching**: Use Caffeine for local hot-data, Redis for shared.
    *   `@Cacheable(value = "users", key = "#id")`
2.  **Async**: Use `@Async` properly with a custom `Executor` (don't use default `SimpleAsyncTaskExecutor`).
3.  **Connection Pooling**: Tune HikariCP (`maximumPoolSize`, `connectionTimeout`) based on load tests.

### 2.5. Test Engineer (Reliability)
**Goal:** Verify logic with meaningful tests.

**Trigger:** "Write tests", "Debug feature".

**Strategy:**
1.  **Unit Tests**: JUnit 5 + Mockito. Test Domain and Services isolation.
2.  **Integration Tests**: `@SpringBootTest` + `Testcontainers` (Postgres, Kafka, Redis).
    *   Spin up real DB constraints, not H2 (unless simple).
3.  **Usage**:
    *   `webEnvironment = WebEnvironment.RANDOM_PORT` for full stack tests.
    *   `@MockBean` for external 3rd party services.

### 2.6. Reliability Ops (Observability)
**Goal:** Ensure system is observable and resilient.

**Guidelines:**
1.  **Logging**: Use SLF4J. Include context (UserId, OrderId) in MDC.
2.  **Tracing**: Ensure OpenTelemetry agents or Micrometer Tracing is active.
3.  **Resilience**: Apply Resilience4j Circuit Breakers for external calls.
    *   `@CircuitBreaker(name = "payment-service", fallbackMethod = "fallback")`
4.  **Problem Details**: Map Exceptions to RFC 7807 responses using `@ControllerAdvice`.

---

## 3. Workflow Orchestration

### Scenario: New Feature Development
1.  **(Design)** `Architect Core`: Read `docs/user-stories/` and `docs/api-spec/`. Define Domains.
2.  **(DB)** `JPA Optimizer`: Create Flyway/Liquibase migration. Implement Entity & Repository.
3.  **(Core)** `Architect Core`: Implement Service Logic & Unit Tests (`Test Engineer`).
4.  **(API)** `Architect Core`: Implement Controller & DTOs.
5.  **(Security)** `Security Sentinel`: Configure `@PreAuthorize` and DTO Validation.
6.  **(Verify)** `Test Engineer`: Write Integration Test with Testcontainers.

### Scenario: Bug Fix
1.  **(Reproduce)** `Test Engineer`: Create failing `@Test` case.
2.  **(Debug)** Analyze stack trace/logs.
3.  **(Fix)** `Refactor Pro`: Patch code.
4.  **(Verify)** Run all tests.

### Scenario: Performance Refactor
1.  **(Analyze)** `Perf Tuner`: Check Slow Query Logs or Java Flight Recorder.
2.  **(Tune)** Opt 1: Add DB Index. Opt 2: Add Cache. Opt 3: Refactor to Async.
3.  **(Verify)** Load test.

---

## 4. Pre-Merge Checklist (Product-Driven)

1.  **Traceability**:
    - [ ] Code implements specific US (referenced in commit/PR).
    - [ ] API matches `docs/api-spec/`.
    - [ ] No `System.out.println` (Use Slf4j).
    - [ ] No generic `Exception` catching (`catch (Exception e)`).
    - [ ] Constants extracted, Magic numbers removed.
    - [ ] Records used for DTOs (if Java 17+).
2.  **Architecture**:
    - [ ] Logic in Services, Controllers are thin.
    - [ ] No N+1 Queries detected
    - [ ] Migration scripts (Flyway/Liquibase) are backward compatible.
    - [ ] Pagination applied to list endpoints.
3.  **Testing**:
    - [ ] Unit tests (Service) pass.
    - [ ] Integration tests (Controller+DB) pass.
4.  **Security**:
    - [ ] Inputs validated (`@Valid`).
    - [ ] AuthZ checks present (`@PreAuthorize`).
