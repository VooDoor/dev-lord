# Road D: Session Resume Protocol (Zero-Context-Loss)

> **Mission:** Fast-boot re-entry into an ongoing project containing an active `.devop-process/` state directory. Road D re-establishes state without context-window amnesia or hallucinated continuations, reading only the essential summaries needed to recover the working branch, heal interrupted tasks, and execute the next task safely.

---

## Operating Flow

```text
[Detect .devop-process/ at Workspace Root]
       │
       ▼
[Step D1: Focused Bootstrap Reading] (constitution, progress-log summary, tasks index, plan approval)
       │
       ▼
[Step D2: Branch Reconciliation & Git Log Cross-Check] (Resolve interrupted commit & targeted revert)
       │
       ▼
[Step D3: Task Status Routing & Plan Approval Guard]
       ├── All Tasks DONE ──► Bridge to Road C (Step C11: Final Validation Gate)
       ├── Plan Unapproved / In Revision ──► Bridge to Road C (Step C5: Approval Gate)
       ├── T_next is ARCHITECTURE-REVIEW ──► Bridge to Road C (Step C5: Replanning Exit Protocol)
       ├── T_next is BLOCKED ──► Prompt User to Resolve External Blocker ──► Set OPEN
       └── T_next is OPEN / PENDING ──► Present Status Readback & Proceed to Step C8
```

---

## Resume Protocol Steps

### Step D1: Focused Bootstrap Reading (Context Window Protection)
To preserve the agent's context window, **DO NOT** re-read the entire codebase, past chat transcripts, or full historical log entries.
Read **only** these core files based on the active track:
1. `.devop-process/constitution.md` (if present, to load stack constraints, avoid-lists, rigor mode, version control mode, and UI design source of truth).
2. The **Current Summary** section of `.devop-process/progress-log.md` (to locate last completed task, active feature branch, version control mode, and track).
3. **If Full Track (`Current Track == "Full Track"`):**
   - The **Task Index table** of `.devop-process/tasks.md` (to identify `T_next` status, risk, and failure counter).
   - The **Current Summary** of `.devop-process/plan.md` (to verify `Approval Status`).
4. **If Express Lite Track (`Current Track == "Express Lite Track"`):**
   - Do NOT attempt to read `tasks.md` or `plan.md` (which do not exist on Lite track). Proceed directly to Step D2 and Guard 0-Lite.

---

### Step D2: Branch & Worktree Reconciliation (Interrupted State Recovery)
Before writing or executing any code:

1. **Version Control & Repository Mode Check:**
   - Test if the workspace is an active Git repository (`git rev-parse --is-inside-work-tree` or test for `.git` directory).
   - **If NOT a Git repository:**
     - If Git was never negotiated, execute the Git Negotiation Protocol (SKILL.md Operating Principle 17).
     - If running in **Snapshot Mode**:
       - Skip `git checkout`, `git log`, and `git status`.
       - **Snapshot Reconciliation:** Inspect `.devop-process/.snapshots/` and `progress-log.md`.
       - If `T_next` was interrupted mid-flight leaving dirty or unverified edits, restore strictly the target files from `.devop-process/.snapshots/<task-id>/`. Proceed directly to Step D3.
2. **Feature Branch Re-Attachment (Git Mode):**
   - Inspect `Active Working Feature Branch` from `progress-log.md` Current Summary.
   - **Branch Safety Guard:** If the recorded branch is `N/A`, `N/A (Lite)`, `None`, empty, or already checked out, **skip `git checkout`** and remain on the current branch.
   - Otherwise, cleanly checkout the recorded branch: `git checkout <feature-branch>`.
3. **Git Log vs. Task Reconciliation (Git Mode):**
   - Inspect last commit: `git log -1 --pretty=%B`.
   - If last commit matches `[T00X]` but `tasks.md` shows `T00X` as `OPEN` or `IN-PROGRESS`:
     Reconcile disk state immediately: mark `T00X` as `DONE` in `tasks.md`, update `progress-log.md` Current Summary, and advance to `T00X+1`.
4. **Interrupted / Dirty Worktree Recovery (Git Mode):**
   - Check worktree cleanliness: `git status --porcelain`.
   - **If uncommitted changes exist:**
     - Check whether modified files match `T_next` target files.
     - **If unrelated files are modified:** Ask the user:
       > *"Uncommitted changes detected outside the current task: `[files]`. How would you like to proceed?*
       > - **[A] Commit current changes**
       > - **[B] Git stash changes**
       > - **[C] Keep files as-is**
       > - **[D] Discard changes** (`git restore -- <files>`)"*
     - **If uncommitted changes belong to interrupted `T_next`:**
       - Run `T_next`'s verification check.
       - **If verification passes:** Finalize task per Step C9 (atomic commit, mark `DONE`, update summaries) and advance.
       - **If failing / unverified:** Revert **strictly the task's target files**: `git restore -- <target-files>`. Never run blanket `git restore .`.

