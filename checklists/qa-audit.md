# Pre-Ship Audit Checklist: QA & Test Automation Governor (15-Point Rubric)

## A. Cross-Skill Architectural Alignment (Pre-Test Refactoring)
- [ ] **1. Database Hygiene (`database-schema-architect`):** All foreign keys, unique constraints, and query filter indexes exist; N+1 query loops are eliminated; seed scripts are idempotent and contain zero real PII.
- [ ] **2. API Contract & Security (`backend-contract-engineer`):** Every write/query endpoint enforces strict DTO schema validation (`Zod`/`Pydantic`/`Serde`), extracts user identity strictly from verified auth tokens, enforces pagination, and returns standardized error envelopes.
- [ ] **3. UI/UX 5-States & Accessibility (`UI-UX-Design-Engineer`):** Every primary screen implements Ideal, Loading, Empty, Error, and Partial states; satisfies WCAG 2.1 AA contrast and keyboard navigation; exhibits zero horizontal overflow on mobile (`375px`) and desktop (`1440px`).
- [ ] **4. Performance & Token Discipline (`rust-systems-architect` / `llm-token-optimizer`):** Zero production `.unwrap()` or unhandled promise rejections; LLM calls (if any) enforce model routing and token caps.
- [ ] **5. Workspace Cleanliness (`phase-state-governor`):** Zero `.bak`, scratch test scripts, or hardcoded secrets in source code; `.env.example` is up to date.

## B. 6-Layer Automated Test Pyramid
- [ ] **6. Gate 1 — Production Build & Typecheck:** `tsc --noEmit` (or `cargo check`/`mypy`), linter, and full production build (`npm run build` / `cargo build`) exit with code `0`.
- [ ] **7. Gate 2 — Database & Migration Verification:** Migrations and seed scripts execute cleanly from a fresh state without errors.
- [ ] **8. Gate 3 — Unit & Boundary Value Tests:** Fast unit tests cover core business logic plus edge cases (`null`, empty, `0`, negative values, max limits, UTF-8 Vietnamese/Unicode strings).
- [ ] **9. Gate 4 — API Contract & Security Matrix:** Automated API tests verify `200/201` happy paths, `400/422` invalid payload rejections, `401` unauthenticated access blocks, and `403` IDOR/cross-user protection.
- [ ] **10. Gate 5 — Playwright Live E2E Journeys:** End-to-end browser tests verify critical user workflows using semantic locators (`getByRole`, `getByLabel`) with **zero** `waitForTimeout()` calls.
- [ ] **11. Gate 5b — Zero Console & Network Error Trap:** Live browser navigation produces `0` uncaught `pageerror` exceptions, `0` `console.error` logs, and `0` unexpected HTTP `5xx` or broken asset `404` responses.

## C. Autonomous Self-Healing & Deliverables
- [ ] **12. Root-Cause Bug Remediation:** Every bug discovered during Gates 1–5 was fixed in source code and locked with a dedicated regression test (100% pass rate, zero unreferenced `test.skip`).
- [ ] **13. 100% English QA Report:** `.specs/06_QA_TEST_REPORT.md` generated in 100% English detailing architectural fixes, test metrics, and healed defects.
- [ ] **14. In-Place State Sync:** `.agent/state.md` updated in-place with QA verification completion status.
- [ ] **15. Live Localhost Preview Gate:** Application is running cleanly on `http://localhost:<PORT>` and presented to the user before any production deployment.
