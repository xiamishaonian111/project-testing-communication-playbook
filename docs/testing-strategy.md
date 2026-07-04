# Testing Strategy

## Goal

The goal is to move from "the workflow can run" to "we know when it works, how well it works, and where it fails."

For a high-priority reliability project, an early MVP may be useful for proving feasibility. The next planning cycle should focus on confidence building:

- replace rough implementation paths with the intended long-term design,
- define correctness checks,
- add automated coverage where the behavior is stable,
- run the workflow in a representative long-lived environment,
- measure timing under realistic data load,
- and capture the automation path for future work.

## Quality Dimensions

Do not reduce project quality to one timing number. Track at least these dimensions:

- Completion time: how long the end-to-end workflow takes under a representative load.
- Freshness: how recent the resulting state can be.
- Correctness: whether the resulting state is complete and internally consistent.
- Repeatability: whether the workflow can be run again with predictable setup and teardown.
- Observability: whether failures are visible, explainable, and diagnosable.

## Test Environment Ladder

Use a ladder instead of forcing every test into the most expensive environment.

### Fast Local Tests

Use for fast feedback and narrow correctness checks.

Best fit:

- core logic,
- edge cases,
- metadata transformations,
- deterministic validation,
- small scenario coverage,
- regression checks that should run frequently.

Q3-style target:

- make this layer automated,
- add missing critical cases,
- keep it cheap enough to run often.

### Short-Lived Integration Tests

Use for end-to-end workflow validation in an environment that is cheaper to create and reset.

Best fit:

- full workflow smoke tests,
- cross-component integration,
- setup and sequencing validation,
- partial automation of stable operational steps.

Q3-style target:

- keep the full workflow runnable,
- automate the most stable and repeatable steps,
- keep manual steps explicit until the workflow is better understood.

### Long-Lived Representative Tests

Use for a higher-confidence validation with realistic setup, realistic data shape, and real operational constraints.

Best fit:

- representative end-to-end validation,
- timing measurement,
- data-load sensitivity,
- correctness checks under scale,
- finding setup gaps that cheaper environments hide.

Q3-style target:

- run at least one manual end-to-end validation,
- use production-shaped synthetic data load,
- capture actual timing and correctness results,
- identify the smallest useful automation slice for future work.

## Manual First Does Not Mean Manual Forever

Manual validation is useful when the workflow is not fully understood yet. It can reveal setup gaps, data gaps, instrumentation gaps, and correctness gaps before the team spends time automating the wrong assumptions.

The right sequencing is usually:

1. Run a minimum manual representative validation.
2. Use it to discover the real steps, gaps, and checks.
3. Automate fast local tests and stable integration steps.
4. Design representative-environment automation based on what was learned.
5. Automate the representative path once the workflow is stable enough.

## Production-Shaped Data Load

For representative-environment measurement, synthetic data must approximate production shape. The goal is not to copy production content. The goal is to match the workload dimensions that affect timing and correctness.

### Required Dimensions

- Total data volume.
- Number of objects.
- Number of directories or containers.
- Namespace depth.
- Directory or container fanout.
- Small-object versus large-object distribution.
- Metadata volume and metadata complexity.
- Metadata-to-data ratio.
- Hot and cold distribution.
- Recent mutation pattern.
- Burstiness and concurrency.
- Delete, recreate, rename, and update patterns.

### Correctness Oracle

The data generator should create an expected-state manifest before the simulated failure point or validation checkpoint.

The manifest should support comparison of:

- object existence,
- namespace structure,
- key metadata fields,
- permissions or ownership fields if relevant,
- timestamps if relevant,
- content checksum if data content matters,
- known exceptions and intentionally unsupported cases.

Without a correctness oracle, the team can only say the workflow ran. It cannot confidently say the output is correct.

### Dataset Tiers

Use tiers so the team can make progress without pretending that every run is production-scale.

#### Tier 0: Flow Validation Dataset

- Small scale.
- Useful for validating the workflow can execute end to end.
- Not suitable for meaningful timing claims.

#### Tier 1: Representative Dataset

- Main near-term target.
- Matches production on the most important distributions.
- Suitable for first credible timing and correctness signal.

#### Tier 2: Stress Dataset

- Larger scale or intentionally difficult shape.
- Useful for finding bottlenecks and upper-bound behavior.
- Usually a later milestone unless the team has enough capacity.

## Planning Template

Use this checklist when writing the test strategy.

- What is the minimum representative validation we need this quarter?
- What must be automated now?
- What should remain manual until the workflow is better understood?
- Which test cases belong in fast local tests?
- Which test cases belong in short-lived integration tests?
- Which test cases require a long-lived representative environment?
- What data-load shape is required for a meaningful measurement?
- What expected-state manifest will prove correctness?
- What known gaps remain after the quarter?
- What is the post-quarter automation plan?

## Decision Record Template

Use a decision record so tradeoffs are explicit.

```text
Decision:
Use a manual representative-environment run this quarter, while automating fast local coverage and stable integration steps.

Reason:
The representative workflow has not been validated enough to justify automating the full path first. A manual run will reveal setup, data-load, correctness, and instrumentation gaps.

Risk:
Manual validation is slower and may be less repeatable.

Mitigation:
Keep the manual run scoped, record every step, generate an expected-state manifest, automate stable lower-level tests, and produce a follow-up automation plan.

Review date:
[date]
```
