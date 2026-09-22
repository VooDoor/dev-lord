# Road C-Lite: Express Execution Track

> **Mission:** A lightweight execution path for small, well-scoped bug fixes, micro-features, or tweaks touching **roughly 1–3 files** with no architectural, security, or database schema implications. Road C-Lite enforces the non-negotiables of the orchestrator (real verification, targeted revert safety, and factual logging) while skipping formal requirement IDs and heavyweight planning documents.

---

## Operating Flow

```text
[Step L1: Quick Scope Confirmation, Constitution Check & Escalation]
       │
       ▼
[Step L2: Implement with Targeted Snapshot / Git Safety]
       │
       ▼
[Step L3: Real Execution Verification (HARD GATE)]
       ├── Fails / Ambiguity / Complexity Discovered ──► GRADUATION PROTOCOL (to Full Road C)
       └── Passes Real Execution Check
             │
             ▼
[Step L4: Log to progress-log.md with Populated Summary Header]
             │
             ▼
[Step L5: Report & Delivery with Real Execution Evidence]
```

---

## Execution Steps

### Step L1: Quick Scope Confirmation & Escalation Check
1. State in 1–2 sentences your precise understanding of the change and the exact target files:
   > *"Running in Road C-Lite. Modifying `[file1, file2]` to `[specific outcome]`."*
2. **Constitution Check:**
   If `.devop-process/constitution.md` exists locally, read its **Avoid-List** and **Coding Conventions** to ensure the proposed edit strictly respects existing project boundaries.
3. **Immediate Escalation Check:**
   If the change involves authentication, payments, permissions, data deletion, public API contracts, new database migrations, or is ambiguous in scope:
   **ESCALATE IMMEDIATELY.** Halt Lite execution and route directly to **Full Road C, Step C3 (Specify & Decompose)**.
4. **Pre-Edit Baseline Check:**
   If the project has an automated test suite, run the relevant target test file once *before* editing to capture pre-existing test failures. Any pre-existing failures are noted and must not be attributed to the current fix or trigger a false graduation.

### Step L2: Implement with Targeted Revert Safety
1. **Worktree Cleanliness & Version Control Check:**
   - If Git repository is active: Ensure clean working tree (`git status --porcelain`). If unrelated files are dirty, ask user before proceeding.
   - If Git is not initialized: Follow the Git Negotiation Protocol (SKILL.md Operating Principle 17). If user chose Snapshot Mode, copy target files to `.devop-process/.snapshots/lite/` before modifying code.
2. Apply the atomic change directly to declared target files only. No speculative edits or unrequested additions.

### Step L3: Real Verification (HARD GATE — Same Standard as C9)
Verify the modification against real execution:
1. Run automated test suite (`npm test`, `pytest`, etc.).
2. If tests unavailable, run live CLI command, HTTP curl, or DOM check per `<SKILL_DIR>/references/verification-fallbacks.md`.
3. **Reading code back to yourself DOES NOT count as verification.**
4. If the fix fails or reveals deeper coupling:
   - Cleanly revert **only the target files**: `git restore -- <target-files>` (if Git) or restore from `.devop-process/.snapshots/lite/`.
   - Trigger the Graduation Protocol below.

### Step L4: Lightweight Log & Atomic Commit
1. **Progress Logging:**
   If `.devop-process/progress-log.md` is absent or missing its header:
   Seed it from `<SKILL_DIR>/templates/progress-log.md.template`, setting:
   - `Current Track: Express Lite Track`
   - `Active Working Feature Branch: N/A (Lite)`
   - `Last Completed Task: LITE: [summary of change]`
   - `Last Updated: [Current ISO Timestamp]`

   Append the chronological entry to the table:
   ```markdown
   | YYYY-MM-DD HH:MM | LITE | path/to/file.ext | `npm test -- auth.test.js` -> 5 passed | PASS |
   ```
2. **Atomic Commit / Checkpoint:**
   - **If Git is active:** Stage modified target files and log, then commit:
     ```bash
     git add <target-files> .devop-process/progress-log.md
     git commit -m "lite: [summary of change] — verified: <method>"
     ```
   - **If in Snapshot Mode:** Copy verified target files to `.devop-process/.snapshots/lite-verified/`.

### Step L5: Report & Delivery
Deliver the completed work to the user:
- Concise summary of lines changed.
- The exact verification command executed and its actual terminal output.

---

## The Lite-to-Full Graduation Protocol

If a task initially classified as Lite expands beyond 3 files, reveals architectural coupling, or touches security/database logic mid-flight:

1. **Halt Lite Execution:** Do not continue patching without formal tracking.
2. **Preserve History:** Keep all prior entries in `.devop-process/progress-log.md`.
3. **Kernel Completeness Rule:**
   - Inspect repository conventions to seed `.devop-process/constitution.md` using `<SKILL_DIR>/templates/constitution.md.template`.
   - Seed all remaining kernel files (`spec.md`, `plan.md`, `tasks.md`, `decisions.md`) from `<SKILL_DIR>/templates/` with initial Current Summaries.
4. **Mandatory Confirmation Gate:** Present the draft constitution to the user for explicit confirmation (satisfying Step C2).
5. **Transition:** Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and proceed directly to **Step C3 (Specify & Decompose)**.
