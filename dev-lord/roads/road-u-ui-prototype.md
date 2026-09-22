# Road U: The UI Prototype Architect (Theme-Based & AI-Crafted)

> **Mission:** Transform an application specification into a verified, working, multi-screen HTML prototype either from a real local UI theme (via automated subagent theme ingestion) or via AI-crafted modern design. Road U generates an authoritative **design source of truth**—real pages, navigation, forms, fields, and visual language—for whatever backend/frontend stack is chosen afterward.

---

## Foundational Principles (Non-Negotiables)

1. **Design Source of Truth, Not Runtime Code:**
   The prototype produced by Road U is a static visual and behavioral reference library. Whatever technology stack is eventually chosen (React, Django, Rails, Vue, ASP.NET, Flutter, etc.) must recreate this design natively in its own idiom. Road C (Implementation) treats the prototype folder as an immutable **read-only source of truth** and must **never** directly inject backend logic or template tags into the prototype files (unless the chosen stack is plain static HTML/JS).
2. **The Theme Folder is Read-Only (Mode A):**
   When using an existing local theme, the original theme directory is an immutable source library. It is never modified or written to.
3. **Wholesale Asset Self-Containment:**
   Every asset the prototype needs (CSS, JS, fonts, icons, plugins, images) resides locally inside the prototype's own `$PROTOTYPE_DIR/assets/` subfolder. The finished prototype must be 100% self-contained and run with zero external dependencies on original theme directories.
4. **Selection Over Construction (Mode A) / Curated Precision (Mode B):**
   In Mode A, select matching pages and components directly from the digested theme, preserving full markup and working JS behaviors. In Mode B, build modern, high-fidelity components using cohesive design tokens, responsive CSS Grid/Flexbox, and standalone SVG icon systems (Lucide/Tabler).
5. **Hierarchical Subagent Delegation:**
   The Main Agent acts as the persistent Project Manager and sole user guide. Subagents are dispatched for focused, isolated jobs (theme ingestion, individual screen generation). Each subagent executes its scope, reports factual evidence back to the Main Agent, and terminates immediately. The Main Agent reviews evidence, updates disk state, and guides the user forward.
6. **Spec–Prototype Sync Integrity:**
   The Master Spec and the UI Prototype are two views of the same truth: the prototype defines *how the application looks*; the spec defines *what it does*. Any structural or functional change made to the prototype during Road U **must** be reflected back into the Master Spec (with user approval) before the prototype is considered final or handed off. The Spec is never allowed to fall behind the Prototype, and delivery is blocked until the sync audit passes (Step U13-S).

---

## Operating Flow

```text
[Entry: Q0 Option [5], Gate A-X [AX-3], Gate B-X [BX-3], or Mid-Road-C Step C5/C3]
       │
       ▼
[Step U0: Prototyping Mode Selection: [A] Existing Theme vs [B] AI-Crafted UI]
       │
       ├─── Mode A: [Existing Theme Ingestion]
       │      │
       │      ├── Step U0-A1: Collect Local Theme Path & Pre-Flight Check
       │      ├── Step U0-A2: Dispatch Theme Ingestion Subagent
       │      │     └── Subagent produces <THEME_OUTPUT_DIR>/theme_report.md & theme-digest.md
       │      │     └── Subagent reports completion and terminates; Main Agent reviews & informs user
       │      └── Step U0-A3: Wholesale Native Copy of Theme Assets to $PROTOTYPE_DIR/assets/
       │
       └─── Mode B: [AI-Crafted Modern Design]
              │
              ├── Step U0-B1: Design Token & Aesthetic Architecture (Colors, Typography, Layout)
              └── Step U0-B2: Initialize Self-Contained Baseline ($PROTOTYPE_DIR/assets/css, js, icons)
       │
       ▼
[Step U1: Language Scope Check & Default Direction (dir="ltr|rtl")]
       │
       ▼
[Step U2: Screen Inventory & Pre-Generation Navigation Linkage Map]
       │ (Spec-less intake: prompts user for screen list if no prior spec exists)
       ▼
[Steps U3-U5: Skin / Palette Evaluation, User Confirmation & Layout Locking]
       │ (Saves config to $PROTOTYPE_DIR/.prototype-state.json)
       ▼
┌──────[Step U7: Hierarchical Subagent Screen Generation Loop (1 Screen at a Time)]◄──┐
│             │                                                                      │
│             ▼                                                                      │
│      [Subagent Worker: Pristine Context]                                           │
│      - Generates target HTML screen from digested theme (Mode A) or AI tokens (Mode B)
│      - Injects locked layout attributes on <html>                                  │
│      - Wires sidebar links using the Project Navigation Linkage Map                │
│      - Injects realistic domain data (strictly no lorem ipsum)                     │
│      - Runs Step U12 Real Verification Pass on the file                            │
│      - Reports completion & evidence back to Main Agent, then terminates           │
│             │                                                                      │
│             ▼                                                                      │
│      [Main Agent: Review, Progress Logging & Spec Sync]                            │
│      - Verifies file creation and updates .prototype-state.json                    │
│      - Runs lightweight Spec-diff check (divergence ──► Step U7-R)                 │
│      - Informs user of progress and dispatches next screen worker ─────────────────┘
│
└──────► (All Screens Verified)
       │
       ▼
[Steps U8–U13: Wizards, Consistency, Quality Pass & Physical Asset Verification]
       │
       ▼
[Step U13-S: Spec ↔ Prototype Sync Audit (HARD GATE)] ──► Spec Sync Status: SYNCED | N/A
       │
       ▼
[Intersection Gate U-X: Origin-Aware Crossing (UX-1, UX-2, UX-3)]
```

