# Nav Loop (Instant Navigation Loop)

Optimize in-app navigation between two routes. Run this when navigation from A to B holds A's UI until B's data resolves.

## capture baseline

1. Navigate to route A in the browser.
2. Set the instant cookie: `agent-browser cookies set next-instant-navigation-testing '[0,"p<random>"]' --url <origin>`
3. Simulate SPA navigation to B (this simulates what production would prerender without needing prefetch to actually fire):
   ```
   agent-browser eval "window.next.router.push('/route-b')"
   ```
4. Wait for DOM stabilisation (poll `document.documentElement.innerHTML.length` until unchanged across two reads).
5. Capture the React tree: `agent-browser react suspense --json`
6. Screenshot: hide dev overlay → screenshot → restore overlay.
7. Classify each suspended boundary by its `suspended_by[].name`.

## classify suspended boundaries post-nav

Use the same three buckets as the PPR loop:

- **Data-dependent** — fetch, cache, use, async component
- **Library-induced** — usePathname, useSearchParams, cookies, headers
- **State-dependent** — useState → drop SSR-only client hooks

The difference from the PPR loop: nav optimization targets boundaries that suspend on **client navigation**, not initial page load. A boundary that renders fine on first paint may still suspend on SPA nav if it reads request-time data.

## apply fix

Same levers as the PPR loop: push down, cache, drop SSR-only hooks. The key difference: pushing a boundary down for nav means ensuring the parent component tree (layout + shared chrome) renders without suspending.

## verify

After each fix, re-run the SPA nav simulation and re-capture. The capture must show more content on first paint (fewer/smaller fallbacks). If the capture is identical, undo and try a different approach.

## done

When either:
- Navigation from A to B shows B's layout + chrome immediately (only the genuinely dynamic content suspends)
- Or all remaining suspended boundaries wrap content that requires per-request data

Capture the final screenshot. Report before/after in the summary.
