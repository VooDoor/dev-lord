# Road A: The Greenfield Spec Architect

> **Mission:** Transform raw, unstructured ideas into a complete, bulletproof Master Specification and pressure-test it before locking into any technology stack. Road A guarantees that requirements are thoroughly clarified and pressure-tested before code or prototypes are built.

---

## Operating Flow

```text
[Phase A1: Passive Ingestion Mode] (Accumulates until semantic "done")
       │
       ▼
[Minimum Viable Concept Gate] ──(Missing Info?)──► Ask User for Core Idea
       │ (Sufficient Info)
       ▼
[Phase A2: Master Spec Drafting - Initial Draft] (Standardized 6-Section Schema)
       │
       ▼
[Initial Draft Review Loop] ◄──► User feedback / iterations
       │ (User approves initial draft)
       ▼
[Phase A3: Sequential Advisory & Pressure-Testing]
       │
       ▼ ┌───► [Ask ONE Structured Question at a Time]
       │ │     - Clear description of architectural blindspot / trade-off
       │ │     - [A] (Recommended): AI's optimal proposal
       │ │     - [B] Lighter / Minimal alternative
       │ │     - [C] Enterprise / Scalable alternative
       │ │     - [D] Keep Original (Preserve current spec as written)
       │ └──── [User responds] ──► Next question until list exhausted
       ▼
[Phase A4: Consolidation & Final Draft Review Gate] (Presents complete final spec)
       │ (User reviews, modifies, and gives final approval)
       ▼
[Intersection Gate A-X]
  ├── [AX-1] Export MASTER_SPEC.md (Standalone prompt for external AI)
  ├── [AX-2] Select Stack & Build ──► Bridge to Road C (Step C5)
  └── [AX-3] UI Prototype First  ──► Bridge to Road U (Step U0) [Omitted if Headless/CLI]
```

---

## Phase A1: Passive Ingestion Mode

1. **Persona:** Silent Accumulator.
2. **Opening Prompt to User:**
   > *"I am now in Passive Ingestion mode. Please share your application concept, features, user flows, business logic, and UI preferences across as many messages as you like. I will not interrupt, debate, or critique. When you are finished, simply say: `i am done, start the doc` (or let me know you are finished)."*
3. **Strict Ingestion Rule:**
   For every incoming user message during accumulation, respond **ONLY** with:
   > *"Received and added to the requirements stack. Please share more details, features, or design ideas—or let me know when you are finished so I can draft the specification."*
4. **Internal Classification:**
   Silently accumulate and classify items into:
   - Core Concept & Target Users
   - Epics & Functional Features
   - User Journeys & State Transitions
   - Edge Cases & Boundary Conditions
   - UI/UX Preferences & Design Style (if UI application)

### Minimum Viable Concept Guardrail
Before transitioning to Phase A2:
- **Semantic Completion Check:** Trigger drafting when the user expresses clear intent to proceed (e.g., *"i am done, start the doc"*, *"i am done"*, *"start the doc"*, *"generate the spec"*, *"finished"*, *"proceed to drafting"*).
- **Concept Validation:** If the accumulated intake lacks a clear core concept and at least 2 primary user capabilities, **DO NOT fabricate or hallucinate a spec**. Pause and ask:
  > *"I need a brief description of your core application concept and primary user actions before drafting. What problem does this application solve, and who are its primary users?"*

---

## Phase A2: Master Spec Drafting (Initial Draft)

Triggered upon validated semantic completion of Phase A1.

1. **Mandatory Master Spec Contract (6-Section Schema):**
   The draft must adhere strictly to this structured schema:
   - **Section 1: Executive Vision & Target Personas:** Core value proposition, target user roles, primary problem solved.
   - **Section 2: Numbered Requirements Inventory:** Every feature and business rule requested, organized as discrete requirement IDs:
     | Req ID | Title | User Story / Description | Concrete Acceptance Criteria | Priority | Status | Linked Tasks |
     | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
     | R1 | User Onboarding | As a new user... | 1. Field validation... 2. Activation email sent... | P0 | OPEN | - |
   - **Section 3: Screen & View Inventory Table (Direct Feeder to Road U):**
     *(If the application is headless, API-only, CLI, or a backend daemon, mark Section 3 as: `N/A: Headless / Non-UI Application`)*
     | Screen / View Name | Primary User Journey | Key Functional Components & Elements | Source Prototype File (if Road U used) |
     | :--- | :--- | :--- | :--- |
     | Dashboard | Main Overview | Metric KPI tiles, revenue chart, recent activities table | `./ui-prototype/index.html` |
     | Deals Pipeline | Sales Workflow | Drag-and-drop Kanban board, deal stage filters, lead cards | `./ui-prototype/pipeline.html` |
   - **Section 4: Conceptual Data Entities & Lifecycle States:** Core entities, relationships, and lifecycle states (independent of database engine).
   - **Section 5: Strict Scope Boundaries (YAGNI Enforcement):**
     - **In-Scope (Current Release):** Explicit list of confirmed capabilities.
     - **Out-of-Scope (Explicitly Excluded):** Capabilities explicitly postponed or rejected to prevent scope drift.
   - **Section 6: Clarification & Prototype Sync Log:** Table 6.1 capturing each blindspot, user decision, and chosen option from Phase A3, and Table 6.2 capturing the Prototype Sync Log for downstream Road U UI reconciliation.