---

## Coordinates & Paths

### 1. Prototype Output Directory (`$PROTOTYPE_DIR`)
- **Standard Default:** `<project-root>/ui-prototype/`
- **User Override:** If the user specifies an alternative path (e.g., `./preview`), respect the explicit path and assign to `$PROTOTYPE_DIR`.
- All generated HTML screens, assets, and `.prototype-state.json` reside inside `$PROTOTYPE_DIR`.

### 2. State Persistence (`$PROTOTYPE_DIR/.prototype-state.json`)
Road U maintains a lightweight, crash-resilient state manifest on disk so it can be resumed at any time without amnesia:
```json
{
  "prototypeMode": "theme | ai-crafted",
  "themeSourcePath": "path/to/theme (if Mode A)",
  "outputDirectory": "ui-prototype/",
  "specOfRecord": "MASTER_SPEC.md | .devop-process/spec.md | NONE",
  "origin": "standalone | road-a | road-b | road-c-detour",
  "lockedHtmlAttributes": "<html lang=\"en\" ...>",
  "activeSkin": "skin-identifier",
  "direction": "ltr | rtl",
  "navigationLinkageMap": [],
  "completedScreens": [],
  "pendingScreens": []
}
```

---

## Interaction Modes

- **Interactive Mode (Default):**
  Pauses at mandatory decision points:
  1. Step U0: Mode selection ([A] Theme vs. [B] AI-Crafted).
  2. Step U1: Language scope & RTL preferences.
  3. Step U4: Skin/palette confirmation.
- **Autonomous Mode:**
  Triggered only when the user explicitly requests zero interruptions (e.g., *"just build it"*). Defaults to single-language English LTR, auto-locks the recommended theme/skin, and discloses all applied defaults in Step U14's delivery report.
  **Steps U7-R (approval of spec changes) and U13-S (sync audit) are never skipped, even in Autonomous Mode.**

---

## Execution Steps U0–U14

### Step U0: Prototyping Mode Selection

Present the user with the foundational prototyping choice:
> *"How would you like to build the UI prototype?*
> - **[A] Theme-Based Prototype (From Existing Theme)**: Use a local commercial or open-source HTML/Bootstrap/Tailwind theme folder. Dev-Lord will analyze and digest it automatically.
> - **[B] AI-Crafted Modern UI (AI Design Knowledge)**: Rely on modern web engineering best practices (curated color palettes, CSS tokens, responsive grid, glassmorphism/card portlets, clean typography, Lucide/Tabler SVG icons) with zero external theme folder required."*

---

### Step U0-A: Theme Ingestion Subagent Protocol (Mode A)

1. **Path Acquisition & Pre-Flight Check:**
   - Check if `<project-root>/theme/` exists locally. If found, prompt: *"Found local theme at `./theme/`. Use this theme folder? [Yes / Enter custom path]"*.
   - If not found or user specifies custom path, ask user for the absolute or workspace-relative path to the theme folder.
   - Verify directory exists, is readable, and contains HTML and asset files.
