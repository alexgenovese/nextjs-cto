# Page-Render Loop (PPR Loop)

Grow the static shell of a single page. Run this when the user reports one route's initial load is too slow.

## capture baseline

1. Navigate to the target route in the browser. Wait for the DOM to stabilize.
2. Set the instant cookie: `agent-browser cookies set next-instant-navigation-testing '[0,"p<random>"]' --url <origin>`
3. Hide the dev overlay and screenshot:
   ```
   agent-browser eval "document.querySelector('nextjs-portal').style.display='none'"
   agent-browser screenshot <path>/baseline.png
   agent-browser eval "document.querySelector('nextjs-portal').style.display=''"
   ```
4. Capture the React tree: `agent-browser react suspense --json`
5. Classify each Suspense boundary from the JSON output.

## classify fallback areas

From the JSON output, read each entry's `jsx_source` + `suspended_by[]`. Suspended boundaries fall into three buckets:

- **Data-dependent** — `suspended_by` includes `fetch`, `cache`, `use`, or an async component. Push-down: extract the data read into a leaf component, wrap with `<Suspense>`, lift static siblings above the boundary.
- **Library-induced** — `suspended_by` includes `usePathname`, `useSearchParams`, `cookies`, `headers`. These are framework API calls; the component using them must suspend. Push the component deeper and keep the boundary tight.
- **State-dependent** — `suspended_by` includes `useState`, `useReducer`, `useEffect` with external read. SSR-only client hooks can be dropped from the server pass (use a leaf Client Component with `'use client'`).

## rank by shell impact

Sort boundaries by how much of the visible page they control. A boundary that wraps the entire main content area is higher priority than one wrapping a sidebar badge.

## apply fix

For each boundary, pick the lever:

1. **Push down** — extract the dynamic read into a child component, wrap with `<Suspense>`, move static siblings outside the boundary
2. **Cache** — wrap data fetches in `'use cache'` with `cacheLife(<profile>)`. Ask the user for freshness requirements.
3. **Drop SSR-only hooks** — move client-only interactions to a leaf Client Component

## re-capture

After each fix, re-screenshot. The capture must visibly differ from the previous one. If not, undo and try a different approach.

## ranking for next iteration

After the fix, re-run `agent-browser react suspense --json`. Re-rank boundaries by shell impact. The next fix targets the new highest-impact boundary.

## done

When either:
- The shell renders everything the user expects on first paint (no visible fallbacks)
- Or the remaining suspended boundaries all wrap genuinely dynamic content that can't be cached or pushed

Capture the final screenshot. Report before/after paths in the summary.
