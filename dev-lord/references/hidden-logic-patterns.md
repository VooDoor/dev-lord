# Reference: Hidden-Logic Discovery Patterns

> **Purpose:** Used by **Road B (Step B3)** during migration analysis and **Road C (Step C7)** during pre-flight stress-testing. Modern web frameworks often contain implicit business logic and security policies that exist outside standard controller/handler files. Migrations or refactors that miss these patterns result in critical functional regressions.

---

## Ecosystem Inspection Checklist

| Framework / Service | Hidden Logic Mechanism | Files & Locations to Inspect | Critical Risk if Missed |
| :--- | :--- | :--- | :--- |
| **Supabase** | Row-Level Security (RLS) & Triggers | Database migration SQL, `auth.users` triggers, stored procedures, RPC definitions. | Data leaks across tenants; unauthorized writes bypassing API. |
| **Firebase / Firestore** | Security & Validation Rules | `firestore.rules`, `storage.rules`, Cloud Functions triggers (`onCreate`, `onUpdate`). | Unauthenticated document reads or broken write permissions. |
| **Ruby on Rails** | ActiveRecord Callbacks & Observers | `app/models/*.rb` (`before_save`, `after_create`, `after_commit`), model concerns, `app/jobs/*.rb`. | Skipped audit trails, missing side-effects, unsent transactional emails. |
| **Django** | Signals & Model Lifecycle Hooks | `signals.py`, `models.py` (`save()` overrides), custom middleware (`middleware.py`). | User profile creation failing on signup; missing caching invalidation. |
| **Laravel** | Model Observers & Form Requests | `app/Observers/`, `app/Http/Requests/` (validation & authorization), model `booted()` hooks, Eloquent global scopes. | Bypassed multi-tenant scoping; unvalidated payload execution. |
| **Spring Boot** | AOP Aspects & Entity Lifecycle | `@Aspect` classes, `@PrePersist`, `@PreUpdate` entity annotations, interceptors, `SecurityFilterChain` expressions. | Security bypasses, missing audit timestamps (`createdAt`/`updatedAt`). |
| **Node.js / Express** | Middleware Chains & Route Guards | `app.use()` chains, error-handling middleware (`(err, req, res, next)`), custom passport strategies, ad-hoc inline middleware. | Uncaught async errors; missing token validation; malformed payload parsing. |
| **ASP.NET Core** | Action Filters & Model Binders | `IAsyncActionFilter`, `[Authorize]` policies, FluentValidation rules, Entity Framework interceptors (`SaveChangesInterceptor`). | Unaudited entity modifications, missed permission checks. |

---

## Parity Capture Protocol (For Road B Migrations)

For every identified non-trivial hidden logic item (calculations, permissions, validations, data mutations):
1. **Record the Source Rule:** Quote the exact callback, trigger, or policy code.
2. **Capture Representative Input & Output:**
   - Sample Input payload: `{"user_id": 123, "role": "member", "action": "delete"}`
   - Observed Output in Source App: `403 Forbidden - Only tenant owners may delete` (Baseline Source: LIVE_OBSERVED | STATIC_INFERRED)
3. **Log in `ANALYSIS.md`:** This representative sample becomes the mandatory baseline for the **Migration Parity Check (Road C Step C9)** in the target stack.
