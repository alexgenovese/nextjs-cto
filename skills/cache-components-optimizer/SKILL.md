---
name: cache-components-optimizer
description: >
  Optimize a Next.js app that has `cacheComponents: true` — grow the static
  shell on first paint, and optimize in-app navigation between routes. Picks
  the matching diagnostic loop and runs it.
---

# cache-components-optimizer

Two loops, shared levers and primitives, different diagnostics:

- **Page-render loop** ([references/ppr-loop.md](./references/ppr-loop.md)) — grow the static shell of a single page. Rank Suspense fallback areas on a shell-only render.
- **Nav loop** ([references/instant-nav-loop.md](./references/instant-nav-loop.md)) — when the user clicks a link from A to B, show B's static layout immediately instead of holding A's UI until B's data resolves.

Pick one and run it end-to-end.

> **agent-browser dependency:** This skill uses `agent-browser` for screenshot capture and runtime verification. Before proceeding, **STOP and ask the user to install it**:
> ```
> npx skills add https://www.skills.sh/vercel-labs/agent-browser/agent-browser
> ```
> Wait for confirmation before continuing. Without `agent-browser`, you cannot capture before/after screenshots or verify the visible delta.

## requires

- `next-dev-loop` initiated for this session — it opens the headed browser, exposes the `agent-browser` CLI, and wires the dev MCP server.
- `cacheComponents: true` in `next.config.ts`. Refuse otherwise.

## preflight (shared)

1. Confirm `cacheComponents: true`.
2. **The user must already be at the page each loop needs** in the headed browser — logged in, with any state set up. This skill can't drive auth, SSO, or MFA; it takes the manual setup as the starting point.
3. `agent-browser get url` to anchor the current route.

Each loop sets the instant cookie as needed.

## instant cookie (shared)

Both loops use the `next-instant-navigation-testing` cookie to freeze the framework's dynamic-data writes. Once set, visible content on the page is the static shell + Suspense fallbacks — that's what we capture to assess the optimization.

Set it with a pending-lock tuple `[0, "<unique-id>"]`:

```
agent-browser cookies set next-instant-navigation-testing '[0,"p<random>"]' --url <origin>
```

Each loop's preflight specifies when to set it within the flow. Clear it at teardown.

## decide which loop

- **Page-render loop** when the complaint is about one route's initial load. Read [references/ppr-loop.md](./references/ppr-loop.md).
- **Nav loop** when it's about navigating between two routes. Read [references/instant-nav-loop.md](./references/instant-nav-loop.md).

Ambiguous → ask.

## shared refactor levers

- **Push down** — extract I/O into a Suspense-wrapped child so the parent stays static and static siblings lift into the shell.
  - **Recurse, don't blind-wrap.** If a Suspense boundary already wraps a component containing both static content and the I/O, read inside, extract the I/O-dependent JSX into a new leaf, and lift the static siblings up.
- **Cache** — `'use cache'` + `cacheLife(<profile>)`. Always ask the user for freshness; map to a preset (`seconds` / `minutes` / `hours` / `days` / `weeks` / `max` / `default`).

Push-down and cache compose: push-down lifts static structure, cache eliminates the remaining data gap.

## verify requires a visible delta (shared)

Each loop captures a baseline screenshot of the shell before applying any change, then re-screenshots after. Report both paths in the final summary so the user can see what changed. The two captures must visibly differ. Identical-looking captures mean the refactor didn't land; undo. "Compiles cleanly" is not the bar.

**Hide the dev overlay before each screenshot:**

```
agent-browser eval "document.querySelector('nextjs-portal').style.display='none'"
agent-browser screenshot <path>
agent-browser eval "document.querySelector('nextjs-portal').style.display=''"
```

## anti-patterns (shared)

**Don't replace granular Suspense boundaries with a top-level loading skeleton.** A `loading.tsx` for the whole segment, or a root-level `<Suspense fallback={<Skeleton />}>` that blanks the UI, defeats this skill's optimization.

## gotchas (shared)

- Dev doesn't prefetch the way production does, and routes compile on first hit — wait for the DOM to stabilize before capturing.
- Don't verify nav prefetch by inspecting dev network traffic — dev doesn't fire prefetch requests at all. Use the cookie-locked SPA-nav recipe in the nav loop.
- The diagnose pipeline can be flaky. When a result feels off, re-run 2–3 times and cross-check.

## teardown (shared)

Delete the cookie by name:

```
agent-browser cookies set next-instant-navigation-testing x --url <origin> --expires 1
```

Never `agent-browser cookies clear` — wipes auth.
