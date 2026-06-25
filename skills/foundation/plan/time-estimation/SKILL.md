---
name: time-estimation
description: Produce realistic effort estimates for engineering work. Use when asked to estimate a task, feature, or project — from rough order-of-magnitude to sprint-level granularity.
---

# Time Estimation

Produce estimates that survive contact with reality.

## The Problem with Estimates

Humans are systematically optimistic. We estimate as if everything goes right — but in practice, integration, debugging, review, and context-switching consume 40-60% of the total time. This skill exists because your first instinct ("that'll take 2 days") is wrong.

## Anchoring

For every task, generate three numbers:

| Bound | Meaning | Probability |
|-------|---------|-------------|
| **Best case** | Everything goes right, no interruptions, dependencies ready | ~10% |
| **Nominal** | Reasonable assumptions, some debugging, one review cycle | ~50% |
| **Worst case** | Things go wrong, dependencies slip, API breaks, PRs take 3 rounds | ~90% |

Never give a single number. Always communicate the range. If asked for a single number, use the nominal case but explicitly say "with a confidence range of X to Y."

## Estimation by Analogy

The most reliable technique. Find something similar you've already done and use its actual duration as a baseline, adjusted for differences:

- New tech stack? Multiply by 1.3–1.8
- Same team has done it before? Multiply by 0.7–0.9
- Has compliance/review requirements? Add 30-50%
- Involves 3rd-party integration? Add at least buffer for their response time

## Decomposition Method

Break the work into tasks of < 1 day each (see `task-breakdown` skill). Then:

1. Estimate each sub-task independently
2. Sum the best-case estimates
3. Multiply by 2x — this is your nominal estimate
4. If the sum exceeds 5 days, break it down further

The 2x multiplier accounts for: meetings, code review, testing, documentation, deployment, rollback planning, and the inevitable "oh, I also need to do X."

## The Cone of Uncertainty

Estimates are least accurate at the start and converge as implementation progresses:

| Phase | Uncertainty |
|-------|-------------|
| Initial concept | 4x (actual could be 0.25x to 4x the estimate) |
| Requirements written | 2x |
| Design complete | 1.5x |
| Tasks broken down | 1.25x |
| Implementation started | 1.1x |

Recalibrate at each phase. An estimate from the concept phase is not a commitment.

## Communicating Estimates

- Always include assumptions and exclusions ("Assumes API keys are provisioned", "Does not include E2E tests")
- State the confidence level, not just the number
- Re-estimate when assumptions change — the mid-sprint "update" is not a failure, it's a signal

## References

See `references/calibration-log.md` for tracking how your past estimates compare to actuals — the only way to get better at this.
