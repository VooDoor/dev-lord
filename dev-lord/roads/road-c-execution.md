# Road C: The Gated Execution Engine (Full Track)

> **Mission:** The core runtime implementation engine for medium-to-large features, architectural refactors, and migrations. Road C maintains file-persisted state inside `.devop-process/`, enforces bidirectional requirement traceability, mandates atomic tasks ($\le$ 5 files), requires real execution evidence, protects against architectural drift, and isolates changes on dedicated feature branches.

---

## Operating Flow

```text
[Step C1: Bootstrap & Kernel Integrity Invariant]
       │
       ▼
[Step C2: Constitution Gate] (Clean Baseline Test, Rigor Mode & UI Source of Truth)
       │
       ▼
[Step C3: Specify & Decompose] (Split Bundled Requests & Screen Recreations)
       │
       ▼
[Step C4: Clarify Gate] (HARD GATE: Structured Options A/B/C/D)
       │
       ▼
[Step C5: Architecture Plan Gate] (HARD GATE: Approval Record, Circuit Breaker Exit & Task 1 Assets)
       │
       ▼
[Step C6: Task Sizing & Risk Flagging] (Atomic <= 5 Files, Traceability to spec.md)
       │
       ▼
[Step C7: Pre-Flight Coverage Audit & 3-Way Stress Test] (Write Linked Tasks to spec.md)
       │
       ▼
[Feature Branch Adoption / Isolation (or Snapshot Mode)] ──► Log to progress-log.md
       │
       ▼
┌──────[Step C8: Atomic Implementation Loop (1 Task at a Time)]◄─────────────────┐
│             │                                                                  │
│             ▼                                                                  │
│      [Step C9: Real Verification & Failure Diagnostic]                         │
│             ├── Fail: Env Issue ──► Fix Env & Re-run Verification              │
│             ├── Fail: Code Logic (<3) ──► Targeted Revert, Inc Counter, Retry ─┤
│             ├── Fail: Code Logic (3rd) ──► CIRCUIT BREAKER ──► Loop to C5      │
│             └── Pass ──► Reset Counter, Dual-Summary Update, Atomic Commit     │
│                                                                                │
└─────────────[Step C10: Session Health Check: Degraded? -> Handoff]─────────────┘
       │  (All Tasks Verified & Healthy)
       ▼
[Step C11: Final Validation Gate] (All-YES Checklist including Spec & Plan Status)
       │
       ▼
[Step C12: Delivery, Rigor Framing & Disposition] (Git Branch vs Snapshot Mode)
```

---

## The 6-File State Kernel (`.devop-process/`)

State lives exclusively on disk inside `.devop-process/`, seeded from `<SKILL_DIR>/templates/`:
1. `constitution.md`: Tech stack, rigor mode, constraints, avoid-lists, and UI Design Source of Truth.
2. `spec.md`: Numbered requirements (R1, R2...), success criteria, requirement status, linked tasks, and clarification log.
3. `plan.md`: Architecture overview, affected files, dependencies, approval status, and plan approval record.
4. `tasks.md`: Task index, atomic task details, acceptance criteria, risk flags, and failure counters.
5. `decisions.md`: Append-only architectural rationale and pre-flight stress tests.
6. `progress-log.md`: Chronological execution log with Current Summary and real execution outputs.

*Rule:* Every file (except append-only logs) must open with a **Current Summary** section so state can be refreshed without loading entire files into context.

---

## Execution Steps & Hard Gates

### Step C1: Bootstrap & Kernel Integrity Invariant
1. Create `.devop-process/` if not present.
2. **Kernel Integrity Invariant:** Whenever Road C is entered (via C1, Road A [AX-2], Road B [BX-2], Lite Graduation, or Step 0 Option [2]):
   Assert that all 6 core files exist in `.devop-process/`. If any file is missing, seed it immediately from `<SKILL_DIR>/templates/` with its Current Summary header populated.
