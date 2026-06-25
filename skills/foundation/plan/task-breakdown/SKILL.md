---
name: task-breakdown
description: Decompose complex engineering work into granular, estimable, and independently deployable tasks. Use when planning a new feature, refactoring, or any multi-step engineering effort — from requirements to shippable increments.
---

# Task Breakdown

Turn ambiguous requirements into a structured execution plan.

## Principles

1. **Increments are independent** — each task should be shippable without waiting for another
2. **Smallest verifiable unit** — a task is done when there's something demonstrable (a passing test, a rendered page, a merged PR)
3. **Risk first** — tackle uncertainty early: spike unknowns, validate assumptions, then commit
4. **Traceability** — every task ties back to a requirement, acceptance criteria, or user need

## Breakdown Process

### 1. Surface the boundaries

Ask these questions before writing a single task:

- What's the user-visible outcome? (feature, fix, improvement)
- What systems does this touch? (DB, API, UI, auth, cache, queue)
- What don't we know? (3rd-party behavior, performance profile, migration path)
- What's the smallest thing we can ship and call "done"?

### 2. Identify the shape

Most work falls into one of these patterns:

| Pattern | Structure | Example |
|---------|-----------|---------|
| **Vertical slice** | Full-stack, one feature end-to-end | "User can reset password via email link" |
| **Horizontal layer** | One concern across the app | "Add structured logging to all API routes" |
| **Migration** | Old → new, kept running | "Migrate payments from Stripe to LemonSqueezy" |
| **Spike** | Investigation + report | "Benchmark Postgres vs SQLite for our read pattern" |

### 3. Write tasks

Each task must have:

- **Title**: imperative verb + scope ("Add validation to create-post endpoint")
- **Acceptance criteria**: observable, testable ("POST /api/posts returns 422 with field-level errors when title is missing")
- **Dependencies**: things that must be done first ("Blocked on API key provisioning")
- **Estimated size**: small (<1d), medium (1-3d), large (3-5d — should be broken down further)

Avoid: vague titles ("Refactor auth"), missing acceptance criteria, mixed concerns in one task.

### 4. Order the work

- Dependency chain first (DB schema before API before UI)
- Riskiest items early (spike the unknown API before building around it)
- Visible value early (a working but incomplete feature beats a perfect backend)
- Review + merge each task independently — no mega-branches

## What Nobody Mentions

Task breakdown is a communication tool first, a planning tool second. A well-broken list surfaces disagreements about scope and approach before code is written — that's the real value. If the team argues about tasks, you've found ambiguity that would have been a merge conflict later.

## References

See `references/estimation.md` for techniques to size tasks reliably.
See `references/pr-template.md` for the PR structure that maps one task → one PR.
