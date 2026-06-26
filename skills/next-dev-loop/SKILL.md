---
name: next-dev-loop
description: >
  Verify Next.js runtime behavior after editing app code. Combines /_next/mcp
  (Next.js's view) with agent-browser (the browser's view). Requires a running
  `next dev`. Use this to confirm a change actually works — not just that it
  compiles or type-checks.
---

# next-dev-loop

The edit/verify rhythm during `next dev` — make a change, then confirm it actually works at runtime, not only that the types or the build are happy.

You verify through two views of the same running app:

- **`/_next/mcp`** — an HTTP endpoint Next.js exposes about itself. Knows framework-specific things: routes, segments, RSC, server actions, server logs, and errors as Next.js saw them. Call `tools/list` for the current surface.
- **`agent-browser`** — a CLI that drives a real Chrome. Knows framework-agnostic browser things: DOM, console, network, React fiber, vitals. Run `agent-browser --help` for the current surface.

The two views cross-check each other.

> **agent-browser dependency:** This skill requires `agent-browser >= 0.27.0`. Before proceeding, **STOP and ask the user to install it**:
> ```
> npx skills add https://www.skills.sh/vercel-labs/agent-browser/agent-browser
> ```
> Wait for confirmation. If the user declines, this skill cannot run — fall back to manual `next dev` + build verification.

## requires

- Next.js **16.3+** with **Turbopack** — `/_next/mcp` plus the proactive compile check via `get_compilation_issues`.
- `agent-browser` **>= 0.27.0** — when React introspection landed.

These are hard floors. If anything is missing, tell the user how to upgrade and stop. Don't fall back to grepping source or a weaker probe.

- Upgrade Next.js: `pnpm next upgrade` (or `npx next upgrade`).
- Install or upgrade `agent-browser`: `npm i -g agent-browser@latest`. If the CLI isn't on `PATH`, install it before continuing.

## preflight

Once per session, confirm both views are live.

1. **Open `agent-browser` at the target URL, restoring saved login state when present.** Build the `open` command from:
   - `--session <name>` where `<name>` is the project directory basename.
   - `--state ~/.agent-browser/sessions/<name>-default.json` if that file exists. Omit on first run.
   - `--headed --enable react-devtools`. If the daemon is already running, launch flags are ignored — run `agent-browser close` first, then re-open.

   If state was not restored and the page is gated, the user drives the login — pause until they confirm.

2. POST `tools/list` to `/_next/mcp`. Send `Accept: application/json, text/event-stream`; responses are SSE-framed, strip the `data: ` prefix before parsing JSON.
   - Unreachable → either `next dev` isn't running, or Next.js is below 16.3.
   - `get_compilation_issues` not in the list → Next.js below 16.3. Refuse.
3. `mcp get_compilation_issues` doubles as a Turbopack probe. An error response of `"Turbopack project is not available..."` means webpack. Refuse.
4. `mcp get_routes` → your route map for the rest of the session.

## loop

### before the edit — narrow the scope

Ask the running app, not the codebase. `/_next/mcp` knows which files rendered the current route; use those as your search scope.

### after the edit — verify

Four failure modes. Check each:

- **Compiles** — `mcp get_compilation_issues`.
- **Runs without errors** — `/_next/mcp` (server and bubbled-up browser errors both surface here).
- **Behaves as intended** — `agent-browser` drives the page; assert what the user actually sees.
- **React-level behavior** — `agent-browser` with react-devtools enabled exposes the component tree, props, state, and render counts.

Pick the specific tool from `tools/list` or `agent-browser --help` rather than from memory.

## reference

All tools below are present once preflight passes.

```
# /_next/mcp                 notes
get_project_metadata         projectPath, devServerUrl, bundler
get_routes                   fs-scan; no browser session needed
get_errors                   runtime + build; needs a browser session;
                             includes browser-side errors caught by the
                             dev server
get_page_metadata            segment trie + routerType; needs a browser
                             session; use as a discovery shortcut for
                             which files power a route
get_logs                     returns logFilePath
get_server_action_by_id      hashed id → file + functionName
get_compilation_issues       Turbopack only; errors on webpack
                             ("Turbopack project is not available")
```

## teardown

Close the `agent-browser` session — `--session` writes state to disk so the next loop's `--state` restores login. Leave `next dev` up for the next loop.