3. If bridging in from Road A, Road U, or Road B, carry forward the respective spec and constitution details. Work bridging from Road U always enters at Full Track (never Lite).
4. **Version Control Initialization & Repository Hygiene Guard:**
   - **Check Git Repo:** Test if the workspace is an active Git repository (`git rev-parse --is-inside-work-tree` or test for `.git` directory).
   - **If NOT a Git repo:**
     a. Check if the `git` command/tool is installed in the environment (`git --version`).
     b. **If `git` is installed:** Initialize a new repository (`git init`). Ensure `.devop-process/.snapshots/` and `.devop-process/scratch/` are added to `.gitignore`. Stage workspace files and commit the initial baseline: `git add . && git commit -m "chore: baseline initial workspace state"`. Inform user: *"Initialized Git repository with baseline workspace commit."*
     c. **If `git` is NOT installed:** Prompt the user for permission:
        > *"Git is required for atomic version control, verification reverts, and safe checkpoints, but the `git` command was not found in your environment. May I install `git` (e.g. via winget/brew/apt) and initialize a repository?*
        > - **[1] Yes, install git and initialize repository**
        > - **[2] No, use snapshot mode (directory-based backups)"*
        - If user approves: install `git` using the host package manager (`winget install --id Git.Git -e --source winget` on Windows, `brew install git` on macOS, `apt-get install -y git` on Linux), verify installation, execute `git init`, create baseline commit, and proceed with Git workflow.
        - If user refuses (or install fails): activate **Snapshot Mode**. Store pre-task snapshots under `.devop-process/.snapshots/task-NN/`, execute targeted reverts by copying back from snapshots, set `Active Working Feature Branch: N/A (Snapshot Mode)`, and bypass all Git commands.
   - **If already a Git repo:** Ensure `.devop-process/.snapshots/` and `.devop-process/scratch/` are in `.gitignore`. Core state files ARE tracked for cross-session state persistence.

### Step C2: Constitution Check (HARD GATE)
1. **Existing Code:** Inspect codebase to infer stack, conventions, and existing tests. Present draft to user for confirmation.
2. **Fresh Code:** Prompt user for stack, tools, and testing framework.
3. **Execution Rigor Mode:**
   Confirm execution capability:
   - **Full Rigor (Default):** Live terminal commands, automated tests, and verified CLI/HTTP outputs mandatory for every task.
   - **Reduced Rigor (Code-Review Only):** Live execution unavailable in environment; static code inspection, syntax lint, and checklist verification applied.
   Record in `constitution.md`.
4. **Clean Baseline Check:** Run existing test suites once now, before any task begins, and record the result (pass/fail and list of existing failures) in `constitution.md`. Pre-existing failures must not be counted against new tasks.
5. **UI Design Source of Truth:**
   Record in `constitution.md`:
   - **Source Type:** `[None (Headless / API / CLI) | Prototype ($PROTOTYPE_DIR) | Source Code Views (<path>)]`
   - **Reference Path:** e.g. `./ui-prototype/` or `legacy_app/app/views/`
   - **Parity Standard:** `[3-point Prototype Parity | 1-to-1 Source View Parity | N/A]`
   - **Rule:** The design source of truth is strictly READ-ONLY and must NEVER be modified or directly executed by runtime backend logic (unless the stack is plain static HTML/JS).
6. **Never invent conventions.** Obtain explicit user confirmation before proceeding.

### Step C3: Specify & Decompose
1. Draft or update `spec.md` with numbered requirement IDs (R1, R2...).
2. **Decomposition Rule:** If the user request bundles multiple disparate asks (e.g., *"fix auth, add dark mode, speed up search"*), decompose them into distinct requirements.
3. **UI Screen Requirements:**
   - When initially building an application from a UI prototype, create one requirement per prototype screen (e.g., *"Recreate screen `dashboard.html` natively in React matching `$PROTOTYPE_DIR/dashboard.html` layout, components, and fields"*).
   - When adding features or fixing bugs in an existing project, create requirements **only for the new or modified screens**.
4. **Prototype Staleness Check (Spec → Prototype Sync):**
   If a prototype exists and any spec change adds, removes, or modifies a screen, field, action, status, or flow:
   - Mark the affected prototype screen(s) as **STALE** in `spec.md` and `progress-log.md`.
   - Ask user: *"The spec change affects `<screen>.html`. Would you like to (a) detour into Road U to update the prototype first, or (b) proceed and treat the spec as authoritative for this screen?"*
   - **If (a):** Hand off to `<SKILL_DIR>/roads/road-u-ui-prototype.md` at **Step U2 → U7 → U7-R**, then **U13-S** (`origin: road-c-detour`). On return with `SYNCED`, remove STALE markers and resume.
   - **If (b):** Record decision in Clarification Log. Tasks for that screen reference the spec, not the stale prototype.
5. **Milestone Cap:** If requirements exceed 15–20 items, propose splitting into sequential milestones.

