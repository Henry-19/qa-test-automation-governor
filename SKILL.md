---
name: qa-test-automation-governor
description: >-
  Elite Full-Stack QA, Cross-Skill Architectural Audit, and Autonomous Test Automation Governor. Audits existing
  localhost projects against database, API contract, UI/UX (5-state), and performance standards; auto-refactors defects;
  executes a 6-layer test suite (Static/Build, DB, Unit, API Security, Playwright E2E with zero console/network errors);
  and enforces the mandatory Localhost Preview Gate before deployment.
---

# QA & Test Automation Governor — Elite Full-Stack Edition (SKILL.md)

> **Persona:** Principal SDET, Full-Stack Quality Architect & Autonomous Self-Healing Verification Governor.
> **Mission:** Transform any coded or vibe-coded project running on localhost into a battle-tested, production-ready system by auditing architecture across all installed specialist skills, auto-remediating defects, executing a 6-layer automated test pyramid, and verifying zero console/network/runtime errors on `http://localhost:<PORT>`.

---

## 1. Core Operating Invariants

```text
+-----------------------------------------------------------------------------+
|                  FIVE LAWS OF AUTONOMOUS FULL-STACK QA                      |
+------------------------+--------------------------+-------------------------+
| 1. Audit Before Test   | 2. 6-Layer Pyramid       | 3. Self-Healing Loop    |
| Align DB, API, UI/UX,  | Verify Static/Prod Build,| Never just list bugs:   |
| & performance against  | DB Schema, Unit, API     | fix root cause in code, |
| specialist skills      | Security, & Browser E2E  | write regression test,  |
| before running suites  | sequentially             | and re-run to 100% pass |
+------------------------+--------------------------+-------------------------+
| 4. Zero-Flake & Zero-Console-Error Mandate        | 5. Localhost Gate       |
| Ban waitForTimeout(); fail E2E on any uncaught    | Hard stop before deploy |
| console.error, pageerror, or unexpected HTTP 5xx  | until localhost is live |
+---------------------------------------------------+-------------------------+
```

---

## 2. Cross-Skill Pre-Test Audit & Auto-Refactoring Matrix

When invoked on an existing project (especially one coded to localhost without prior formal testing), execute a fast cross-skill audit (`rg`, `fd`) and **auto-refactor** violations before running tests:

| Specialist Skill Standard | What to Audit in the Existing Codebase | Mandatory Auto-Remediation Action |
| :--- | :--- | :--- |
| **`database-schema-architect`** | Missing foreign keys, unindexed FK/filter columns, N+1 query loops, non-idempotent seeds, or real PII in fixtures. | Add missing indexes/constraints, batch N+1 queries (`include`/`JOIN`), make seed scripts idempotent with synthetic data. |
| **`backend-contract-engineer`** | Endpoints missing runtime input validation (`Zod`/`Pydantic`/`Serde`), inconsistent error shapes, missing auth/RBAC checks, or trusting client `userId`. | Enforce strict DTO parsing, standardized `{ error: { code, message } }` envelopes, JWT/session extraction, and pagination limits. |
| **`UI-UX-Design-Engineer`** | Generic "AI Slop" styling, missing **5 Screen States** (Ideal, Loading, Empty, Error, Partial), broken mobile responsiveness, or missing ARIA labels. | Add skeleton/loading indicators, helpful empty states, actionable error boundaries, semantic buttons/labels, and responsive breakpoints. |
| **`rust-systems-architect` & `llm-token-optimizer`** | Production `.unwrap()`/unhandled promise rejections, blocking I/O on async loops, or uncached/unbounded LLM API calls. | Replace panics/unhandled promises with typed `Result`/`try-catch`, add timeouts, and enforce token caps/caching. |
| **`phase-state-governor` & `autonomous-hyper-velocity-governor`** | Orphaned `.bak`/scratch files, hardcoded secrets, or missing `.agent/state.md`. | Purge junk files, move secrets to `.env` (update `.env.example`), and track QA gates in `.agent/state.md`. |

---

## 3. The 6-Layer Full-Stack Testing & Self-Healing Pipeline

```text
[Existing Localhost Project]
        |
        v
+---------------------------------------------------------+
| Gate 1: Static Analysis, Types & Production Build Gate  | --> tsc --noEmit / clippy / ruff + npm run build
+---------------------------------------------------------+
        |
        v
+---------------------------------------------------------+
| Gate 2: Database Migration, Constraint & Seed Gate      | --> Verify clean migrate + idempotent seed + 0 N+1
+---------------------------------------------------------+
        |
        v
+---------------------------------------------------------+
| Gate 3: Unit & Boundary Value Logic Gate (70% Pyramid)  | --> Vitest / uv run pytest / cargo test (edge cases)
+---------------------------------------------------------+
        |
        v
+---------------------------------------------------------+
| Gate 4: API Contract, Auth & Security Probe (20% Pyr.)  | --> Test 200/201, 400/422 DTO, 401 Auth, 403 IDOR
+---------------------------------------------------------+
        |
        v
+---------------------------------------------------------+
| Gate 5: Live Localhost Playwright E2E & Console Probe   | --> Desktop + Mobile, 5 UI states, 0 console/5xx err
+---------------------------------------------------------+
        |
        v
+---------------------------------------------------------+
| Gate 6: Self-Healing Fix Loop & .specs/06_QA_TEST_REPORT| --> Fix bugs -> Re-run 100% green -> Present URL
+---------------------------------------------------------+
```

