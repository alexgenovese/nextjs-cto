# Next.js Expert CTO — Agent Skills

[![skills.sh](https://skills.sh/b/alexgenovese/nextjs-cto)](https://skills.sh/alexgenovese/nextjs-cto)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Agent Skills Compliant](https://img.shields.io/badge/Standard-Agent_Skills-4a90d9)](https://agentskills.io)

A composable library of AI agent skills that transforms Claude Code (or any agent supporting the [Agent Skills spec](https://agentskills.io)) into a **CTO-level technical leader** for Next.js projects.

Instead of a single monolithic system prompt, each capability lives in its own [`SKILL.md`](skills/<skill-name>/SKILL.md) file. The orchestrator skill — [`nextjs-expert-cto`](skills/nextjs-expert-cto/SKILL.md) — loads only what it needs, keeping context tight and reasoning sharp.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Technical Requirements](#technical-requirements)
- [Installing the Skills](#installing-the-skills)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Available Skills](#available-skills)
- [How Skills Work Together](#how-skills-work-together)
- [Contributing](#contributing)
- [License](#license)

---

## Project Overview

This repository implements a **modular AI agent skill system** following the [Agent Skills](https://agentskills.io) standard. Each skill is encapsulated in its own folder with:

- `SKILL.md` — the skill definition (YAML frontmatter with metadata + instructions)
- `references/` — supporting documentation, runbooks, and configuration templates
- `scripts/` — executable automation tools for repetitive or deterministic tasks

### Core Philosophy

| Principle | Description |
|-----------|-------------|
| **Composition over Inheritance** | Skills are independently discoverable and combinable |
| **Context Awareness** | Each skill knows its domain and knows when to invoke |
| **Minimal Context** | The orchestrator loads only what it needs per task |
| **Extensibility** | Adding new skills requires only a `SKILL.md` file |

### When to Use This Project

This project is designed for scenarios where:
- You need **architect-level judgment** on technical decisions
- You want **team-level insights** on delivery bottlenecks and velocity
- You're making **high-stakes decisions** that are hard to reverse
- You need **rapid extensibility** by adding new domain skills

### Example Prompts

```bash
# Architecture review
"Review my current Next.js architecture and flag the top 3 risks."

# CI/CD planning
"We're 3 engineers shipping to Vercel. What's the right CI/CD setup?"

# Decision framework
"Should I use Server Actions or a dedicated API route for checkout?"

# Using specific skills
"Using technical-decision-making: Compare tRPC vs REST for our backend."
```

---

## Technical Requirements

| Technology | Purpose |
|------------|---------|
| Node.js 18+ | For running `npx skills` commands |
| skills CLI | Core discovery and execution engine |
| Markdown | Documentation format |
| Git | Version control |

### Prerequisites

1. Install the [Agent Skills CLI](https://agentskills.io):
   ```bash
   corepack enable
   npx skills@latest add @anthropic/vercel-labs/skills
   ```

2. Add this repository as a source:
   ```bash
   npx skills add alexgenovese/nextjs-cto
   ```

3. Verify installation:
   ```bash
   npx skills list
   # Should show: nextjs-expert-cto and all sub-skills
   ```

---

## Installing the Skills

### Option 1: One-Command Install

```bash
npx skills add alexgenovese/nextjs-cto
```

### Option 2: Manual Installation

```bash
# Clone the repository
git clone https://github.com/alexgenovese/nextjs-cto.git
cd nextjs-cto

# Add as a skills source
npx skills add alexgenovese/nextjs-cto
```

### Available Skills After Installation

The `nextjs-expert-cto` orchestrator brings the following sub-skills:

| Skill | Description |
|-------|-------------|
| [`nextjs-expert-cto`](skills/nextjs-expert-cto/SKILL.md) | Main orchestrator — CTO-level reasoning and delegation |
| [`technical-decision-making`](skills/technical-decision-making/SKILL.md) | ADR-based technology evaluation framework |
| [`team-velocity`](skills/team-velocity/SKILL.md) | Bottleneck diagnosis and throughput optimization |
| [`deployment-strategy`](skills/deployment-strategy/SKILL.md) | CI/CD pipelines, rollback, and progressive delivery |
| [`quality-gates`](skills/quality-gates/SKILL.md) | Automated quality standards and CI checks |
| [`typescript-ecosystem`](skills/typescript-ecosystem/SKILL.md) | TypeScript configuration and advanced type patterns |
| [`api-design`](skills/api-design/SKILL.md) | REST/GraphQL design, versioning, error responses |
| [`cache-components-adoption`](skills/cache-components-adoption/SKILL.md) | Next.js Cache Components enablement |
| [`cache-components-optimizer`](skills/cache-components-optimizer/SKILL.md) | Static shell growth and navigation optimization |
| [`next-dev-loop`](skills/next-dev-loop/SKILL.md) | Runtime verification during development |
| [`skill-creator`](skills/skill-creator/SKILL.md) | Skill creation, evaluation, and optimization |

---

## Quick Start

### Using the Orchestror

```bash
npx skills call alexgenovese/nextjs-cto/nextjs-expert-cto --input "Review this architecture decision"
```

### Using Individual Skills

```bash
# Technical decision making
npx skills call alexgenovese/nextjs-cto/technical-decision-making --input "Should we use T3 stack or CREATE studio?"

# Team velocity analysis
npx skills call alexgenovese/nextjs-cto/team-velocity --input "Our PRs take 3 days to merge. What's the bottleneck?"
```

---

## Project Structure

```
skills/
├── api-design/              # REST/GraphQL design, error responses
│   ├── SKILL.md
│   ├── references/          # error-codes.md, rest-vs-graphql.md
│   └── scripts/             # (reserved for automation)
│
├── cache-components-adoption/  # Enable cacheComponents in Next.js 16.3+
│   ├── SKILL.md
│   ├── references/          # per-page-decisions.md
│   └── scripts/
│
├── cache-components-optimizer/ # Optimize static shell and nav
│   ├── SKILL.md
│   ├── references/          # instant-nav-loop.md, ppr-loop.md
│   └── scripts/
│
├── deployment-strategy/     # CI/CD, rollback, progressive delivery
│   ├── SKILL.md
│   ├── references/
│   └── scripts/
│
├── next-dev-loop/           # /_next/mcp + agent-browser dev verification
│   ├── SKILL.md
│   ├── references/
│   └── scripts/
│
├── nextjs-expert-cto/       # Main orchestrator skill
│   ├── SKILL.md
│   ├── examples/            # Usage examples
│   └── scripts/
│
├── quality-gates/           # CI checks, type checking, coverage budgets
│   ├── SKILL.md
│   ├── references/
│   └── scripts/             # coverage reports, bottleneck analysis
│
├── skill-creator/           # Create, test, and optimize skills
│   ├── LICENSE.txt          # Apache 2.0 (individual skill license)
│   ├── SKILL.md
│   ├── agents/              # analyzer.md, comparator.md, grader.md
│   ├── assets/              # eval_review.html
│   ├── eval-viewer/
│   ├── references/          # schemas.md
│   └── scripts/             # eval runners, report generators
│
├── team-velocity/           # Throughput measurement and improvement
│   ├── SKILL.md
│   ├── references/
│   └── scripts/
│
└── technical-decision-making/   # ADRs, build vs buy, RFC templates
    ├── SKILL.md
    ├── references/
    └── scripts/

AGENTS.md                    # Agent configuration and meta-testing
LICENSE                      # Main project license (not shown, implied)
README.md                    # This file
.prettierrc                  # Code formatting config
.gitignore                   # Git ignore patterns
```

### Skill Folder Anatomy

Each skill folder provides a standard structure:

```
skills/<skill-name>/
├── SKILL.md        # Required: name, description in YAML frontmatter
├── references/     # Optional: domain knowledge, runbooks, templates
└── scripts/        # Optional: automation tools
```

---

## Available Skills

### Orchestrator: [`nextjs-expert-cto`](skills/nextjs-expert-cto/SKILL.md)

**Description:** Technical leader and CTO specialized in Next.js — evaluates architecture decisions, manages delivery pipelines, ensures code quality, and guides team velocity.

**Key Capabilities:**
- Architecture trade-off evaluation with explicit risk assessment
- Cross-audience communication (engineers vs stakeholders)
- Trunk-based development advocacy
- CI/CD pipeline design

**When to invoke:**
- You need a second opinion on an architecture choice
- You want a roadmap review or technical strategy session
- You're about to make a decision that will be hard to reverse

---

### [`technical-decision-making`](skills/technical-decision-making/SKILL.md)

**Description:** Systematic framework for technical decisions using Architecture Decision Records (ADRs).

**Framework:** Uses the standard ADR format:
- **Context** — What forces are at play?
- **Options** — At least 2 real alternatives
- **Decision** — What we chose and why
- **Consequences** — What we gain, what we give up

Patterns covered:
- Build vs Buy vs Open Source
- Framework and library evaluation
- RFC writing

**When to invoke:**
- Choosing between frameworks, databases, or libraries
- Debat/build vs buy vs open source
- Writing a formal RFC to align the team

```bash
npx skills call alexgenovese/nextjs-cto/technical-decision-making \
  --input "Write an ADR for migrating pages/ to App Router"
```

---

### [`team-velocity`](skills/team-velocity/SKILL.md)

**Description:** Diagnoses bottlenecks in delivery workflow and prescribes targeted fixes.

**Metrics tracked:**
| Metric | What it tells you |
|--------|-------------------|
| Cycle time | From start to ship |
| Deployment frequency | How often we ship |
| Change fail rate | How often things break |
| Mean time to recovery | How fast we fix |

**Common bottlenecks and fixes:**
- **Code review** → Rotate reviewers, set SLA, smaller PRs
- **QA/testing** → Shift left: unit + integration before QA
- **Requirements** → Write acceptance criteria before coding
- **Deployment** → Automate pipeline, feature flags

**When to invoke:**
- PRs sitting in review for days
- Sprint commitments consistently missed
- Measuring cycle time or deployment frequency

```bash
npx skills call alexgenovese/nextjs-cto/team-velocity \
  --input "Our PRs take 3+ days to review. What's the fix?"
```

---

### [`deployment-strategy`](skills/deployment-strategy/SKILL.md)

**Description:** Designs release pipelines covering CI/CD, rollback, blue-green, canary, and progressive delivery.

**Deployment strategies comparison:**

| Strategy | Risk | Speed | Complexity | Best for |
|----------|------|-------|------------|----------|
| Feature flags | Lowest | Instant | Medium | Anything |
| Canary | Low | Gradual | High | Traffic-sensitive |
| Blue-green | Low | Immediate | Medium | Infra changes |
| Rolling update | Medium | Per-instance | Low | Stateless |

**Progressive delivery pattern:**
```
Internal → Beta (5%) → Gradual (25→50→75%) → Full (100%)
```

**Database migration rules:**
1. Backward-compatible (old code works with new schema)
2. Split into phases: schema → code → backfill → cleanup
3. Never lock production table > few seconds

**When to invoke:**
- Setting up or auditing CI/CD pipeline
- Planning risky database migration
- Choosing deployment strategy for new feature

---

### [`quality-gates`](skills/quality-gates/SKILL.md)

**Description:** Defines and enforces automated quality standards in CI.

**Gate pipeline (required):**

| Gate | Tooling | Purpose |
|------|---------|---------|
| Type checking | `tsc --noEmit` | Type errors |
| Linting | ESLint | Style, patterns |
| Formatting | Prettier | Consistency |
| Unit tests | Vitest/Jest | Logic errors |
| Build | `next build` | Compilation |
| Integration | Playwright/Vitest | Behavior |

**Optional, high-value gates:**
- Security scan (Snyk/npm audit)
- Bundle analysis (@next/bundle-analyzer)
- Performance budget (Lighthouse CI)
- Type coverage reporting

**Thresholds:**
- Test coverage: Start with 50% new code, increase over time
- Bundle size: Warn at +5%, block at +10%
- Type coverage: Target 90%+ on new code

**When to invoke:**
- Setting up CI checks for new project
- Defining "definition of done"
- Auditing current gate effectiveness

---

### [`typescript-ecosystem`](skills/typescript-ecosystem/SKILL.md)

**Description:** TypeScript configuration, type patterns, and ecosystem decisions.

**Key strict flags:**

| Flag | What it prevents | Cost |
|------|------------------|------|
| `strictNullChecks` | null/undefined access | Low |
| `noUncheckedIndexedAccess` | Unsafe object access | Medium |
| `exactOptionalPropertyTypes` | Accidental undefined writes | Medium |

**Type patterns:**
- **Branded types** for primitive safety
- **Discriminated unions** for state machines
- **Union types** over interfaces for data

**Library guidance:**
| Library | When to use | When to avoid |
|---------|-------------|---------------|
| Zod | Runtime validation | Internal-only types |
| tRPC | End-to-end TS | Non-TS clients |
| Prisma | Type-safe DB | Complex queries |
| Valibot | Small bundle | Need Zod ecosystem |

**When to invoke:**
- Starting new project and setting tsconfig
- Migrating codebase to strict mode
- Choosing Zod/Valibot/tRPC/Prisma

---

### [`api-design`](skills/api-design/SKILL.md)

**Description:** Designs APIs that are consistent, cacheable, and resilient.

**REST-First approach:**
URL structure:
```
POST   /api/v1/resource    # C-create
GET    /api/v1/resource    # R-list
GET    /api/v1/resource/:id # R-read
PATCH  /api/v1/resource/:id # U-update
DELETE /api/v1/resource/:id # D-delete
```

**Error response shape:**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Title must be at least 3 characters",
    "details": [
      { "field": "title", "code": "too_short", "min": 3 }
    ]
  }
}
```

**Versioning:** URL versioning (`/api/v1/`) from day one.

**When to invoke:**
- Designing new API endpoints
- Reviewing existing API for inconsistencies
- Deciding between REST and GraphQL

---

### [`cache-components-adoption`](skills/cache-components-adoption/SKILL.md)

**Description:** Step-by-step guide to enabling `cacheComponents: true` in Next.js 16.3+.

**Requirements:**
- Next.js 16.3+
- [agent-browser](https://agentskills.io/skills/agent-browser)

**When to invoke:**
- Enabling Cache Components on existing app
- Debugging `cacheComponents` build errors
- Running cache-components-instant-false codemod

---

### [`cache-components-optimizer`](skills/cache-components-optimizer/SKILL.md)

**Description:** After adoption, grows static shell on first paint and speeds up navigation.

**When to invoke:**
- First-paint feels slow after Cache Components enablement
- Navigation between routes shows loading
- Measuring and growing static shell

---

### [`next-dev-loop`](skills/next-dev-loop/SKILL.md)

**Description:** Fast edit/verify loop during `next dev` — combines `/_next/mcp` with `agent-browser`.

**Requirements:**
- Next.js 16.3+ with Turbopack
- `agent-browser` >= 0.27.0

**When to invoke:**
- Verifying streaming behavior after Server Component edits
- A change compiles but behaves wrongly in browser
- Inspecting React fiber state, console errors, network calls

---

### [`skill-creator`](skills/skill-creator/SKILL.md)

**Description:** Create new skills, modify existing skills, and measure performance with automated evals and benchmarks.

**Components:**
- **agents/**: analyzer.md, comparator.md, grader.md
- **scripts/**: eval runners, report generators, optimization tools
- **eval-viewer/**: HTML viewer for evaluation results

**When to invoke:**
- Creating a new skill from scratch
- Running evals on existing skill
- Optimizing trigger description

---

## How Skills Work Together

A typical feature from zero to production:

```mermaid
graph LR
    A[technical-decision-making] --> B[F: RFC: Server Actions vs API route]
    B --> C[typescript-ecosystem]
    C --> D[F: Type the request/response contract]
    D --> E[api-design]
    E --> F[F: Define error shapes and versioning]
    F --> G[cache-components-adoption]
    G --> H[F: Enable caching on new route]
    H --> I[next-dev-loop]
    I --> J[F: Verify runtime behavior]
    J --> K[cache-components-optimizer]
    K --> L[F: Grow static shell]
    L --> M[quality-gates]
    M --> N[F: CI checks before merge]
    N --> O[deployment-strategy]
    O --> P[F: Canary rollout to production]
```

### Practical Workflow

1. **Phase 1: Design & Contract**
   - `technical-decision-making` → Write ADR
   - `typescript-ecosystem` → Define types
   - `api-design` → Specify error responses

2. **Phase 2: Implementation & Verification**
   - `cache-components-adoption` → Enable caching
   - `next-dev-loop` → Verify in browser

3. **Phase 3: Quality & Deployment**
   - `cache-components-optimizer` → Optimize shell
   - `quality-gates` → CI checks
   - `deployment-strategy` → Progressive rollout

---

## Contributing

### Adding a New Skill

1. Create the skill folder structure:
   ```
   skills/<new-skill-name>/
   ├── SKILL.md      # Required: yaml frontmatter with `name` and `description`
   ├── references/   # Optional: domain documentation
   └── scripts/      # Optional: automation tools
   ```

2. Define the skill frontmatter:
   ```yaml
   ---
   name: <new-skill-name>
   description: Clear description of when to use this skill
   ---
   ```

3. Add to orchestrator:
   ```yaml
   # In skills/nextjs-expert-cto/SKILL.md
   skills:
     - <new-skill-name>
   ```

### Contributing Guidelines

- **Writing skills:** Follow SKILL.md format; keep under 500 lines; use imperative tone
- **Naming:** Use kebab-case for directory and YAML name field
- **Validation:** Every skill must have `scripts/` and `references/` directories (can be empty)
- **Documentation:** Add relevant references in `references/` for longer documentation

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

Individual skills may have their own licenses (e.g., [`skill-creator`](skills/skill-creator/LICENSE.txt) uses Apache 2.0).

---

*Built on the [Agent Skills](https://agentskills.io) standard — compatible with Claude Code, Cursor, GitHub Copilot, Amp, Goose, Windsurf, and Devin.*
