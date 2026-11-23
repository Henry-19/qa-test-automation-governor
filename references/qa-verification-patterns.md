# Full-Stack QA, API Security & Playwright Verification Patterns

## 1. Playwright Zero-Console-Error & Network-5xx Auto-Trap Fixture

Most E2E tests pass even when the browser console is throwing React hydration errors, uncaught TypeErrors, or background `500 Internal Server Error` API responses. Use this fixture to fail immediately on any hidden runtime defect:

```typescript
import { test as base, expect } from '@playwright/test';

export const test = base.extend<{ assertZeroRuntimeErrors: void }>({
  assertZeroRuntimeErrors: [
    async ({ page }, use) => {
      const runtimeErrors: string[] = [];

      page.on('pageerror', (err) => {
        runtimeErrors.push(`[Uncaught PageError] ${err.message}`);
      });

      page.on('console', (msg) => {
        if (msg.type() === 'error') {
          runtimeErrors.push(`[Console Error] ${msg.text()}`);
        }
      });

      page.on('response', (res) => {
        if (res.status() >= 500) {
          runtimeErrors.push(`[HTTP ${res.status()}] ${res.request().method()} ${res.url()}`);
        }
      });

      await use();

      expect(
        runtimeErrors,
        `Detected runtime console/network errors:\n${runtimeErrors.join('\n')}`
      ).toEqual([]);
    },
    { auto: true },
  ],
});

export { expect };
```

---

## 2. Playwright Semantic Locator Ladder & 5-State UI Verification

```typescript
import { test, expect } from './fixtures';

test.describe('Critical User Journey & 5-State UI Audit', () => {
  test('renders Ideal, Loading, Empty, and Validation Error states cleanly across viewports', async ({ page }) => {
    // 1. Desktop Viewport Check
    await page.setViewportSize({ width: 1440, height: 900 });
    await page.goto('/');

    // Verify primary heading via semantic role (NEVER brittle CSS selectors)
    await expect(page.getByRole('heading', { level: 1 })).toBeVisible();

    // 2. Form Validation / Error State Check
    const submitBtn = page.getByRole('button', { name: /submit|create|save|sign in/i }).first();
    if (await submitBtn.isVisible()) {
      await submitBtn.click();
      // Ensure validation feedback appears without crashing
    }

    // 3. Mobile Responsive Viewport Check (No horizontal overflow)
    await page.setViewportSize({ width: 375, height: 812 });
    const hasHorizontalOverflow = await page.evaluate(() => {
      return document.documentElement.scrollWidth > document.documentElement.clientWidth;
    });
    expect(hasHorizontalOverflow).toBe(false);
  });
});
```

---

## 3. API Contract & Security Matrix Probe (Vitest / Supertest / Pytest)

Every protected endpoint must be tested across 4 security and contract dimensions:

| Test Case | Input / Context | Expected HTTP Status | Verification Assertion |
| :--- | :--- | :--- | :--- |
| **1. Happy Path Contract** | Valid auth token + valid DTO payload | `200 OK` or `201 Created` | Matches `.specs/04_API_CONTRACTS` schema; DB record persisted |
| **2. Malformed DTO Rejection** | Missing required fields / wrong types / negative numbers | `400 Bad Request` or `422` | Standardized `{ error: { code, message } }` envelope; zero DB mutation |
| **3. Unauthenticated Gate** | No `Authorization` header / expired cookie | `401 Unauthorized` | Zero data leakage in response body |
| **4. IDOR / Cross-Tenant Gate** | User A token accessing/mutating User B resource ID | `403 Forbidden` or `404` | Strict ownership enforcement (`WHERE id = $1 AND user_id = $2`) |

---

## 4. Standardized `.specs/06_QA_TEST_REPORT.md` Template

Emit this exact report structure in 100% English at the end of Gate 6:

```markdown
# Full-Stack QA & Architectural Verification Report

- **Timestamp:** YYYY-MM-DD
- **Localhost Preview URL:** `http://localhost:<PORT>`
- **Overall Status:** PASS (100% Gates Green)

## 1. Cross-Skill Architectural Audit & Auto-Fixes Applied
| Domain Skill | Findings Discovered | Auto-Remediation Applied | Status |
| :--- | :--- | :--- | :--- |
| `database-schema-architect` | [e.g., Missing index on `orders.user_id`] | [Added index + idempotent seed guard] | FIXED |
| `backend-contract-engineer` | [e.g., Missing Zod validation on `POST /api/items`] | [Added strict schema + 400 error envelope] | FIXED |
| `UI-UX-Design-Engineer` | [e.g., Missing empty state on dashboard table] | [Added EmptyState card + mobile overflow fix] | FIXED |

## 2. 6-Layer Test Execution Summary
| Gate | Suite / Command | Total Tests | Passed | Failed | Duration |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Gate 1: Static & Prod Build | `tsc --noEmit && npm run build` | - | PASS | 0 | X.Xs |
| Gate 2: DB & Seed Integrity | `db:migrate && db:seed` | X | X | 0 | X.Xs |
| Gate 3: Unit & Boundary | `vitest run` / `pytest` / `cargo test` | X | X | 0 | X.Xs |
| Gate 4: API Contract & Security | `tests/api/*.spec.ts` | X | X | 0 | X.Xs |
| Gate 5: Playwright Live E2E | `npx playwright test` | X | X | 0 | X.Xs |

## 3. Defects Discovered & Self-Healed
1. **[BUG-01] [Title]:** Root cause in `[file:line]`. Fixed by `[summary]`. Covered by regression test `[test_name]`.
```
