---
name: playwright
description: Playwright E2E testing technical knowledge — test conventions, locator patterns, configuration, and project structure. Loaded into e2e-expert agent during Phase 3.
version: 1.0.0
---

You now have **Playwright** technical knowledge for writing E2E tests.

## Test Conventions

```typescript
import { test, expect } from '@playwright/test'
import { execFileSync } from 'child_process'
import path from 'path'

test.beforeEach(() => {
  const scriptPath = path.resolve(__dirname, '../scripts/reset-db.sh')
  execFileSync('bash', [scriptPath], { stdio: 'inherit' })
})

test.describe('Feature Name', () => {
  test('AC description in Chinese', async ({ page }) => {
    await page.goto('/path')
    await expect(page.getByLabel('欄位名')).toBeVisible()
  })
})
```

## Key Patterns

- Use `test.beforeEach` with `reset-db.sh` for database reset
- Group related tests with `test.describe`
- Test names in Chinese, matching the AC description
- Use semantic locators: `getByLabel`, `getByRole`, `getByText`
- Base URL is configured in `playwright.config.ts`
- One test per Acceptance Criteria (minimum)
- Cover both positive and negative cases
- Tests must be independent (no order dependency)
- Use meaningful assertions that match the AC's "Then" clause

## Project Structure

```
packages/e2e/
├── tests/              # Test files (*.spec.ts)
├── scripts/
│   └── reset-db.sh     # Database reset before each test
├── playwright.config.ts # Playwright configuration
├── global-setup.ts      # Global setup (if exists)
└── package.json
```

## Documentation Lookup

Before using Playwright APIs, check Context7 for up-to-date documentation:
- `mcp__context7__resolve-library-id` to find Playwright
- `mcp__context7__get-library-docs` to fetch API docs