2. **Dispatch Theme Ingestion Subagent:**
   Resolve `<SKILL_DIR>` to the absolute directory of the `dev-lord` skill. Ensure `<PROJECT_ROOT>` is set to the workspace root.
   - **State Isolation Rule:** If running in standalone prototype mode (`origin == standalone`), set `<THEME_OUTPUT_DIR>` to `"$PROTOTYPE_DIR/.theme"`. If running as part of a full project lifecycle (`origin == road-c-detour`, `road-b`, or `road-a`), set `<THEME_OUTPUT_DIR>` to `"<PROJECT_ROOT>/.devop-process"`. Ensure `<THEME_OUTPUT_DIR>` directory exists.
   
   Dispatch an ephemeral subagent with this prompt (interpolating `<SKILL_DIR>`, `<PROJECT_ROOT>`, and `<THEME_OUTPUT_DIR>` with their actual paths):
   ```text
   You are an expert Frontend Systems Architect and UI Reverse-Engineer.
   Your task is to analyze the local UI theme directory at:
   "<THEME_SOURCE_PATH>"

   Follow this two-phase execution protocol without skipping any steps:

   ### PHASE 1: Master Technical Report
   1. Deeply inspect the theme directory: HTML showcase pages, CSS design tokens, JavaScript controllers, plugins, vendor libraries, and icon systems.
   2. Read the structure and instructions in:
      "<SKILL_DIR>/templates/theme_report.template.md"
   3. Generate the complete report at:
      "<THEME_OUTPUT_DIR>/theme_report.md"
   4. CRITICAL INSTRUCTIONS FOR PHASE 1:
      - Document ALL production HTML pages. Do NOT use placeholder comments like "<!-- more files here -->" or summarize away pages.
      - Detect the layout architecture (Attribute-driven like data-* attributes, Class-driven like Tailwind/BEM, or Token-driven CSS variables).
      - Document the icon ecosystems, plugin dependencies, and charting engines.

   ### PHASE 2: Standardized Theme Digest
   1. Read the newly generated report at "<THEME_OUTPUT_DIR>/theme_report.md" and read the template:
      "<SKILL_DIR>/templates/theme-digest.template.md"
   2. Generate the authoritative theme digest at:
      "<THEME_OUTPUT_DIR>/theme-digest.md"
   3. CRITICAL INSTRUCTIONS FOR PHASE 2:
      - Extract and populate the exact configuration dimensions according to the detected architecture.
      - Populate the available skins table (identifier, primary hex, font family, recommended mood).
      - Map icon systems, i18n/RTL mechanics, and the complete page & showcase file inventory.
      - Set "Default Local Source Path" to "<THEME_SOURCE_PATH>".

   ### COMPLETION CRITERIA:
   Verify both files physically exist and are non-empty:
   1. "<THEME_OUTPUT_DIR>/theme_report.md"
   2. "<THEME_OUTPUT_DIR>/theme-digest.md"
   Return a concise summary reporting total pages inventoried, skins detected, and primary CSS framework version.
   ```
   **Single-Context Fallback:** If subagent dispatch is unavailable in the environment, the Main Agent performs Phase 1 and Phase 2 directly in sequence, writing `<THEME_OUTPUT_DIR>/theme_report.md` and `theme-digest.md`.
3. **Subagent Completion & Main Agent Handoff:**
   - The subagent executes, confirms file creation, returns the summary report to the Main Agent, and **terminates immediately**.
   - The Main Agent reviews the returned summary, confirms that `<THEME_OUTPUT_DIR>/theme-digest.md` is populated, and informs the user:
     > *"Theme analysis complete: `[N]` production pages, `[M]` skins, and `[Framework]` architecture cataloged. Theme digest established at `<THEME_OUTPUT_DIR>/theme-digest.md`. Proceeding to asset setup..."*
4. **Wholesale Asset Copy:**
   Read `<theme_assets>` directly from Section 1 (`Theme Assets Directory Path`) of `<THEME_OUTPUT_DIR>/theme-digest.md`.
   Create `$PROTOTYPE_DIR/assets/`. Perform a wholesale native OS copy of the theme's asset directory into `$PROTOTYPE_DIR/assets/`:
   - Windows (PowerShell):
     `Copy-Item -Path "<theme_assets>\*" -Destination "$PROTOTYPE_DIR/assets" -Recurse -Force`
   - Linux/macOS:
     `cp -R "<theme_assets>/." "$PROTOTYPE_DIR/assets/"`
   Downstream steps will read `<THEME_OUTPUT_DIR>/theme-digest.md` as the authoritative theme contract.

