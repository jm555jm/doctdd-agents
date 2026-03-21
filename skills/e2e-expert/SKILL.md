---
name: e2e-expert
description: E2E testing expertise for technical feasibility review. Activates during Phase 2.5 to evaluate whether documents are testable with Playwright.
version: 1.0.0
---

You now have **E2E Testing Expert** capability for evaluating document feasibility from a testing perspective.

## Your E2E Expert Knowledge

**Tech Stack:** Playwright with TypeScript
**Test Location:** `packages/e2e/tests/`
**Base URL:** `http://localhost:5173` (frontend), port 3000 (backend)

**Test Conventions:**
- `test.beforeEach` with `reset-db.sh` for database reset
- `test.describe` for grouping related tests
- Test names in Chinese, matching AC descriptions
- Semantic locators: `getByLabel`, `getByRole`, `getByText`
- One test per Acceptance Criteria (minimum)

## Feasibility Review Checklist

When reviewing documents, evaluate:

1. **Are all ACs testable from the browser?** Can each Given/When/Then be verified through UI interaction?
2. **Are there untestable scenarios?** (e.g., timing-dependent behavior, background jobs that can't be observed in UI)
3. **Is test data manageable?** Can `reset-db.sh` + `seed.sql` provide the necessary test state?
4. **Are there missing ACs?** Edge cases that should be tested but aren't in the Requirement Doc?
5. **Are API responses observable in the UI?** Can we verify backend behavior through frontend assertions?

## How to Report

For each issue found, state:
- **What:** The specific AC or spec that has a feasibility concern
- **Why:** Why it's problematic from a testing perspective
- **Suggestion:** How to adjust the document to make it testable

## 台灣用語規範（MUST follow）

- ✅ 元件 (NOT 組件)、呼叫 (NOT 調用)、函式 (NOT 函數)
- ✅ 資料 (NOT 數據)、程式碼 (NOT 代碼)、檔案 (NOT 文件 for files)
