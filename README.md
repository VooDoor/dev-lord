# Dev-Lord (v2.0.0)
### Master Software Engineering Lifecycle Wizard & Orchestrator

**Dev-Lord** is a production-grade, framework-agnostic AI agent skill designed to guide software projects from fuzzy concepts or legacy codebases to verified, runnable software. It eliminates hallucinations, architectural drift, context amnesia, and unverified code edits through a file-persisted, gated lifecycle across **5 interconnected roads**.

---

## 🌟 The 5 Roads of Dev-Lord

```text
               ┌──► [Road A: Greenfield Spec Architect] ────┐
               │                                            ▼
[User Request] ┼──► [Road U: UI Prototype Architect] ─────► [Road C: Execution Engine]
   (Boot Q0)   │    (Theme Ingestion vs. AI-Crafted)        ▲  (Full & Lite Tracks)
               ├──► [Road B: Re-Platform & Migration] ──────┘
               │
               └──► [Road D: Session Resume & Recovery] ──► (Direct fast-boot into Road C)
```

| Road | Playbook | Purpose & Core Capabilities |
| :--- | :--- | :--- |
| **Road A** | [`roads/road-a-greenfield-spec.md`](dev-lord/roads/road-a-greenfield-spec.md) | **Idea to Master Spec:** Passive ingestion mode accumulates thoughts silently; Minimum Viable Concept guardrail prevents empty drafts; sequential **one-at-a-time** advisory pressure-tests architectural blindspots; conditional Section 3 for UI vs Headless/CLI apps; exports a clean 6-section spec (`MASTER_SPEC.md`). |
| **Road U** | [`roads/road-u-ui-prototype.md`](dev-lord/roads/road-u-ui-prototype.md) | **Liberated UI Prototyping (Dual Mode):**<br>• **Mode A (Theme-Based)**: Ingests any local theme via an automated subagent into project-specific `.devop-process/theme_report.md` & `theme-digest.md`.<br>• **Mode B (AI-Crafted)**: Builds modern, high-fidelity prototypes using AI frontend engineering knowledge (CSS variable tokens, responsive grid, card portlets, Lucide/Tabler SVG icons) with **zero** theme folder required.<br>• Serves as an immutable **read-only design source of truth** with state persistence (`.prototype-state.json`) and origin-aware crossing gates. |
| **Road B** | [`roads/road-b-replatform.md`](dev-lord/roads/road-b-replatform.md) | **Zero-Regression Migration:** Pre-flight manifest health check (detects missing configs without failing); scans for hidden logic (RLS, model callbacks, triggers); conditional Auth and UI questions; captures concrete input/output **parity baseline samples**; compiles dynamic avoid-lists and propagates Execution Rigor Mode. |
| **Road C** | [`roads/road-c-execution.md`](dev-lord/roads/road-c-execution.md)<br>[`roads/road-c-lite.md`](dev-lord/roads/road-c-lite.md) | **Gated Implementation Engine:** Idempotent Kernel Integrity Invariant across all entries; atomic task sizing ($\le 5$ files); dedicated feature branch adoption / checkout; subagent task worker pattern; mandatory Task 1 Asset Adoption when bridging from Road U; dual-summary atomic updates (`progress-log.md` & `tasks.md`); targeted file restores (`git restore -- <files>` or snapshot restore); 3-failure Circuit Breaker with Replanning Exit Protocol; Execution Rigor Mode branching (Full vs Reduced); and an express **Lite track** with pre-edit baselines, atomic commits, and auto-graduation. |
| **Road D** | [`roads/road-d-resume.md`](dev-lord/roads/road-d-resume.md) | **Zero-Context-Loss Resume:** Sub-second fast boot reading only 4 summary files; Plan Approval Guard; automatic git feature branch re-attachment; `git log -1` vs `tasks.md` reconciliation for interrupted commits; targeted file recovery; dirty worktree guards; in-flight UI detour recovery (Guard 0); Express Lite Track resume (Guard 0-Lite); and clean separation of `BLOCKED` (waiting on user/credentials) vs `ARCHITECTURE-REVIEW` (replanning). |

---

## ⚡ Key Architectural Invariants & Philosophy