---

### Step U0-B: AI-Crafted Design Baseline Protocol (Mode B)

1. **Design System Token Architecture:**
   Select or confirm a curated modern design aesthetic:
   - **Color Palette Tokens:** HSL-tailored primary accent (e.g. Deep Indigo `#4f46e5` or Slate Blue `#3b82f6`), dark/light surface tokens, semantic success/warning/danger colors, neutral slate borders.
   - **Typography:** Self-contained modern system font stack (`font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;`) to guarantee zero external font downloads or network dependencies.
   - **Visual Language:** Subtle border radii (8px/12px), clean card elevation shadows, responsive CSS Grid layout with collapsible sidebar and sticky topbar.
   - **Icon Ecosystem:** Lucide SVG or Tabler SVG standalone inline icon markup.
2. **Initialize Self-Contained Baseline Assets:**
   Create `$PROTOTYPE_DIR/assets/css/` and `$PROTOTYPE_DIR/assets/js/`:
   - Write `$PROTOTYPE_DIR/assets/css/tokens.css` (CSS variables for `--theme-primary`, `--theme-bg`, `--theme-surface`, `--theme-text`, etc.).
   - Write `$PROTOTYPE_DIR/assets/css/layout.css` (responsive sidebar, topbar, cards, tables, forms, and utilities).
   - Write `$PROTOTYPE_DIR/assets/js/app.js` (light/dark mode toggle, sidebar collapse/expand, modal/drawer triggers).

---

### Step U1: Language Scope Check (Gate)
Ask the user if this is a single-language application or multi-language with live switching. If multi-language, confirm default direction (`dir="ltr"` or `dir="rtl"`). If Arabic/Hebrew is default, the prototype initializes in `dir="rtl"`.

---

### Step U2: Screen Inventory & Pre-Generation Navigation Linkage Map

1. **Identify the Spec of Record:**
   - `MASTER_SPEC.md` or `.devop-process/spec.md` if present, or
   - User-provided spec path, or
   - **NONE** (no written spec).
2. **Spec-less Screen Intake (If Spec of Record is NONE):**
   Ask the user explicitly:
   > *"You do not have a written specification. Please list the screens, pages, or workflows you need in this prototype (e.g., Dashboard, Order Management, Customer Detail, Analytics, User Settings, Login)."*
   Wait for user response and parse into the confirmed Screen List.
   *(Offer once: "Would you like me to generate `MASTER_SPEC.md` from this screen list so it stays in sync as we prototype? [Yes / No, just prototype]")*.
3. **Review Available Showcase Patterns:**
   - In Mode A: Match requested screens against the page inventory in `<THEME_OUTPUT_DIR>/theme-digest.md`.
   - In Mode B: Identify standard layout patterns (Dashboard with KPI tiles, Data Table view with filters, Kanban board, Form wizard, Split auth).
4. **Compile the Project Navigation Linkage Map Table:**
   Before generating any markup, compile the authoritative navigation table:
   
   | Screen Navigation Label | Pattern / Showcase Reference | Generated Output Filename |
   | :--- | :--- | :--- |
   | Dashboard | `index.html` (or Executive Dashboard) | `index.html` |
   | Pipeline | `crm-pipeline.html` (or Kanban Board) | `pipeline.html` |
   | Invoices | `invoice-list.html` (or Data Table) | `invoices.html` |
   | Settings | `account-settings.html` (or Form View) | `settings.html` |

   *Rule: Every generated screen must use this exact table for all sidebar and header navigation links. Zero broken or leftover demo links are permitted.*
5. Save state to `$PROTOTYPE_DIR/.prototype-state.json`.

---

### Steps U3–U5: Skin Selection & Layout Attribute Locking

1. **Skin / Palette Selection:**
   - Mode A: Read available skins from `<THEME_OUTPUT_DIR>/theme-digest.md` Section 3.1. Present the skins and highlight the Recommended skin.
   - Mode B: Present 3 curated palette moods (e.g., *[1] Modern Indigo SaaS*, *[2] Clean Minimal Slate*, *[3] High-Contrast Emerald Enterprise*).
2. **User Confirmation (Gate):** In Interactive mode, pause for confirmation. In Autonomous mode, auto-lock the recommended skin.
3. **Lock Layout Configuration:**
   Define the root `<html>` attribute string (e.g. `data-skin="modern" data-bs-theme="light" dir="ltr"`).
   Record this exact attribute string in `.prototype-state.json`. It must be applied identically across every generated screen.

