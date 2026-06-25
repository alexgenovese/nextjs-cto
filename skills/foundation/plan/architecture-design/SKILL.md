---
name: architecture-design
description: Design and document software architecture — system boundaries, module decomposition, data flow, API contracts, and trade-off analysis. Use when starting a new project, adding a major feature, or reviewing an existing architecture for coupling and cohesion problems.
---

# Architecture Design

Systematic approach to software architecture decisions.

## Before You Design

Three questions to answer first:

1. **What problem are we solving?** Not the feature, the underlying need (latency, team throughput, compliance)
2. **What are the constraints?** Budget, timeline, team size, existing infra, compliance (SOC 2, GDPR, HIPAA)
3. **What's the decision horizon?** Architecture for 6 months looks different from architecture for 5 years

## Decision Framework

For each architectural decision, document:

- **Context**: why this decision matters now
- **Options considered**: at least 2 real alternatives, not straw men
- **Decision**: what we chose
- **Consequences**: what we gain and what we give up (no free lunches)
- **Status**: proposed, accepted, deprecated, superseded

Format as ADRs (Architecture Decision Records) stored alongside the code. One file per decision, `docs/adr/NNN-title.md`.

## Common Concerns

### Service & Module Boundaries

- High cohesion inside, loose coupling across
- Shared schemas in a library, not copy-pasted
- A module should be replaceable without rewriting its consumers (interface before implementation)
- If two modules share a database table, they're one module

### Data Flow

- Who owns the source of truth? One service writes, others read (via API or event)
- Event-driven: define the event schema first, before producer or consumer
- No hidden channels: if module A reads module B's database, document the dependency explicitly

### API Contracts

- Internal APIs: tolerate breaking changes with version negotiation or grace periods
- External APIs: version from day one, deprecate before removing
- Error responses include a stable machine-readable code, not just an HTTP status

## Review Checklist

- [ ] Every component has one reason to change (Single Responsibility)
- [ ] Dependencies point inward (core has no dependency on framework)
- [ ] Can deploy components independently?
- [ ] Can test components in isolation?
- [ ] Is the data flow documented and traceable?
- [ ] Are failure modes documented? (What happens when service X is down?)
- [ ] Are rate limits, backpressure, and retry policies defined?

## What Nobody Mentions

Architecture is not about getting it right the first time — it's about making change cheap. A good architecture is one where you can change your mind without rewriting everything. If your design can't tolerate a wrong prediction, it's over-engineered.

## References

See `references/adr-example.md` for an ADR template.
See `references/decision-log.md` for patterns on maintaining a living decision log.
