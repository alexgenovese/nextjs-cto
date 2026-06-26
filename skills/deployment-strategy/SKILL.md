---
name: deployment-strategy
description: Design and manage release pipelines — from commit to production. Use when setting up CI/CD, planning a release, rolling back, or choosing between deployment strategies (blue-green, canary, feature flags, progressive delivery).
---

# Deployment Strategy

Ship reliably at any scale.

## The Ideal Pipeline

```
commit → lint+typecheck → unit tests → build → integration tests → staging → E2E → production
```

Every stage must be fast enough that developers do not bypass it. If CI takes 30 minutes, developers will stop waiting for it.

## Deployment Strategies

| Strategy | Risk | Speed | Complexity | Best for |
|----------|------|-------|------------|----------|
| **Feature flags** | Lowest | Instant on/off | Medium | Anything |
| **Canary** | Low | Gradual | High | Traffic-sensitive changes |
| **Blue-green** | Low | Immediate switch | Medium | Infra changes |
| **Rolling update** | Medium | Per-instance | Low | Stateless services |
| **Recreate** | Highest | Full swap | None | Dev environments |

**Prefer feature flags** for application-level changes. They decouple deployment from release — you can deploy unfinished code safely and release features when ready.

## Rollback Strategy

Every deployment must have a rollback plan tested before the deploy:

1. Can we revert the code? (git revert is not enough — DB migrations may not be reversible)
2. Can we toggle the feature off? (feature flags are the fastest rollback)
3. How long does rollback take? (if > 5 minutes, you need a faster mechanism)

## Progressive Delivery

Ship to a subset of users first:

1. **Internal** — deploy to dev/staging, validate with team
2. **Beta** — deploy to 5% of users, monitor errors and performance
3. **Gradual rollout** — 25% → 50% → 75% → 100%, with monitoring gates at each step
4. **Full release** — 100% with kill switch ready

## Database Migrations

The riskiest part of any deploy. Rules:

- Migrations must be backward-compatible (old code works with new schema)
- Split into phases: schema change → deploy code → backfill data → remove old columns
- Never do a migration that locks a production table for more than a few seconds
- Use a tool (Sqitch, Prisma Migrate, Flyway) — raw SQL scripts in a README do not count

## References
See `references/deployment-playbook.md` for runbooks for common deploy scenarios.

See `references/rollback-checklist.md` for the pre-deploy rollback verification checklist.