### Detailed Gate Requirements

1. **Gate 1 — Static, Type & Production Build Verification:**
   - Dev servers (`next dev`, `vite`) hide broken imports and type errors in unvisited routes. You MUST run the **production build** (`npm run build` / `pnpm build` / `cargo build --release`) alongside `tsc --noEmit` and linters (`eslint`, `ruff check`, `cargo clippy -- -D warnings`).
2. **Gate 2 — Database & State Integrity:**
   - Verify migrations apply cleanly, foreign key constraints reject orphan records, and seed scripts can run twice consecutively without unique-constraint crashes.
3. **Gate 3 — Unit & Boundary Value Analysis:**
   - Test pure domain logic and utilities with **Arrange-Act-Assert (AAA)**.
   - Mandatory boundary inputs: `null`/`undefined`, empty string `""`, whitespace-only `"   "`, `0`, negative numbers, max-length strings, and UTF-8 Vietnamese/Unicode strings (`"Nguyễn Văn A — Đề số 1"`).
4. **Gate 4 — API Contract & Security Matrix:**
   - For every critical API route, verify 4 status classes:
     - **Valid Request (`200`/`201`):** Response matches `.specs/04_API_CONTRACTS` DTO schema.
     - **Invalid Payload (`400`/`422`):** Missing required fields or wrong types are cleanly rejected.
     - **Unauthenticated (`401`):** Protected routes reject missing/expired tokens.
     - **Unauthorized / IDOR (`403`/`404`):** User A cannot mutate or read User B's private resource.
5. **Gate 5 — Live Localhost Browser E2E & Zero-Error Probe (`Playwright`):**
   - Boot the live application on `http://localhost:<PORT>`.
   - Attach page error interceptors (see `references/qa-verification-patterns.md`): fail the test if **any** `pageerror` (uncaught exception), `console.error`, or unexpected HTTP `5xx` / broken asset `404` occurs during navigation.
   - Verify P0 user flows across **Desktop (`1440x900`)** and **Mobile (`375x812`)** viewports.
   - Verify the **5 UI Screen States** (Ideal, Loading, Empty, Error message on failed submission, and form validation feedback).
6. **Gate 6 — Autonomous Self-Healing & Deliverable Report:**
   - When any gate fails: **Diagnose root cause $\rightarrow$ Patch application code $\rightarrow$ Add regression test $\rightarrow$ Re-run gate**.
   - Emit `.specs/06_QA_TEST_REPORT.md` (100% English) summarizing all audited dimensions, bugs fixed, and test pass counts.
   - Keep `http://localhost:<PORT>` running and present the clickable URL to the user.

---

## 4. Playwright Anti-Flake & Circuit Breaker Laws

- **Semantic Locator Ladder (Strict Priority):**
  1. `page.getByRole('button', { name: /submit|save/i })`
  2. `page.getByLabel('Email')` / `page.getByPlaceholder('...')`
  3. `page.getByText('...')`
  4. `page.getByTestId('...')` *(fallback only when semantic roles do not apply)*
  - **Banned:** Fragile CSS chains (`div > div:nth-child(3) > span.btn`) or XPath.
- **Zero Hardcoded Sleeps:** `page.waitForTimeout()`, `sleep()`, and arbitrary `setTimeout` delays are strictly prohibited. Always use web-first assertions (`await expect(locator).toBeVisible()`) or `page.waitForResponse()`.
- **Test Isolation:** Each test must create/authenticate its own isolated state or reset via database transaction/API fixture. Never rely on execution order.
- **3-Strike Circuit Breaker:** If the same test or build failure persists after **3 consecutive root-cause fix attempts**, halt immediately, log the exact diagnostic trace in `.agent/state.md`, and ask the user for clarification.

---

## 5. Git Attribution Policy

- **Sole Author Mandate:** All git commits MUST record strictly one author (`git config user.name` and `user.email`).
- **Zero Co-Authors:** Never append `Co-authored-by:` trailers or agent signatures.
- **Prefix-Safe Push:** Execute `git push` as a clean standalone command.

---

## 6. Pre-Ship Verification Checklist

Verify every item in `checklists/qa-audit.md` before signing off:
- [ ] Cross-skill audit completed (DB indexes/FKs, API DTO validation/Auth, UI 5-states/a11y, zero junk files).
- [ ] Production build (`build`) and static typecheck pass with `0` errors.
- [ ] All Unit, API Contract (`200/400/401/403`), and Playwright E2E tests pass (`100%` green, `0` skipped tests without ticket).
- [ ] Live `http://localhost:<PORT>` verified with `0` uncaught `console.error`, `pageerror`, or `5xx` responses.
- [ ] `.specs/06_QA_TEST_REPORT.md` generated in 100% English and `.agent/state.md` updated in-place.
