---
name: refactoring
description: Restructure existing code to improve readability, maintainability, and performance without changing its external behavior. Use when cleaning up technical debt, preparing code for new features, or responding to code review feedback about complexity.
---

# Refactoring

Structural changes that preserve behavior and reduce future cost.

## Before You Refactor

Three questions to avoid wasted effort:

1. **Is this code changing anyway?** Refactor-on-touch is cheap; refactoring untouched code is speculation
2. **Do we own the tests?** Without a safety net, refactoring is risky — add characterization tests first (see below)
3. **Is there a measurable problem?** "This feels messy" is not a justification; "this module takes 3 minutes to understand a 5-line change" is

## The Characterization Test

For untested code, write a characterization test before touching it:

1. Call the function/module with known inputs
2. Record the actual outputs
3. Write assertions that match those outputs
4. Now you can refactor — the test will tell you if behavior changed

These tests document current behavior, not correct behavior. If the current behavior is buggy, fix that separately.

## Refactoring Techniques

### Extract Method

When a function is doing multiple things, extract groups of related logic into named functions. Name describes *what*, not *how*:

```ts
// Before
function applyDiscount(items, code) {
  let total = items.reduce((s, i) => s + i.price, 0)
  let discount = total * 0.1
  if (code === "SAVE20") discount = total * 0.2
  return total - discount
}

// After
function applyDiscount(items, code) {
  const total = sumItems(items)
  const discount = calculateDiscount(total, code)
  return total - discount
}
```

### Replace Conditionals with Objects/Map

Long if-else chains or switch statements with similar structure → lookup table:

```ts
const DISCOUNT_RULES = {
  SAVE10: (t) => t * 0.1,
  SAVE20: (t) => t * 0.2,
  FREESHIP: (t) => t > 50 ? 0 : 5.99,
} as const
```

### Decompose Conditional

Extract each branch of a complex conditional into a named function:

```ts
if (isHoliday(date) && isPremium(member) && !hasRecentOrder(user)) {
  // ...
}
```

The extracted functions (`isHoliday`, `isPremium`, `hasRecentOrder`) now encode business rules that can be tested and reused independently.

### Separate Query from Command

Functions that return a value AND mutate state are the #1 source of surprise bugs:

```ts
// Bad
function processOrder(order) {
  // mutates order AND returns result
}

// Good
const validated = validateOrder(order)     // query
const saved = await saveOrder(order)       // command
```

## The Strangler Pattern

For large, tightly-coupled code that can't be safely extracted, introduce a new structure alongside the old one, route new code to the new structure, and incrementally migrate old callers. Never rewrite from scratch — you lose all the embedded knowledge of edge cases.

## What Nobody Mentions

The best refactoring is the one that makes the next change trivial. If you're refactoring but the next feature still feels hard, you refactored the wrong thing. Good refactoring makes the *next* change smaller — measure that, not the diff size.

## References

See `references/dead-code.md` for identifying and safely removing dead code patterns.
See `references/migration-strategies.md` for structured approaches to replacing legacy systems.