1. **Hierarchical Subagent Delegation (Principle 15)**:
   The Main Agent operates as the persistent Project Manager (PM) and sole user guide. Subagents are dispatched for focused, isolated jobs (theme ingestion, individual screen generation, atomic task implementation). Subagents complete their scope, return factual evidence to the Main Agent, and **terminate immediately**. The Main Agent reviews evidence, updates disk state, and guides the user forward.
2. **The Liberated Road U Architecture**:
   Dev-Lord is no longer tied to any single pre-bundled theme. It can digest any local commercial or open-source theme dynamically, or craft complete, responsive modern UI prototypes from scratch using AI design system knowledge when no theme is present.
3. **Execution Over Assertion & Proportional Rigor**:
   Self-review is not verification. Tasks cannot be marked `DONE` without live terminal commands, test suites, or live HTTP requests (Full Rigor). If terminal access is restricted, Reduced Rigor applies static code inspection, type auditing, and delivers transparently framed code as *"Implemented & Code-Reviewed"*.
4. **Targeted Revert Safety**:
   Blanket `git restore .` is forbidden. In the event of a verification failure or interrupted session recovery, Dev-Lord reverts **only the task's declared target files**, strictly protecting external user edits. Dirty worktrees trigger interactive prompts rather than silent overwrites.
5. **Atomic Dual-Summary State Sync & Kernel Invariant**:
   State lives exclusively on disk in `.devop-process/`. Every completed task atomically synchronizes both `progress-log.md` and `tasks.md` Current Summaries alongside code commits. The Kernel Integrity Invariant ensures all 6 core state files are seeded and maintained across every road transition.
6. **Strict Dynamic Avoid-Lists**:
   Architectural choices automatically create counter-default rules in `constitution.md` (e.g., *Fastify chosen $\rightarrow$ forbid Express middleware; Dapper chosen $\rightarrow$ forbid EF Core*) to prevent AI hallucinations.
7. **Read-Only UI Design Source of Truth**:
   Screens generated by Road U in `$PROTOTYPE_DIR` are permanent visual specifications. Target frameworks (React, Django, .NET) recreate them natively, never modifying or wiring backend code directly into the prototype files. Task 1 (`T001`) in Road C adopts prototype stylesheets into the native asset pipeline.
8. **Dynamic Version Control & Snapshot Mode (Principle 17)**:
   Dev-Lord operates with first-class safety in both Git and non-Git repositories. If a workspace is not a Git repository, it detects whether `git` is installed and auto-initializes a repository with a clean baseline commit. If `git` is absent, it asks permission to install it; if refused, it seamlessly activates **Snapshot Mode** (`.devop-process/.snapshots/`), ensuring safe rollbacks and targeted restores without requiring Git.

---

## 📁 Directory Structure

```text
dev-lord/
├── SKILL.md                          # Master router (v2.0.0: intent fast-path, artifact discovery, Q0 [1]..[5], git negotiation)
├── roads/
│   ├── road-a-greenfield-spec.md     # Greenfield spec architect (conditional Section 3, AX-1..AX-3)
│   ├── road-u-ui-prototype.md       # Liberated UI prototype (Dual Mode, theme ingestion subagent, state persistence, spec sync)
│   ├── road-b-replatform.md          # Re-platforming (hidden-logic discovery, parity samples, baseline test, BX-1..BX-3)
│   ├── road-c-execution.md           # Gated full-track execution loop (kernel invariant, dual summary, branch adoption, snapshot fallback)
│   ├── road-c-lite.md                # Express 1-3 file track with baseline check, atomic commit & graduation protocol
│   └── road-d-resume.md              # Zero-context-loss session resume (Guard 0 UI detour, Guard 0-Lite resume, git log reconciliation)
├── references/
│   ├── hidden-logic-patterns.md      # Framework checklists (RLS, signals, callbacks, AOP)
│   └── verification-fallbacks.md     # 4-tier verification hierarchy & baseline test isolation
└── templates/
    ├── constitution.md.template      # Tech stack, rigor mode, avoid-list, version control mode, UI source of truth
    ├── spec.md.template              # Standardized 6-section requirements specification with linked tasks & prototype sync log
    ├── plan.md.template              # Architecture plan, dependencies, approval status & approval record
    ├── tasks.md.template             # Atomic task index, risk flags, failure counters & blocked reasons
    ├── analysis.md.template          # Source environment, schema, hidden logic, parity catalog with baseline source tags
    ├── conversion_plan.md.template   # Target stack, execution rigor mode, missing parts, 6-phase roadmap
    ├── decisions.md.template         # Pre-flight stress tests & architectural rationale
    ├── progress-log.md.template      # Chronological verification log with standardized Current Summary header & VC mode
    ├── theme_report.template.md      # Standardized 10-section theme reverse-engineering guide
    └── theme-digest.template.md       # Standardized theme digest interface template
```

