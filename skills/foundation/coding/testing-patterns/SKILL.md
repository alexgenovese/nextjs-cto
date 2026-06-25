---
name: testing-patterns
description: Write effective tests at every level — unit, integration, and E2E. Use when adding tests to a project, reviewing test coverage, designing a testing strategy, or debugging flaky tests.
---

# Testing Patterns

Practical patterns for tests that catch regressions without becoming maintenance burden.

## The Testing Trophy

Not the pyramid. Write most tests at the integration level, fewer at unit and E2E:

- **Unit tests** (10%) — pure functions, utilities, complex logic with no I/O
- **Integration tests** (70%) — components with their dependencies, API routes, database operations
- **E2E tests** (20%) — critical user flows only (login, checkout, core feature)

Integration tests give the best confidence-to-maintenance ratio because they test real interactions without the fragility of full browser automation.

## What to Test

Test behavior, not implementation. A good test tells you what went wrong, not just *that* something went wrong.

| Test | Implementation detail? | Should you test it? |
|------|----------------------|---------------------|
| "Clicking submit calls the API with correct data" | No — behavior | Yes |
| "Component renders with `data-testid="form"`" | Yes — structure | No |
| "formatDate returns 'Jan 5, 2024' for 2024-01-05" | No — pure output | Yes |
| "useState was called with initial value X" | Yes — implementation | No |
| "User sees error toast when API returns 500" | No — behavior | Yes |

**Rule of thumb**: if the test breaks when you refactor (without changing behavior), it's testing implementation. Fix the test, not the code.

## Testing Async & Data

- Stub at the boundary you own: for API calls, mock `fetch` at the network layer, not the module
- For DB operations, use a test database or in-memory alternative — never mock the ORM
- Assert on the rendered/returned output, not on intermediate state
- Use `waitFor` / `findBy` for async rendering, not arbitrary timeouts

## Testing Next.js (App Router)

- Server Components: test data transformation logic separately, test the component with rendered output
- Server Actions: call them directly in tests with mock formData, assert on the return value or side effects
- Client Components: use `@testing-library/react`, wrap in necessary providers (Theme, QueryClient, etc.)
- API Routes: use `fetch` or `supertest` against the route handler directly

## Flaky Tests

Flaky tests lose trust faster than missing coverage. If a test fails intermittently:

1. Check for timing assumptions (fixed waits instead of `waitFor`)
2. Check for shared mutable state between tests (not cleaning DB, not resetting mocks)
3. Check for order-dependent tests (relying on previous test's side effects)
4. If it flakes more than once in CI, quarantine it — don't ignore, don't retry

## References

See `references/react-testing.md` for React-specific testing patterns (mocking hooks, portals, context providers).
See `references/e2e-playwright.md` for Playwright setup and best practices.