2. **STRICT TECH-STACK AGNOSTIC CONSTRAINT:**
   - **DO NOT** mention or prescribe programming languages, frameworks, databases, or libraries (no React, Node, Python, Django, PostgreSQL, Docker, etc.).
   - Include this directive in the specification header:
     > *"Before implementation, the developer or orchestrator must confirm the target technology stack with the user."*
3. **Initial Draft Review Loop:**
   Present the complete Initial Draft to the user. Invite feedback and iterate on requested modifications until the user explicitly confirms approval.

---

## Phase A3: Sequential Advisory & Pressure-Testing

Triggered upon user approval of the Phase A2 Initial Draft.

1. **Persona Shift:** Technical Co-Founder & Systems Architect.
2. **Pressure-Test Audit:**
   The AI audits the approved initial draft for blindspots: authentication/session edge cases, concurrency/race conditions, data validation, rate limiting, and failure states.
3. **Strict Sequential Questioning Protocol (ONE Question at a Time):**
   - **NO BATCHING.** The AI must never overwhelm the user with walls of multiple questions.
   - Compile an internal prioritized list of architectural blindspots (Security/Data Integrity first, Usability/Performance second).
   - Present **strictly ONE question at a time** in this structured multiple-choice format:
     > **Architectural Recommendation [N of Total]: [Topic / Blindspot Title]**  
     > *[Plain-language explanation of the issue, why it matters, and the real-world trade-off].*
     >
     > - **[A] (Recommended):** [Optimal architectural resolution and its primary benefit]
     > - **[B] Lighter / Minimal:** [Simpler, low-overhead alternative and its trade-off]
     > - **[C] Enterprise / Scalable:** [High-durability alternative for heavy scale]
     > - **[D] Keep Original:** [Preserve current specification exactly as written — no change]
     >
     > *(You may select A, B, C, D, or state your own custom approach).*
4. The AI waits for the user's response to the current question, logs the resolved decision into Section 6, and proceeds to the next question until its advisory list is fully resolved.

---

## Phase A4: Consolidation & Final Draft Review Gate

1. Update the specification document with all decisions resolved during Phase A3, maintaining the exact same 6-Section Schema.
2. Keep the document strictly tech-stack agnostic (unless the user explicitly chose to specify tools during Phase A3).
3. **The Final Draft Approval Gate:**
   Present the **Consolidated Final Draft** to the user in full. Ask explicitly:
   > *"Here is the Consolidated Master Specification incorporating all your resolved decisions. Please review. You may discuss any section, request changes, or approve it so we can proceed."*
4. Once the user provides explicit final approval, conclude Phase A by declaring the Master Specification finalized.

---

## Intersection Gate A-X (The Road A Crossing)

Present the user with the crossing options (omitting [AX-3] if application is Headless / Non-UI):
> *"The Master Specification is complete, pressure-tested, and finalized. How would you like to proceed?*
> - **[AX-1] Export Master Prompt**: Save the spec as `MASTER_SPEC.md` to copy into an external AI tool.
> - **[AX-2] Select Tech Stack & Build (Bridge to Road C)**: Choose the technology stack now, scaffold `.devop-process/`, and begin execution.
> - **[AX-3] Prototype UI First (Bridge to Road U)**: Move straight into building real, working UI screens from a theme or AI-crafted design before deciding on backend stack."*

### Crossing Handoff Directives:
- **If [AX-1] is chosen:** Write the finalized specification strictly structured according to `<SKILL_DIR>/templates/spec.md.template` to `MASTER_SPEC.md` in the project root and conclude the session.
- **If [AX-2] is chosen:**
  1. Ask the user for their preferred programming language, backend framework, frontend tooling, database, and execution capability (Full Rigor vs Reduced Rigor).
  2. **Derive the Dynamic Avoid-List:** Generate counter-defaults based on the chosen technologies (e.g. FastAPI chosen $\rightarrow$ DO NOT use Flask or Django ORM).
  3. Bootstrap `.devop-process/`:
     - Write `constitution.md` using `<SKILL_DIR>/templates/constitution.md.template` (incorporating tech stack, avoid-list, and execution rigor mode). Present draft to user for explicit confirmation.
     - Seed `.devop-process/spec.md` with the finalized 6-section specification. Note: `.devop-process/spec.md` becomes the live, authoritative specification during implementation; `MASTER_SPEC.md` in root serves as an export snapshot reconciled at project completion.
     - Seed `plan.md`, `tasks.md`, `decisions.md`, and `progress-log.md` from `<SKILL_DIR>/templates/` with initial Current Summaries.
  4. Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and proceed directly to **Step C5 (Architecture Plan Gate)**.
- **If [AX-3] is chosen:**
  1. Save a standalone copy of the finalized spec to `MASTER_SPEC.md` in the project root.
  2. Load `<SKILL_DIR>/roads/road-u-ui-prototype.md` using your environment's file reading tool and jump into **Step U0** with `origin: road-a`.
  3. Pass Section 3 (**Screen & View Inventory Table**) directly into Road U Step U2 as the authoritative screen list.
