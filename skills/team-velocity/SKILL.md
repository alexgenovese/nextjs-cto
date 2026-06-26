---
name: team-velocity
description: Measure, diagnose, and improve engineering team throughput. Use when evaluating team performance, planning sprints, identifying bottlenecks, or improving delivery cadence.
---

# Team Velocity

Understand and improve how your team ships.

## Measuring Velocity

Do not use velocity for comparison across teams — use it for a single team's trend over time.

| Metric | What it tells you | Caveat |
|--------|-------------------|--------|
| Cycle time | How long from start to ship | Excludes blocked time |
| Deployment frequency | How often you ship | Batched deploys inflate it |
| Change fail rate | How often things break | Needs good monitoring |
| Mean time to recovery | How fast you fix | Only counts when paged |

## Identifying Bottlenecks

Most teams have ONE bottleneck at a time. Find it:

1. Map your workflow: ideation → spec → design → implementation → review → QA → deploy
2. Measure time spent in each stage (use your issue tracker or PR data)
3. The stage with the longest wait time is your bottleneck

**Common bottlenecks and fixes:**

| Bottleneck | Symptom | Fix |
|------------|---------|-----|
| Code review | PRs sit for days | Rotate reviewers, set SLA, smaller PRs |
| QA/testing | Bugs found late | Shift left: unit + integration before QA |
| Requirements | "We built the wrong thing" | Write acceptance criteria before coding |
| Deployment | Deploy takes hours | Automate pipeline, feature flags for safe shipping |

## Improving Without Burning Out

Velocity improvements that compound:

- **Smaller PRs** — a PR under 200 lines reviews faster, merges faster, breaks less
- **Trunk-based development** — short-lived branches, no merge hell
- **Automated quality gates** — CI fails fast so humans review less garbage
- **Capacity buffer** — reserve 20% of each sprint for tech debt, bugs, and unexpected work

## What Nobody Mentions

Velocity is a symptom, not a goal. If you optimize for velocity directly (story points, sprint commitments), you get inflated estimates and quality cuts. Optimize for cycle time and deployment friction instead — velocity follows naturally.

## References

See `references/bottleneck-analysis.md` for scripts to analyze your PR and issue tracker data.
