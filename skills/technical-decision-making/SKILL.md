---
name: technical-decision-making
description: Evaluate technology choices, architectural approaches, and design trade-offs systematically. Use when choosing between frameworks, libraries, architectures, or implementation strategies — from build vs buy to database selection to API design patterns.
---

# Technical Decision Making

Systematic framework for technical decisions that stand up to scrutiny.

## The Decision Template

Every significant technical decision should follow this structure:

| Field | Description |
|-------|-------------|
| **Context** | What forces are at play? (team size, deadline, existing infra, compliance) |
| **Options** | At least 2 real alternatives with honest pros/cons |
| **Decision** | What we chose and why |
| **Consequences** | What we gain, what we give up, what we must monitor |
| **Status** | Proposed → Accepted → Deprecated → Superseded |

## Common CTO-Level Decisions

### Build vs Buy vs Open Source

| Factor | Build | Buy | OSS |
|--------|-------|-----|-----|
| Control | Full | Vendor-dependent | Fork-able |
| Speed to market | Slowest | Fastest | Medium |
| Maintenance burden | High | Low (contract) | Medium |
| Differentiation | Yes | No | Possible |
| Total cost | Often highest | Predictable | Ops cost only |

**Rule of thumb**: buy commodity, build differentiator. If it is not core to your product's value, do not build it.

### Framework & Library Choices

When evaluating a new dependency:

1. Is it actively maintained? (check commit frequency, issue resolution, release cadence)
2. Does it have a healthy ecosystem? (documentation, community, tools)
3. Is it replaceable? (can you swap it without rewriting everything?)
4. What is the migration cost if it goes wrong?

### When to Say No

A CTO's primary job is saying no to good ideas that are not the right ideas right now. Common traps:

- "We should rewrite in [new hotness]" — rewrites discard embedded knowledge of edge cases
- "Let's add this for future-proofing" — future-proofing is speculation; solve today's problem
- "Everyone else is using it" — popularity is not a substitute for requirements analysis

## Writing ADRs

Architecture Decision Records live in `docs/adr/` and are numbered sequentially. Each ADR is one decision. Write them when the decision is made, not before.

```markdown
# ADR-001: Use Postgres as primary datastore

## Context
We need a datastore for user profiles, billing, and content. Team is experienced with SQL.

## Options Considered
- **Postgres** — familiar, ACID, good ecosystem, managed options (RDS, Neon, Supabase)
- **MongoDB** — flexible schema, but no team experience, eventual consistency concerns for billing
- **DynamoDB** — infinite scale, but complex query modeling, no joins, vendor lock-in

## Decision
Postgres, via Supabase (managed, per-project DBs, real-time subscriptions, row-level security).

## Consequences
+ Strong consistency guarantees for billing
+ Team can be productive immediately
+ Supabase RLS maps well to our multi-tenant model
- Need to plan for read replicas at scale
- Schema migrations require discipline (we use Sqitch)
```

## References

See `references/adr-examples.md` for more ADR patterns and anti-patterns.
