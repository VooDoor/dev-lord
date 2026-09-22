# Road B: The Re-Platform & Migration Architect

> **Mission:** Re-platform, port, modernize, or rewrite an existing working codebase into a new technology stack with **zero functional regression**. Road B discovers hidden business logic, database triggers, and lifecycle hooks, establishes baseline parity samples, and formulates an airtight migration blueprint before any target code is written.

---

## Operating Flow

```text
[Step B1: Source Coordinates & Pre-Flight Health Check]
       │
       ├── (Detects missing parts? Informs user, logs items, offers continuation)
       ▼
[Step B2: Execution Capability Check] (Full Rigor vs Reduced Rigor)
       │
       ▼
[Step B3: Source Stack & Hidden-Logic Discovery]
       │ - Scans codebase with <SKILL_DIR>/references/hidden-logic-patterns.md
       │ - Captures concrete input/output Parity Samples
       ▼
[Step B4: Target Architecture & UI Strategy Advisory]
       │ - Sequential ONE Question at a time
       │ - Offers [A] Recommended, [B], [C] + [D] Custom / Other (Free Write-in)
       │ - Questions 4 (Auth) and 6 (UI) are conditional on app profile
       ▼
[Step B5: Avoid-List & Blueprint Compilation]
       │ - Compiles ANALYSIS.md (<SKILL_DIR>/templates/analysis.md.template)
       │ - Compiles CONVERSION_PLAN.md (<SKILL_DIR>/templates/conversion_plan.md.template)
       ▼
[Blueprint Review & Approval Gate] ◄──► User feedback, custom constraints & sign-off
       │ (User explicitly approves)
       ▼
[Intersection Gate B-X]
  ├── [BX-1] Export 6-Phase Migration Prompt (Standalone file for external AI)
  ├── [BX-2] Live Migration ──► Bridge to Road C (Step C6)
  └── [BX-3] UI Prototype via Road U ──► Bridge to Road U (Step U0)
```

---

## Execution Steps

### Step B1: Source Coordinates & Pre-Flight Health Check
1. Collect source repository location (local path or git clone URL) and target project directory (default: `new_app_stack/` within project root).
2. **Automated Manifest & Ecosystem Detection:**
   Inspect root directory for primary project manifests:
   - Node: `package.json`
   - Ruby: `Gemfile`
   - Python: `requirements.txt`, `Pipfile`, `pyproject.toml`
   - PHP: `composer.json`
   - .NET: `*.csproj`, `*.sln`
   - Java/Kotlin: `pom.xml`, `build.gradle`
   - Go: `go.mod`
3. **Health & Completeness Verification:**
   - Verify directory is readable and non-empty.
   - Check for essential configuration files (e.g., database connection configs, schema migrations, environment templates).
   - **Missing Parts Protocol:** If critical files or dependencies are missing (e.g., missing database schema, absent worker config, missing `.env.example`):
     - **DO NOT** abruptly terminate the process.
     - Inform the user immediately:
       > *"I have detected [Source Framework/Version], but noted the following missing or incomplete components:  
       > - [List of missing elements]  
       > Would you like to continue with the migration analysis? If yes, I will log these missing pieces in our analysis and work with you in Step B4 to select appropriate target replacements or alternates."*
     - If the user confirms continuation, record all missing items in the working state for resolution during Step B4 and Step B5.
4. **Legacy Test Suite Oracle Discovery:**
   Inspect the source directory for existing automated test suites (`tests/`, `spec/`, `__tests__/`, `*Test.java`, `*_test.go`, etc.):
   - If test suites exist: inventory them as the primary behavioral oracle in `ANALYSIS.md`. Note the test runner command and key test files.
   - If no tests exist: record that parity verification must rely on documented input/output samples.

---

### Step B2: Execution Capability Check
Consult **`<SKILL_DIR>/references/verification-fallbacks.md`** and ask the user:
> *"Will this environment have shell/terminal access to run live build commands, execute database migrations, and make real HTTP requests?*
> - **[1] Full Rigor (Recommended)**: Require verified command/test output for every task before moving forward.
> - **[2] Code-Review Only (Reduced Rigor)**: Acknowledge that the environment cannot run code; rely on manual inspection."*

---

### Step B3: Source Stack & Hidden-Logic Discovery
Inspect source files for ecosystem-specific implicit behaviors using **`<SKILL_DIR>/references/hidden-logic-patterns.md`**:
- **Supabase / Firebase:** Row-Level Security (RLS) policies, storage rules, auth triggers.
- **Ruby on Rails:** ActiveRecord lifecycle callbacks (`before_save`, `after_commit`), model concerns, background jobs.
- **Django:** `signals.py`, `save()` overrides, custom request/response middleware.
- **Laravel:** Model Observers, Form Request authorization rules, Eloquent global scopes.
- **Spring Boot:** AOP Aspects, `@PrePersist`/`@PreUpdate` hooks, security interceptors.
- **Express / Node:** Middleware chains, ad-hoc inline validations.

