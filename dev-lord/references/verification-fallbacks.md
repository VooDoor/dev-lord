# Reference: Verification Fallback Matrix

> **Purpose:** Used across **Road B (Step B2)**, **Road C (Step C9)**, and **Road C-Lite (Step L3)**. Defines the deterministic hierarchy of verification tools when specialized testing environments, headless browsers, or direct terminal execution are restricted.

---

## Verification Priority Hierarchy

Verification must always follow this strict descending order. Lower tiers may only be used when higher tiers are demonstrably unavailable in the target runtime environment.

```text
Tier 1: Automated Test Suites (Unit / Integration)
   └── Tier 2: Live Process Execution & CLI Checks
          └── Tier 3: Direct HTTP / Endpoint Verification (curl / API runner)
                 └── Tier 4: UI & Browser Inspection
                        ├── Tier 4-A: Live Browser Render & DOM Verification (Console error free, interactive components)
                        └── Tier 4-B: Static Asset & Syntax Audit (HTML5 parse, physical asset resolution check)
```

---

## Component Fallback Matrix

| Target Component | Preferred Verification Tool (Tier 1) | Fallback When Tool Unavailable | Unacceptable Substitute (Forbidden) |
| :--- | :--- | :--- | :--- |
| **Backend API** | Integration test suite (`pytest`, `jest`, `dotnet test`) or live server `curl`. | Shell-executed unit test or mock HTTP runner script in node/python. | Reading controller code back to yourself. |
| **Database Schema & Migrations** | Live DB migration execution + SQL schema query (`information_schema`). | In-memory SQLite migration run or dry-run validation script. | Assuming SQL syntax is valid without running it. |
| **Frontend UI (Full Stack)** | Headless browser automation (Playwright / Puppeteer) validating DOM elements. | CLI curl or script verifying HTTP status, HTML headers, and element IDs. | Claiming "pixel perfection" without a browser or DOM check. |
| **UI Prototype (Road U)** | **Tier 4-A (Preferred):** Live browser render via browser tool/agent asserting 0 console errors and functional navigation/toggles. | **Tier 4-B (Fallback):** HTML5 syntax validation parse, physical asset existence check, and self-containment grep audit. | Claiming screens are verified without checking referenced asset paths or checking markup. |
| **Restricted Shell Environment** | Direct tool execution via terminal. | Output a copy-pasteable terminal script; wait for user to execute and return output. | Fabricating mock CLI output or pretending execution succeeded. |

---

## The Non-Negotiable Rule: Execution Over Assertion

1. **Self-Review Is Not Verification:** An AI reading code it just wrote and stating *"The code looks correct and handles all errors"* is NOT verification.
2. **Real Output Evidence Required:** Every completed task must have the exact command executed and its actual terminal stdout/stderr logged in `progress-log.md`.
3. **Pre-Existing Baseline Failures:** If an automated test suite already has failing tests before any task begins (verified in Step C2 Clean Baseline Check), pre-existing failures must not be counted against the current task's code logic.