---

## 🚀 How to Use Dev-Lord

### 1. Activating the Skill
Trigger Dev-Lord naturally in your prompt based on your goal:
- **Have an idea?** $\rightarrow$ *"I have an idea for an app, let's spec it out"* (Launches **Road A**)
- **Want to prototype screens?** $\rightarrow$ *"Prototype the UI for my app"* (Launches **Road U**)
- **Have legacy code to migrate?** $\rightarrow$ *"Help me migrate this Rails app to Node/TypeScript"* (Launches **Road B**)
- **Want to build or fix code right now?** $\rightarrow$ *"Implement feature X"* or *"Fix bug Y"* (Launches **Road C / C-Lite**)
- **Resuming previous work?** $\rightarrow$ *"Resume working on this project"* or *"continue T003"* (Launches **Road D**)

### 2. Boot Mode (Step 0)
When Dev-Lord initializes, it executes an intelligent boot triage:
- **Intent Fast-Path:** If your prompt contains a clear, unambiguous request (e.g. *"continue task T002"* or *"prototype UI from ./theme"*), it confirms in one line and dispatches immediately without displaying menus.
- **Active State Detection:** If `.devop-process/` exists, it offers to resume work (**Road D**), add a feature/bug (**Scope Check Q1** $\rightarrow$ Lite or Full Road C), or start a fresh activity.
- **Workspace Artifact Auto-Discovery:** If `.devop-process/` is absent, it inspects your workspace for existing `ui-prototype/`, `MASTER_SPEC.md`, `ANALYSIS.md`, or `MIGRATION_PROMPT.md` blueprints and offers direct continuation.
- **Wizard Question Q0:** If starting fresh with no artifacts, it presents the disambiguated `[1]` through `[5]` menu:
  - `[1] Greenfield Project / New Idea (Road A)`
  - `[2] Re-Platform / Migrate Existing App (Road B)`
  - `[3] Implement, Fix, or Refactor (Road C / Lite)`
  - `[4] Ready Spec Implementation (Road C Step C2)`
  - `[5] Prototype UI (Theme or AI-Crafted) (Road U Step U0)`

### 3. State Persistence (`.devop-process/` & `$PROTOTYPE_DIR`)
During execution, Dev-Lord maintains all requirements, decisions, tasks, and verification logs in `.devop-process/` in your workspace (and prototype configuration in `$PROTOTYPE_DIR/.prototype-state.json`). You can pause, close your terminal, or restart your agent session at any moment with **zero loss of context**.

---

### 🗺️ Lifecycle State Machine Diagram
The complete lifecycle and state machine diagram is maintained in [`dev-lord-diagram.mmd`](dev-lord-diagram.mmd). It features visual color coding across every subsystem:

- 🟢 **Road A (Greenfield)**: Deep Emerald (`#064e3b` / `#10b981`)
- 🟣 **Road U (UI Prototype - Dual Mode)**: Fuchsia / Purple (`#701a75` / `#d946ef`)
- 🟡 **Road B (Migration & Parity)**: Warm Amber (`#78350f` / `#f59e0b`)
- 🔵 **Road C (Full Track Execution)**: Sky Blue (`#0c4a6e` / `#0284c7`)
- 🔷 **Road C-Lite (Express Track)**: Teal (`#134e4a` / `#14b8a6`)
- ⚪ **Road D (Session Resume)**: Stone Slate (`#1c1917` / `#78716c`)
- 🔴 **Safeguards & Circuit Breakers**: Crimson Red (`#7f1d1d` / `#ef4444`)
- 🟣 **Intersection Gates & Crossings**: Indigo (`#312e81` / `#6366f1`)
- ❇️ **Deliverables & Milestones**: Forest Green (`#14532d` / `#22c55e`)
