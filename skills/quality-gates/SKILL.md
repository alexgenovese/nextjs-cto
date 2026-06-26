---
name: quality-gates
description: Define and enforce automated quality standards throughout the development lifecycle. Use when setting up CI checks, defining test coverage requirements, establishing performance budgets, or creating a definition of done for PRs.
---

# Quality Gates

Automated checks that prevent regressions without requiring human review for every detail.

## The Gate Pipeline

Every PR must pass these gates before merging:

| Gate | Tooling | What it catches |
|------|---------|-----------------|
| Type checking | `tsc --noEmit` | Type errors, null/undefined paths |
| Linting | ESLint + `@typescript-eslint` | Style, anti-patterns, dead imports |
| Formatting | Prettier | Inconsistent style (auto-fix in CI) |
| Unit tests | Vitest / Jest | Logic errors in isolation |
| Build | `next build` | Compilation errors, bundle issues |
| Integration tests | Playwright / Vitest | Component + API behavior |

**Optional, high-value:**

| Gate | Tooling | What it catches |
|------|---------|-----------------|
| Security scan | Snyk / npm audit | Known vulnerabilities in deps |
| Bundle analysis | `@next/bundle-analyzer` | Unexpected bundle size increases |
| Performance budget | Lighthouse CI | Regressions in Core Web Vitals |
| Type coverage | `typescript-coverage-report` | `any` creeping into the codebase |

## Setting Thresholds

Gates must be realistic or they will be ignored:

- **Test coverage**: start with 50% on new code, increase over time. Do not set a global coverage requirement — it incentivizes worthless tests
- **Lint rules**: start strict and relax only with team consensus. Never disable a rule with a comment without documenting why
- **Bundle size**: warn at +5%, block at +10%. Allow overrides with written justification
- **Type coverage**: target 90%+ on new code. The remaining 10% is legitimate (dynamic data, third-party types)

## Incident Response Gates

For production incidents, the gate pipeline compresses:

1. Fix the issue (hotfix branch bypasses some gates)
2. Add a regression test (within 24 hours)
3. Postmortem (within 72 hours)

The hotfix bypass is temporary — the regression test closes the gate for the same class of error.

## What Nobody Mentions

Quality gates are a trust mechanism. If a gate fails but you merge anyway, the gate loses credibility. If a gate never fails, it is noise. Review your gate effectiveness quarterly — remove gates that do not catch real issues, and tighten gates that are too permissive.

## Scripts

Scripts in `scripts/` analyze type coverage, track coverage trends, and generate gate reports. Use them when setting up or auditing quality gates.

## References

See `references/gate-config.md` for recommended ESLint, TypeScript, and Vitest configurations.
