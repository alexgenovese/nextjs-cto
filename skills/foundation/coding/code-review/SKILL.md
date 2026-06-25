---
name: code-review
description: Systematic code review focused on correctness, maintainability, performance, and security. Use when reviewing pull requests, preparing code for review, or establishing team review standards.
---

# Code Review

Structured review methodology organized by concern.

## Before Reviewing

1. Understand the intent — read the PR description and linked issue first
2. Check the diff size — if > 400 lines, ask to split into smaller PRs
3. Pull the branch and run it locally for non-trivial changes

## Review Taxonomy

Review in this order, from most to least impactful:

### 1. Correctness

Does the code do what it claims?

- Off-by-one errors, null/undefined paths, race conditions
- Error handling: every fetch/DB call has a `.catch()` or try/catch
- Edge cases: empty state, pagination boundaries, auth failures
- No silent failures — errors are surfaced, logged, or propagated

### 2. Maintainability

Will someone understand this in 6 months?

- Is the naming consistent with the rest of the codebase?
- Are side effects documented or obvious?
- Is the logic duplicated elsewhere? (signal to extract)
- Complexity: can this function/component be split without losing clarity?
- Every abstraction should pull its weight — if extracting a helper saves only one repetition, it may not be worth it

### 3. Performance

- N+1 queries: is there a loop making separate DB/API calls?
- Unnecessary re-renders in client components (missing `useMemo`/`useCallback` for referentially stable props)
- Bundle impact: is a large library imported only for a single utility function?
- Data fetching in a loop instead of batching

### 4. Security

- User input is validated with a schema (Zod, Valibot, etc.) — never trusted directly
- SQL queries use parameterized statements, not string interpolation
- Auth checks exist on every protected route/action, not just the client
- Tokens, secrets, API keys are never logged, committed, or exposed to the client
- CSRF protection for state-changing endpoints

### 5. Testing

- Does the PR include tests for the new behavior?
- Are the tests asserting behavior or implementation? (prefer behavior)
- Do error paths have test coverage?
- Are there tests for the edge cases identified during correctness review?

## Review Etiquette

- Explain the *why* behind each suggestion, not just the *what*
- Distinguish: blocking (must fix), recommendation (should fix), nitpick (optional)
- Offer specific alternatives, not vague complaints
- Praise good patterns — positive reinforcement sets standards faster than corrections

## References

See `references/review-checklist.md` for the full checklist.
See `references/dx-patterns.md` for codebase conventions and structural patterns.