**Parity Baseline Requirement:**
For each non-trivial calculation, permission check, or state transition discovered, record a **representative input and the original app's actual output/behavior**:
- **Tag Baseline Source:** Explicitly tag each sample as either:
  - `Baseline Source: LIVE_OBSERVED` (captured from real execution of running source app)
  - `Baseline Source: STATIC_INFERRED` (deduced from static inspection of source code)
- This concrete sample becomes the mandatory baseline for Road C Step C9's Migration Parity Check.

---

### Step B4: Target Architecture & UI Strategy Advisory

**Strict Protocol: ONE Question at a Time with Open Choice ([D] Custom / Other):**
Present questions sequentially. For each question, offer the AI's contextual recommendation first (derived from source analysis), sensible alternatives, and always include an option allowing the user to specify their own choice.

#### Question 1: Architecture Pattern & Decoupling
> - **[A] (Recommended):** [e.g., Decoupled: Fastify TypeScript API + Next.js Frontend — modern separation of concerns]
> - **[B] Monolithic SSR:** [e.g., Unified framework handling both backend and server-rendered views]
> - **[C] Micro-services / Edge API:** [Separate domain workers for high scale]
> - **[D] Custom / Other:** Specify your preferred architectural topology.

#### Question 2: Backend Framework & Language Runtime
*(Wait for user response to Question 1 before presenting)*
> - **[A] (Recommended):** [Best-fit modernization based on source stack]
> - **[B] Lightweight Alternative:** [Minimal overhead runtime]
> - **[C] Enterprise Alternative:** [Strictly typed, high-throughput ecosystem]
> - **[D] Custom / Other:** Specify your exact programming language and framework.

#### Question 3: Database & Data Access Layer
*(Wait for user response to Question 2 before presenting)*
> - **[A] (Recommended):** [Target ORM/driver matching source complexity]
> - **[B] Query Builder / Raw SQL:** [Direct SQL control without heavy ORM abstraction]
> - **[C] High-Level Schema ORM:** [Auto-migrations and type generation]
> - **[D] Custom / Other:** Specify your preferred database engine and data access library.

#### Question 4: Authentication & Identity Model
*(Conditional: Ask ONLY if the source app contains auth logic, or if the user indicated auth is needed in target stack. If not needed, skip to Question 5).*
> - **[A] (Recommended):** [Modern secure session / JWT standard matching app type]
> - **[B] Built-in Framework Auth:** [Simplest built-in identity engine]
> - **[C] Third-Party Identity Provider:** [OAuth2 / Auth0 / Clerk integration]
> - **[D] Custom / Other:** Specify your preferred authentication model.

#### Question 5: Missing Parts & Alternates Resolution
*(Conditional: Ask ONLY if any missing parts were flagged in Step B1).*
> The AI presents the flagged missing parts from Step B1 and offers recommended target replacements:
> - **[A] (Recommended):** [Target replacements suggested by AI for each missing item]
> - **[B] Minimal / Deferred:** [Omit or defer until a subsequent milestone]
> - **[C] Custom / Other:** Specify how you would like each missing part replaced.

#### Question 6: UI & Frontend Modernization Strategy
*(Conditional: Ask ONLY if source application contains web views/frontend templates, or if the user explicitly requested a frontend. If the source is a pure backend API, worker, or CLI tool, default to Option [C] Headless API Only without prompting).*
> *"How would you like to handle the frontend and user interface in the target stack?*
> - **[A] 1-to-1 Native Re-creation (Source UI as Truth)**: Recreate the existing source application's screens, layout, and forms exactly as they are, ported natively to the target frontend framework.
> - **[B] Modernized UI Prototype (Bridge to Road U)**: Redesign and prototype the UI using a theme or AI-crafted design in Road U before writing backend code.
> - **[C] Headless API Only**: Skip UI entirely; build and verify backend endpoints only.
> - **[D] Custom / Other:** Specify your frontend strategy."*
>
> **Sub-Branch: If [B] (Prototype via Road U) is chosen:**
> Ask immediately:
> > *"When generating the UI prototype in Road U, should we:*
> > - **[1] Keep Exact Source Structure (Recommended)**: Use the source application's existing pages, forms, tables, and menu navigation structure as the strict blueprint.
> > - **[2] Restructure Pages & Navigation**: Propose updated/merged screens, menus, and updated layouts before generating the prototype."*