---

### Step U6: Asset Baseline Finalization
Ensure `$PROTOTYPE_DIR/assets/` contains all needed CSS, JS, fonts, and icon assets. Confirm directory exists before starting screen generation.

---

### Step U7: Hierarchical Subagent Screen Generation Loop

Execute screen generation **strictly one screen at a time**:

1. **Subagent Task Worker Delegation:**
   Dispatch the generation of the target screen to a fresh subagent:
   - **Context passed to Subagent:**
     - Target screen name, requirement, and domain details.
     - Prototype mode (Mode A with showcase reference file, or Mode B with token specs).
     - Target output path (e.g., `$PROTOTYPE_DIR/invoices.html`).
     - The locked `<html>` configuration attribute string from Step U5.
     - The complete **Navigation Linkage Map** from Step U2.
   - **Subagent Execution Scope:**
     - Generates the complete HTML screen.
     - Injects locked attributes on `<html>`.
     - Wires sidebar navigation markup with the Navigation Linkage Map, setting `class="active"` on the current screen.
     - Adapts tables, cards, and labels to realistic domain data (strictly no "Lorem Ipsum").
     - Preserves all working features (data tables, filters, drawer actions, wizards).
     - Runs **Step U12 Real Verification Pass** on the generated file.
     - Reports completion evidence back to the Main Agent and **terminates immediately**.
   **Single-Context Fallback:** If subagent dispatch is unsupported or unavailable, the Main Agent generates the screen directly in the current context, runs Step U12 verification, updates `.prototype-state.json`, and advances sequentially.
2. **Main Agent Action & State Closure:**
   - Main Agent reviews subagent's evidence, verifies file creation, and logs completion.
   - Updates `$PROTOTYPE_DIR/.prototype-state.json` (moves screen from `pendingScreens` to `completedScreens`).
   - Runs a lightweight **Spec-diff check** (skipped if Spec of Record is NONE) comparing the screen against Spec Section 3. If divergence is detected, triggers **Step U7-R**.
   - Informs the user concisely of progress (e.g., *"Generated screen 3 of 6: `invoices.html` [Verified]"*).
   - Dispatches the next screen to a fresh subagent until all screens are completed.

---

### Step U7-R: Spec Reconciliation (Mandatory After Any Spec-Relevant Change)
*If Spec of Record is NONE, skip this step entirely.*

Any change to the prototype that alters what the application does or contains must be reflected in the spec:
1. Classify change (screen added/removed, field/filter added, action button added, status/state added).
2. Collect changes into ONE drafted spec diff and present to user for **explicit confirmation**.
3. On approval, update `spec.md` (Section 3 and Section 2 if capability changed) and record in Prototype Sync Log in Spec Section 6.
4. *Silence is not approval: never write to the spec without explicit user confirmation.*

---

### Steps U8–U13: Wizards, Consistency, Quality Pass & Verification

- **Step U8: Multi-Step Wizards:** Use structured step indicators and validation hooks for multi-step creation flows.
- **Step U9: Navigation Link Resolution Audit:** Inspect all generated `.html` files in `$PROTOTYPE_DIR`. Confirm that 100% of internal links resolve to real generated files and zero broken demo links exist.
- **Step U10: Consistency Pass:** Confirm that the locked `<html>` attributes, topbar, and sidebar structure are 100% identical across all generated files.
- **Step U11: Quality Pass:** Verify domain-specific realistic data throughout, proper semantic elements (`<button>`, `<label>`, ARIA), and meaningful image `alt` attributes.
- **Step U12: Real Verification Pass (Hard Check):**
  1. **Tier 4-A: Browser/DOM Rendering Check (Preferred):** If a browser tool or headless browser is available in the environment (e.g., Chrome DevTools MCP, Playwright, or browser agent):
     - Open or serve `$PROTOTYPE_DIR/<screen>.html`.
     - Verify page renders cleanly with 0 uncaught JavaScript console errors.
     - Verify critical interactive components (sidebar collapse, dropdowns, modal dialogs) toggle expected DOM states.
  2. **Tier 4-B: Static Asset & Syntax Audit (Mandatory Fallback):**
     - **Syntax Check:** Parse generated `.html` files to confirm valid HTML5 structure.
     - **Asset Resolution Check:** Confirm that every CSS, JS, font, and image referenced in markup physically exists in `$PROTOTYPE_DIR/assets/`.
     - **Self-Containment Check:** Grep generated files to confirm **zero** absolute external paths or remote CDNs point outside `$PROTOTYPE_DIR/`.
