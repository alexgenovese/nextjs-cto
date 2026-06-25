# Per-Page Decisions for Cache Components Adoption

## When to leave a Block in place

Some routes legitimately cannot be prerendered. Leave `export const instant = false` on them, but rewrite the TODO comment to document the reason:

```ts
// Cache Components: this route uses SSO callback params that
// must be read at request time. Keeping instant=false.
export const instant = false
```

Valid reasons to keep a block:
- Route reads `searchParams` for a search/results page and the param structure is too complex to push into child components
- Route uses `cookies()` in a layout for A/B testing and the test framework requires per-request values
- Route is a catch-all that renders dynamic user content

## Sync-IO: Date.now / Math.random

When you find sync-IO at module or render time:

1. If the value is stable (copyright year, build timestamp) → `'use cache'` with a long `cacheLife` profile
2. If the value is per-request (request ID, timestamp for logging) → `await connection()` + `<Suspense>`
3. If the value is truly dynamic (Math.random for a unique key) → `await connection()` + generate in the client

## Route table glyphs

- `○ (Static)` — fully prerendered, no streaming
- `◐ (Partial Prerender)` — static shell + streamed data (goal state)
- `ƒ (Dynamic)` — fully dynamic, no shell