---

### Step B5: Avoid-List & Blueprint Compilation

1. **Contradiction Check:** Verify that selected libraries do not conflict (e.g., user selects Dapper but references Entity Framework migrations).
2. **Derive the Dynamic Avoid-List:**
   Every explicit architectural decision creates mandatory counter-default rules:
   - Dapper chosen $\rightarrow$ *"Do NOT use Entity Framework Core."*
   - Prisma chosen $\rightarrow$ *"Do NOT use TypeORM or raw pg drivers."*
   - Cookie Auth chosen $\rightarrow$ *"Do NOT use JWT tokens in headers."*
3. **Compile Foundational Artifacts:**
   - **`ANALYSIS.md`:** Populate following **`<SKILL_DIR>/templates/analysis.md.template`** (Source Environment, Data Entities, Routes/Screens, Hidden-Logic Rules, Legacy Test Suite Inventory, and Parity Baseline Catalog with concrete input/output samples and source tags).
   - **`CONVERSION_PLAN.md`:** Populate following **`<SKILL_DIR>/templates/conversion_plan.md.template`** (Target Architecture, Execution Rigor Mode, Missing Parts Resolution, Dynamic Avoid-List, 6-Phase Conversion Roadmap including Data Cutover).

---

## Blueprint Review & Approval Gate

Before presenting crossing options, present the compiled Migration Plan to the user:
> *"The Migration Analysis (`ANALYSIS.md`) and Conversion Plan (`CONVERSION_PLAN.md`) have been compiled.  
> - **Discovered Entities & Hidden Logic:** [Summary count of entities and critical hooks]  
> - **Parity Baseline Checks:** [Number of parity test cases captured]  
> - **Target Stack:** [Summary of agreed target technologies]  
> - **Execution Rigor Mode:** [Full Rigor / Reduced Rigor]  
> - **Dynamic Avoid-List:** [Summary of forbidden anti-patterns]  
> 
> Please review the blueprint. You may request adjustments to any phase or constraint, or provide your approval to proceed."*

Wait for explicit user approval before moving to Intersection Gate B-X.

---

## Intersection Gate B-X (The Road B Crossing)

Once the Blueprint is approved, present the crossing options:
> *"The migration blueprint is finalized and approved. How would you like to proceed?*
> - **[BX-1] Export Migration Prompt**: Generate the complete, standalone `MIGRATION_PROMPT.md` for execution by an external AI coding tool.
> - **[BX-2] Live Migration (Bridge to Road C)**: Initialize `.devop-process/`, scaffold the environment, and begin Phase 1 execution immediately.
> - **[BX-3] Prototype UI First (Bridge to Road U)**: Move directly to Road U to build the modernized HTML UI prototype before executing backend tasks."*

### Crossing Handoff Directives:

- **If [BX-1] is chosen:**
  1. Write the complete, self-contained migration prompt to `MIGRATION_PROMPT.md` in the project root containing all contents from `ANALYSIS.md` and `CONVERSION_PLAN.md`.
  2. Conclude the session.

- **If [BX-2] is chosen:**
  1. Bootstrap `.devop-process/` using the templates in `<SKILL_DIR>/templates/`.
  2. Write target stack, avoid-list, `Execution Rigor Mode`, and `UI Design Source of Truth` (Source Code Views path if Question 6 was [A], or None if [C]) to `.devop-process/constitution.md`.
  3. Write parity requirements to `.devop-process/spec.md`.
  4. Write conversion steps and `Approval Status: Approved` to `.devop-process/plan.md`.
  5. Seed `tasks.md`, `decisions.md`, and `progress-log.md` with initial Current Summaries.
  6. Execute the **Clean Baseline Verification Check**: Run the discovered legacy test suite or initial target scaffold test/build command. Record the exact terminal output and status under `Baseline Test Suite Status` in `.devop-process/constitution.md`.
  7. Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and jump directly to **Step C6 (Task Sizing & Risk Flagging)**.  
     *(Steps C1–C5 are bypassed because Constitution, Spec, Plan, and Baseline were established and approved in Road B).*

- **If [BX-3] is chosen:**
  1. Save `ANALYSIS.md` and `CONVERSION_PLAN.md` to the project root.
  2. Load `<SKILL_DIR>/roads/road-u-ui-prototype.md` using your environment's file reading tool and jump directly into **Step U0** with `origin: road-b`.
  3. Pass Section 3 (**Screen, View & Route Inventory**) from `ANALYSIS.md` directly into Road U Step U2 as the authoritative screen list, respecting whether the user chose to keep exact source structure or restructure pages.