### Step C4: Clarify Gate (HARD GATE)
If any requirement has ambiguous scope, unspecified edge cases, or missing data shapes:
- **STOP IMMEDIATELY.**
- Compile an internal prioritized list of ambiguities.
- **Strict Sequential Questioning Protocol (ONE Question at a Time):**
  Present the highest-priority question to the user with structured options:
  > *"Question [N/Total]: [Clear statement of ambiguity]*
  > - **[A] Recommended:** [AI's contextual recommendation based on architecture/conventions]
  > - **[B] Minimal / Lighter:** [Simplest viable alternative]
  > - **[C] Robust / Complete:** [More resilient alternative handling further edge cases]
  > - **[D] Custom / Other:** Specify your preferred resolution."*
- Wait for user response before presenting the next question.
- On user selection, record decision in `spec.md` Section 6 (Clarification & Advisory Log) and `decisions.md`.
- Advance only when all blocking ambiguities are resolved.

### Step C5: Architecture Plan Gate (HARD GATE)
1. Draft implementation plan in `plan.md`:
   - Architecture overview & technology selections.
   - Affected files and new file creation list.
   - External dependencies and configuration changes.
   - Initial risk assessment and verification strategy per requirement.
2. Present plan to user with structured approval options:
   > *"Architecture and Implementation Plan compiled in `.devop-process/plan.md`. Please review:*
   > - **[A] Approved as written** → Proceed to Step C6 (Task Sizing)
   > - **[B] Adjust technical approach** → Specify adjustments
   > - **[C] Re-plan from scratch** → Return to Step C3"*
3. Wait for explicit user confirmation.
4. On approval, update `plan.md`:
   - `Approval Status: Approved`
   - Record `Approved By: User`, `Approval Timestamp: [ISO]`, and any specific caveats.
5. **Mid-Flight Road U Diversion:** If new UI screens are needed, offer the Road U detour (`<SKILL_DIR>/roads/road-u-ui-prototype.md` with `origin: road-c-detour`).
6. **Task 1: Theme Asset Pipeline Adoption (Mandatory when Bridging from Road U):**
   If the native application recreates screens from a UI prototype, Task 1 (`T001`) in `plan.md` and `tasks.md` **must be an Asset Pipeline Adoption Task**:
   Copy or integrate required compiled stylesheets, fonts, and icons from `$PROTOTYPE_DIR/assets/` into the target application's native asset directory (e.g., `public/assets/`, `src/assets/`, or framework static root), ensuring the native layout shell compiles and styles cleanly before screen components are built.
7. **Circuit Breaker Replanning Exit Protocol:**
   When entering Step C5 from a Circuit Breaker loopback (a task tagged `ARCHITECTURE-REVIEW` after 3 failures):
   - Analyze failure root causes logged in `decisions.md`.
   - Formulate architectural solution in `plan.md` and set `Approval Status: In Revision`.
   - Present resolution to user in Structured Option format and obtain explicit approval.
   - Upon approval, transition the task in `tasks.md`:
     - **Option 1 (Adjust existing task):** Update acceptance criteria/target files, reset status to `OPEN`, and reset `Consecutive Verification Failures: 0`.
     - **Option 2 (Decompose/Replace):** Tag failed task `SUPERSEDED` in `tasks.md`, add replacement sub-tasks (e.g. `T003a`, `T003b`), link to parent requirement in `spec.md`, and log rationale in `decisions.md`.
8. **Explicit Plan Approval Record:**
   Present plan to user. Upon explicit confirmation, update `plan.md`:
   - `Approval Status: Approved`
   - Record `Approved By: User`, `Approval Timestamp: [ISO]`, and any specific caveats.

### Step C6: Task Sizing, Risk-Flagging & Traceability
Decompose plan into atomic tasks in `tasks.md` (T001, T002...):
1. **Sizing Heuristic:** A task must touch **$\le$ 5 files** and have a single verifiable acceptance criterion.
2. **Traceability:** Link each task to parent requirement ID (R1, R2...).
3. **Risk-Flagging:** Tag `RISK: HIGH` if touching auth, payments, permissions, data deletion, or external APIs (requires mandatory test coverage and worst-case impact statement).
4. **Consecutive Verification Failures:** Initialize to `0`.

### Step C7: Pre-Flight Coverage Audit & 3-Way Stress Test (ANALYZE)
1. **Bidirectional Coverage:** Verify every requirement maps to at least one task, and write the generated task IDs into the **`Linked Tasks` column of `spec.md` Section 2**.
2. **3-Way Stress Test:** List at least 3 failure modes (null input, DB timeout, concurrent race), verify plan handles each, and record in `decisions.md`.

### Step C8: Atomic Implementation Loop & Revert Safety
Implement tasks strictly **one at a time**:
1. **Feature Branch Isolation & Adoption:**
   - **If Git is active:**
     - Inspect current branch: `git branch --show-current` (or `git rev-parse --abbrev-ref HEAD`).
     - **Branch Adoption:** If already on a dedicated feature branch (non-main / non-master), **adopt it**. Record the current branch in `progress-log.md` under `Active Working Feature Branch`.
     - If on `main` or `master` (or detached HEAD), create and checkout a dedicated feature branch on task 1: `git checkout -b feature/<name>`. Immediately record branch name in `progress-log.md` under `Active Working Feature Branch`.
   - **If in Snapshot Mode:**
     - Record `Active Working Feature Branch: N/A (Snapshot Mode)` in `progress-log.md`.
     - Before editing any task files, create a pre-task snapshot directory and backup target files:
       `mkdir -p .devop-process/.snapshots/<task-id>` and copy target files into it.
2. **Dirty Worktree Guard:**
   - **If Git is active:** Check `git status --porcelain`. If uncommitted changes exist:
     - Check if changes belong to current task. If unrelated files are modified, **halt and prompt user**:
       > *"Uncommitted changes detected in files outside current task: `[files]`. How would you like to handle them?*
       > - **[A] Commit current changes**
       > - **[B] Git stash changes**
       > - **[C] Keep files as-is**
       > - **[D] Discard changes** (`git restore -- <files>`)"*
   - **If in Snapshot Mode:** Compare declared target files against `.devop-process/.snapshots/<task-id>/` to verify only declared files are being modified.
3. **Subagent Task Worker Pattern (Context Hygiene):**
   - **Main Agent as PM:** Manages task queue in `tasks.md`, coordinates commits, logs execution, interfaces with user.
   - **Fresh Subagent per Task:** Dispatch strictly 1 task at a time to a fresh subagent with clean context. Pass task ID, acceptance criteria, target files ($\le 5$), design/parity references, and expected verification command.
   - **Subagent Contract:** Subagent edits target files, executes verification command, returns actual terminal evidence to Main Agent, and **terminates immediately**.
   - **PM Verification & State Update:** Main Agent reviews evidence against Step C9 standards. If verified, Main Agent marks task `DONE`, records atomic commit, updates state files, and advances.
   - **Single-Context Fallback:** If subagent dispatch is unavailable or unsupported in the host environment, the Main Agent implements the task directly in the current context: reads target files, applies minimal atomic edits, executes verification command, captures stdout/stderr, and proceeds to Step C9.
4. **YAGNI Enforcement:** Implement only what the task's acceptance criterion demands.

### Step C9: Real Verification & Failure Diagnostic (HARD GATE)
Verify task against acceptance criterion using `<SKILL_DIR>/references/verification-fallbacks.md`:
1. **Execution Rigor Check:**
   - **Full Rigor:** Live terminal commands / automated tests mandatory. High-risk tasks are never exempt.
   - **Reduced Rigor:** Perform thorough static code inspection against acceptance criteria, syntax lint check, and type audit. Status logged as `PASS (CODE-REVIEW ONLY)`.
2. **Pre-Existing Baseline Failure Isolation:** Cross-reference failure list against Step C2 baseline in `constitution.md`. Pre-existing failures do not fail the current task.
3. **Migration Parity Check (Road B Tasks):** Verify target output matches legacy output side-by-side using representative payload from `ANALYSIS.md` Section 5.
4. **UI Fidelity Parity Check (Road U Tasks):**
   For screens from `$PROTOTYPE_DIR/`, verify 3-point parity: component/field inventory, element IDs/names, layout hierarchy.
5. **Source UI Parity Check (Road B 1-to-1 Native Tasks):**
   For screens recreated from legacy source views, verify form controls, data table columns, and routes match legacy templates from `ANALYSIS.md` Section 3.
6. **Failure Diagnostic Protocol:**
   - **Env Failure:** Fix environment, re-run verification. Do not revert code.
   - **Code Logic Failure:**
     - Increment `- **Consecutive Verification Failures:** N` in `tasks.md`.
     - Cleanly revert **only the task's declared target files**: `git restore -- <target-files>` (if Git) or copy back from `.devop-process/.snapshots/<task-id>/` (if in Snapshot Mode). Never run blanket `git restore .`.
     - Record lesson in `decisions.md` and retry.
   - **Architecture Circuit Breaker:** On the **3rd consecutive code logic failure** on the same task: **HALT.** Do not attempt a 4th fix. Tag task `ARCHITECTURE-REVIEW`, record pattern in `decisions.md`, and loop back to **Step C5** for replanning exit protocol.
7. **Lightweight Quality Pass (Gate):**
   - Run syntax lint and typecheck on target files.
   - Check for obvious security flaws (hardcoded secrets, SQL injection, unvalidated inputs).
   - **Critical findings BLOCK task completion:** Must be resolved before proceeding to staging and commit.
8. **Passing Verification & Atomic Commit / Snapshot:**
   - Set `Consecutive Verification Failures: 0` in `tasks.md`.
   - Mark task `DONE` in `tasks.md`.
   - Update `tasks.md` Current Summary (`Completed: N`, `In-Flight: M`, `Next Task: T00Y`).
   - Append command and real output to `progress-log.md` table.
   - Update `progress-log.md` Current Summary (`Last Completed Task: T00X`, `Next Open Task: T00Y`, `Last Updated: [ISO]`).
   - If all tasks for requirement $R_x$ are `DONE`, mark $R_x$ `COMPLETED` in `spec.md` and update `spec.md` Current Summary.
   - **Atomic Staging & Commit (or Snapshot Finalization):**
     - **If Git is active:**
       ```bash
       git add <target-files> .devop-process/tasks.md .devop-process/progress-log.md .devop-process/decisions.md .devop-process/spec.md
       git commit -m "[T00X] <title> — verified: <method>"
       ```
     - **If in Snapshot Mode:** Copy verified target files to `.devop-process/.snapshots/<task-id>-verified/` to preserve a rollback baseline for subsequent tasks.

### Step C10: Session Degradation Check & Handoff Protocol
After completing each task, evaluate context health:
- If substantive turns exceed 40+ or large file dumps fill context: all state is already persisted in `Current Summary` files. Instruct user:
  > *"Session context is filling up. All state has been persisted to `.devop-process/`. Please start a fresh session and say: `continue [Next Task ID]`."*
  Halt execution cleanly.

### Step C11: Final Validation Gate (HARD GATE)
Before declaring completion, verify all items in the checklist (all must be YES):
- [ ] `.devop-process/` exists with all 6 core files up to date.
- [ ] Implementation plan in `plan.md` has recorded `Approval Status: Approved`.
- [ ] All requirements in `spec.md` are marked `COMPLETED` and match completed tasks.
- [ ] Every completed task has real execution evidence logged in `progress-log.md`.
- [ ] Bidirectional traceability holds (every requirement has a task).
- [ ] All avoid-list rules from `constitution.md` were strictly respected.
- [ ] Any Road B tasks passed Migration/Source Parity; Road U tasks passed UI Fidelity Parity.
- [ ] No unresolved Critical quality finding remains.
- [ ] **Spec Reconciliation:** If `MASTER_SPEC.md` exists in project root, reconcile it with completed requirements from `.devop-process/spec.md` to prevent documentation drift.

### Step C12: Delivery, Runnability Framing & Branch Disposition
1. **Honest Framing:**
   - Full Rigor: Deliver completed work described accurately as **"Verified & Runnable"**.
   - Reduced Rigor: Deliver completed work described accurately as **"Implemented & Code-Reviewed (Reduced Rigor — Not Live Verified)"**.
2. **Disposition Protocol:**
   - **If Git Repository:** Present branch closing options:
     > *"All tasks are verified and committed on `<branch-name>`. How would you like to proceed?*
     > - **[1] Merge to `main`/`master`**
     > - **[2] Prepare a Pull Request description**
     > - **[3] Keep the branch, decide later**
     > - **[4] Discard the branch** (Destructive confirmation required)"*
   - **If Git Unavailable (Snapshot Mode):** Present snapshot archival options:
     > *"All tasks are verified using local snapshots in `.devop-process/.snapshots/`. How would you like to proceed?*
     > - **[1] Keep final snapshot archive**
     > - **[2] Clean up snapshot directory**
     > - **[3] Export changed files diff summary**"*
