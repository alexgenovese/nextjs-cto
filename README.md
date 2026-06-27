# Next.js Expert CTO — Agent Skills

A composable library of AI agent skills that turns Claude Code (or any agent supporting the [skills spec](https://agentskills.io)) into a **CTO-level technical leader** for Next.js projects.

Instead of a single giant system prompt, each capability lives in its own `SKILL.md`. The orchestrator skill — `nextjs-expert-cto` — loads only what it needs, keeping context tight and reasoning sharp.

## Install in one command

```bash
npx skills add alexgenovese/nextjs-cto
```

---

## What you get

### `nextjs-expert-cto` — the orchestrator

The top-level skill. It thinks like a technical leader: evaluates trade-offs, owns delivery risk, communicates decisions to both engineers and stakeholders, and delegates to the right sub-skill automatically.

**When to invoke it:**
- You need a second opinion on an architecture choice
- You want a roadmap review or a technical strategy session
- You're about to make a decision that will be hard to reverse

**Example prompts:**
```
"Review my current Next.js architecture and flag the top 3 risks."
"We're 3 engineers shipping to Vercel. What's the right CI/CD setup for us?"
"Should I use Server Actions or a dedicated API route for this checkout flow?"
```

---

## Sub-skills

### `technical-decision-making`

Evaluates any technology choice using a structured ADR (Architecture Decision Record) format: context → options → decision → consequences.

**Use it when:**
- Choosing between frameworks, databases, or libraries
- Debating build vs. buy vs. open source
- Writing a formal RFC to align the team

**Example prompts:**
```
"Using technical-decision-making: should we adopt tRPC or keep our REST API?"
"Write an ADR for migrating our pages/ directory to the App Router."
"We're evaluating Supabase vs. PlanetScale. Help me think through the trade-offs."
```

---

### `team-velocity`

Diagnoses bottlenecks in your delivery workflow and prescribes targeted fixes — without burning out the team.

**Use it when:**
- PRs are sitting in review for days
- Sprint commitments are consistently missed
- You want to measure cycle time or deployment frequency

**Example prompts:**
```
"Our PRs take 3+ days to review. What's the fix?"
"Map our workflow and identify where we're losing the most time."
"How should we structure a sprint to keep 20% capacity for unplanned work?"
```

---

### `deployment-strategy`

Designs release pipelines from commit to production, covering CI/CD, rollback, blue-green, canary, and feature flag strategies.

**Use it when:**
- Setting up or auditing a CI/CD pipeline
- Planning a risky database migration
- Choosing a deployment strategy for a new feature

**Example prompts:**
```
"Design a zero-downtime deployment pipeline for our Vercel + Postgres stack."
"We need to ship a breaking schema change. Walk me through the migration steps."
"Set up a canary release for our new checkout flow — 5% of traffic first."
```

---

### `quality-gates`

Defines and enforces automated quality standards: type checking, linting, test coverage, bundle size budgets, and performance thresholds.

**Use it when:**
- Setting up CI checks for a new project
- Defining a team's "definition of done"
- Auditing whether current gates are worth keeping

**Example prompts:**
```
"Define the quality gates for our Next.js monorepo — types, lint, tests, build."
"Our bundle increased by 40KB in the last sprint. Set up a size budget check."
"We have 12 ESLint rules disabled. Help me decide which ones to re-enable."
```

---

### `typescript-ecosystem`

Configures TypeScript for maximum safety without making the team hate it — strict mode, branded types, discriminated unions, and the right library choices.

**Use it when:**
- Starting a new Next.js project and setting up `tsconfig.json`
- Migrating an existing codebase to strict mode
- Choosing between Zod, Valibot, tRPC, or Prisma

**Example prompts:**
```
"Set up a strict tsconfig for our Next.js 15 project."
"We keep getting runtime errors from external API responses. Add Zod validation."
"Model our order state machine with discriminated unions — idle, pending, fulfilled, failed."
```

---

### `api-design`

Designs APIs that are consistent, cacheable, and resilient — REST vs. GraphQL, URL structure, versioning, and error response shapes.

**Use it when:**
- Designing new API endpoints from scratch
- Reviewing an existing API for inconsistencies
- Deciding between REST and GraphQL for a new client

**Example prompts:**
```
"Design the REST API for a multi-tenant SaaS — users, workspaces, and invitations."
"Our error responses are inconsistent across endpoints. Define a standard shape."
"When does it make sense to add GraphQL on top of our existing REST API?"
```

---

### `cache-components-adoption`

Step-by-step guide to enabling `cacheComponents: true` in Next.js 16.3+ — finds every blocking route, runs the codemod, and walks each route to a passing build.

**Use it when:**
- Enabling Cache Components on an existing app
- Debugging `cacheComponents` build errors
- Deciding whether to opt routes out or fix them in place

**Example prompts:**
```
"Enable Cache Components on my app. Start with the incremental strategy."
"My build fails with a blocking prerender error on /dashboard. Fix it."
"Run the cache-components-instant-false codemod and explain what it changed."
```

> **Requires:** Next.js 16.3+, `agent-browser` (`npx skills add vercel-labs/agent-browser`).

---

### `cache-components-optimizer`

After adoption, grows the static shell on first paint and speeds up in-app navigation — screenshots before/after to prove the delta.

**Use it when:**
- First-paint feels slow even after Cache Components is enabled
- Navigation between routes still shows a loading state
- You want to measure and grow the static shell of a specific page

**Example prompts:**
```
"Optimize the static shell of /products/[slug] — show me a before/after screenshot."
"Navigation from /dashboard to /settings is slow. Run the nav optimization loop."
"Which Suspense boundaries on /checkout are blocking the static shell?"
```

> **Requires:** `cacheComponents: true` already enabled, `agent-browser`.

---

### `next-dev-loop`

The fast edit/verify loop during `next dev` — combines `/_next/mcp` (framework view) with `agent-browser` (browser view) to confirm changes actually work at runtime, not just at build time.

**Use it when:**
- You edited a Server Component and want to verify streaming behavior
- A change compiles but behaves wrong in the browser
- You want to inspect React fiber state, console errors, or network calls during dev

**Example prompts:**
```
"Run the dev loop preflight and confirm both /_next/mcp and agent-browser are live."
"I moved this fetch into a Suspense boundary — verify it actually streams in the browser."
"Check for compilation issues and runtime errors on /account after my last edit."
```

> **Requires:** Next.js 16.3+ with Turbopack, `agent-browser` >= 0.27.0.

---

## How skills work together

A typical feature from zero to production:

```
1. technical-decision-making  →  RFC: Server Actions vs API route
2. typescript-ecosystem        →  Type the request/response contract
3. api-design                  →  Define error shapes and versioning
4. cache-components-adoption   →  Enable caching on the new route
5. next-dev-loop               →  Verify runtime behavior in dev
6. cache-components-optimizer  →  Grow the static shell
7. quality-gates               →  CI checks before merge
8. deployment-strategy         →  Canary rollout to production
```

---

## Repository layout

```
skills/
├── nextjs-expert-cto/          # Orchestrator — loads sub-skills automatically
├── technical-decision-making/  # ADRs, build vs buy, RFC templates
├── team-velocity/              # Bottleneck analysis, sprint health
├── deployment-strategy/        # CI/CD, rollback, progressive delivery
├── quality-gates/              # Lint, types, coverage, bundle budgets
├── typescript-ecosystem/       # tsconfig, Zod, tRPC, branded types
├── api-design/                 # REST, GraphQL, error shapes, versioning
├── cache-components-adoption/  # Enable cacheComponents, fix blockers
├── cache-components-optimizer/ # Grow static shell, optimize nav
└── next-dev-loop/              # /_next/mcp + agent-browser dev loop
```

Each skill folder contains:
- `SKILL.md` — the skill itself (frontmatter + instructions)
- `references/` — domain knowledge, runbooks, config templates
- `scripts/` — automation tools (coverage reports, bottleneck analysis)

---

## Contributing

1. Create `skills/<skill-name>/SKILL.md` with `name` and `description` in the YAML frontmatter.
2. Add `references/` and `scripts/` as needed.
3. Add the skill name to the `skills:` list in `nextjs-expert-cto/SKILL.md`.

---

*Built on the [Agent Skills](https://agentskills.io) standard.*