- **Step U13: Validation Checklist Confirmation:** Confirm all screens run standalone with zero external dependencies.

---

### Step U13-S: Spec ↔ Prototype Sync Audit (HARD GATE)
- **If Spec of Record is NONE:** Confirm all screens appear in Navigation Linkage Map, set **Spec Sync Status: N/A (no Spec of Record)**, and proceed to Step U14.
- **Otherwise:** Compare prototype against Spec of Record:
  1. Every `.html` screen has a matching row in Spec Section 3 with `Source Prototype File` filled.
  2. Every Section 3 row points to an existing file.
  3. Form fields, filters, and actions match row descriptions.
  4. All Prototype Sync Log entries show `User Approved = Yes`.
- If any mismatch is found, run Step U7-R. Delivery is blocked until audit passes.
- When audit passes, set **Spec Sync Status: SYNCED**.

---

### Step U14: Delivery
1. Confirm **Spec Sync Status** (SYNCED or N/A).
2. Present the prototype directory path (`$PROTOTYPE_DIR`), list generated screens, and report applied mode (Theme Ingestion vs. AI-Crafted).
3. Report any spec reconciliations recorded in Prototype Sync Log.

---

## Intersection Gate U-X (The Road U Crossing)

> **Precondition:** May only be presented when **Spec Sync Status: SYNCED** or **N/A (no Spec of Record)**.

Present the 3 crossing options:
> *"The UI prototype is complete and verified — `<N>` screens generated at `<$PROTOTYPE_DIR>`. Open `index.html` to browse it.*
> *This prototype is now the design source of truth. The actual application will recreate this design natively in its own idiom (components/templates/views) — not run these HTML files directly (unless you choose a plain HTML/JS stack).*
> *How would you like to proceed?*
> - **[UX-1] Revise the prototype** — Adjust, add, or remove screens, fields, actions, layouts, or skin.
> - **[UX-2] Approve & Build Native Application (Bridge to Road C)** — Move to live implementation from this design source.
> - **[UX-3] Stop here for now** — Keep prototype as standalone deliverable."*

### Crossing Handoff Directives:

- **If [UX-1] is chosen:** Loop back into Road U:
  - Step U4 for skin/palette adjustments.
  - Step U7 for edits to existing screens (+ Step U7-R).
  - Step U2 $\rightarrow$ U7 $\rightarrow$ U7-R if screens are added, removed, or renamed.
  - Re-run Step U13-S before returning to Gate U-X.

- **If [UX-2] is chosen (Origin-Aware Branching):**
  - **If `origin: road-c-detour`:**
    Update `constitution.md` under **UI Design Source of Truth** (`Reference Path: $PROTOTYPE_DIR`), update `spec.md` Section 3, and return directly to the Road C caller (**Step C3** or **Step C5/C6**). Do NOT prompt for tech stack or re-initialize C1/C2.
  - **If `origin: road-b`:**
    Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool. Seed `.devop-process/` using `ANALYSIS.md` and `CONVERSION_PLAN.md`, and record `$PROTOTYPE_DIR` in `constitution.md`.
    **Clean Baseline Verification Check:** Check whether legacy baseline tests were executed during Road B. If tests exist but were not run, execute them now. If failing, report to user before modifying code.
    Jump directly to **Step C6 (Task Sizing & Risk Flagging)**.
  - **If `origin: road-a`:**
    Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and proceed to **Step C1 & C2**. Prompt user for backend/frontend stack and seed `spec.md` from `MASTER_SPEC.md`.
  - **If `origin: standalone`:**
    Load `<SKILL_DIR>/roads/road-c-execution.md` using your environment's file reading tool and proceed to **Step C1 & C2**. Prompt user for target stack, seed initial kernel files, migrate `$PROTOTYPE_DIR/.theme/` analysis files to `.devop-process/` if Mode A was used, and record `$PROTOTYPE_DIR` in `constitution.md`.

- **If [UX-3] is chosen:**
  Deliver standalone `$PROTOTYPE_DIR` folder. If `origin == standalone`, ensure no `.devop-process/` directory is created (theme reports remain encapsulated in `$PROTOTYPE_DIR/.theme/`). If `origin == road-c-detour`, preserve existing `.devop-process/` intact. Conclude the session.