---

### Step D3: Task Status Routing & User Readback

Inspect `Current Track` from `progress-log.md`, `Approval Status` from `plan.md`, and the status of `T_next` in `tasks.md`:

1. **Guard 0: In-Flight UI Prototype Detour Guard**
   Check if `$PROTOTYPE_DIR/.prototype-state.json` exists locally (retrieving `$PROTOTYPE_DIR` from `constitution.md` under `UI Design Source of Truth: Reference Path`, or defaulting to `./ui-prototype/`).
   If it exists and contains pending screens (`pendingScreens.length > 0` or status in-flight):
   - Readback to user:
     > *"Resuming from `.devop-process/` — Detected in-flight UI Prototype detour at `[$PROTOTYPE_DIR]`. Next pending screen: `[Screen Name]`. Loading Road U to continue prototype generation..."*
   - Load `<SKILL_DIR>/roads/road-u-ui-prototype.md` using your environment's file reading tool and jump directly to **Step U7 (Hierarchical Subagent Screen Generation Loop)**.

2. **Guard 0-Lite: Active Express Lite Track Resume**
   If `progress-log.md` Current Summary indicates `Current Track: Express Lite Track`:
   - Readback to user:
     > *"Resuming from `.devop-process/` on Express Lite Track. Last recorded action: `[Last Completed Task]`. Routing to Road C-Lite to continue..."*
   - Load `<SKILL_DIR>/roads/road-c-lite.md` using your environment's file reading tool and proceed directly to **Step L2 (Implement)** or **Step L3 (Real Verification)**.

3. **Guard 1: Early-Lifecycle & Incomplete Planning Guard (Full Track)**
   If `tasks.md` contains no active tasks or is unpopulated:
   - If `plan.md` exists with content but is not approved → Route to **Step C5 (Architecture Plan Gate)**.
   - If `spec.md` exists with requirements but `plan.md` is empty → Route to **Step C4 (Clarify Gate)** or **Step C5**.
   - If `spec.md` is empty or incomplete → Route to **Step C3 (Specify & Decompose)**.
   - Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool to resume the appropriate planning step.

4. **Guard 2: Plan Approval Guard (Full Track)**
   If `plan.md` shows `Approval Status: Pending Approval` or `In Revision`:
   - Readback to user:
     > *"Resuming from `.devop-process/` — The implementation plan is currently `[Pending Approval / In Revision]`. Routing to Step C5 to review and obtain approval before implementation begins..."*
   - Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and jump directly to **Step C5 (Architecture Plan Gate)**.

5. **Scenario 1: All Tasks Complete (`DONE`)**
   - Readback to user:
     > *"Resuming from `.devop-process/` — All tasks in `tasks.md` are marked DONE. Proceeding to Final Validation Gate..."*
   - Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and jump directly to **Step C11 (Final Validation Gate)**.

6. **Scenario 2: `T_next` is Tagged `ARCHITECTURE-REVIEW`**
   - Readback to user:
     > *"Resuming from `.devop-process/` — Task `[T_next: Title]` is tagged ARCHITECTURE-REVIEW due to 3 consecutive verification failures. Routing to Step C5 to resolve architecture via replanning exit protocol..."*
   - Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and jump directly to **Step C5 (Circuit Breaker Replanning Exit Protocol)**.

7. **Scenario 3: `T_next` is Tagged `BLOCKED`**
   - Inspect `- **Blocked Reason:**` in `tasks.md` under `T_next`.
   - Readback to user:
     > *"Resuming from `.devop-process/` — Task `[T_next: Title]` is currently BLOCKED.  
     > - **Reason:** `[Blocked Reason from tasks.md]`  
     > 
     > Please provide the necessary credentials, environment variables, or resolution so this task can proceed."*
   - Once resolved by the user, update `tasks.md` (`Status: OPEN`, `Blocked Reason: None`) and proceed to Scenario 4.

8. **Scenario 4: `T_next` is `OPEN` or `PENDING`**
   - Present standardized readback:
     > *"Resuming from `.devop-process/` on branch `<feature-branch>`.  
     > - **Last Completed:** `[T_last: Title]`  
     > - **Next Task:** `[T_next: Title]` (`[Risk Level]`)  
     > - **Acceptance Criterion:** `[Acceptance Criterion]`  
     > 
     > Starting implementation of `[T_next]`..."*
   - Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and proceed directly into **Step C8 (Atomic Implementation Loop)** targeting `T_next`.
