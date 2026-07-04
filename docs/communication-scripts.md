# Communication Scripts

These scripts are meant for a technical project lead working with a test owner who is concerned about project speed, test coverage, and automation sequencing.

The tone should be direct: acknowledge valid risk, state constraints, give real ownership, and ask for a concrete plan.

## 1:1 Opening

```text
I wanted to sync specifically on the testing strategy. I think some of your concerns are valid, especially because this is a high-priority reliability project. The earlier milestone was more about proving feasibility. For this planning cycle, we need to build confidence that the workflow is correct, measurable, and repeatable.

At the same time, we have real timeline and priority constraints. I do not think we can make a fully automated representative-environment framework a blocker for this quarter. But we do need a concrete testing strategy that makes the risk explicit and gives us a path from manual validation to automation.
```

## Give Ownership

```text
I would like you to own the test strategy for this project.

Concretely, I want you to propose a plan that covers:
1. what should be covered by fast local automated tests,
2. what should be covered by short-lived integration tests,
3. what minimum representative-environment manual test we need this quarter,
4. what correctness checks we need for the resulting state,
5. what automation work is realistic this quarter,
6. and what representative-environment automation should look like after this quarter.
```

## Set Boundaries

```text
The constraint is that this quarter still needs visible progress on the long-term solution and at least one representative-environment validation. So the plan should not assume that we can block the quarter on building a full create-run-teardown representative test framework.

If you think something is a must-have for this quarter, please separate it from nice-to-have automation and explain what risk we take if we defer it.
```

## Ask For a Concrete Output

```text
Can you put together a short doc by [date]?

I think the doc should include:
- the test environment ladder,
- test cases mapped to the right environment,
- must-have test coverage for this quarter,
- known gaps and risks,
- owners and rough timeline,
- realistic automation work for this quarter,
- and the post-quarter automation plan.
```

## Convert Concern Into a Plan

If the concern is "the project is moving too fast" or "testing is not enough":

```text
I hear the concern. To make this actionable, can you identify the top three risks that you think are not adequately covered by the current plan?

For each one, I would like us to write down:
- what could go wrong,
- how severe it is,
- what test would catch it,
- which environment it belongs in,
- and whether it needs to block this quarter or can be tracked as a known gap.
```

## When Someone Says Manual Work Is Wasteful

```text
I agree that if the workflow is already well understood and stable, going directly to automation can reduce total engineering time.

But I do not think we are in that state yet. For the representative environment, we have not completed even one full manual end-to-end run. That means we still do not fully know the setup gaps, data synthesis gaps, correctness checks, instrumentation needs, and operational steps.

If we jump directly to full automation, we may spend the quarter automating assumptions that have not been validated. A manual representative run is not throwaway work here. It is the discovery pass that defines what the automated test actually needs to do.
```

## When Someone Wants to Defer Measurement

```text
I do not think we should defer all representative-environment measurement until next quarter.

The current timing estimate is mostly inferred. That was acceptable for the feasibility phase, but it is not enough for this planning cycle. Even one or a few measured representative runs would significantly improve our confidence and may reveal issues that affect the long-term implementation.

So I am open to reducing the manual scope, but I am not comfortable removing representative-environment validation entirely from this quarter.
```

## Ask For a Comparable Automation Proposal

```text
If you believe direct automation is faster overall, can you write down a concrete proposal?

I would like to see:
- the smallest automated representative test slice we can complete this quarter,
- what it would cover,
- what it would not cover,
- how long it would take,
- what work we would deprioritize,
- and whether it would still give us measured timing and correctness signal this quarter.

Then we can compare that against the manual representative run plan.
```

## Preferred Compromise

```text
My preferred compromise is:

1. Do one minimum manual representative end-to-end run this quarter.
2. Use that run to validate setup, data shape, workflow steps, timing instrumentation, and correctness checks.
3. In parallel, automate the cheaper and more stable layers: fast local tests and parts of short-lived integration testing.
4. Write the representative automation design based on what we learn from the manual run.
5. If we find a small representative automation slice that is realistic this quarter, we can include it, but it should not replace the first manual representative validation.
```

## Clear Bottom Line

```text
I am flexible on how much we automate this quarter. I am not flexible on ending the quarter with no representative-environment validation and no measured timing signal.
```

## Data-Load Framing

```text
I want to be precise about synthetic data. For representative testing, we are not just generating arbitrary test files. We need a production-shaped data load, because timing depends heavily on data volume, metadata volume, namespace shape, and change pattern.

For this quarter, I think the goal should be at least one representative run where the dataset approximates production along the workflow-relevant dimensions. It may not be full production scale, but it needs to be realistic enough that the timing and correctness signal is meaningful.
```

## Data-Load Planning Ask

```text
Can we define the data-load target explicitly?

I think we need:
- target total data volume,
- target number of objects and containers,
- size distribution,
- namespace depth and fanout distribution,
- metadata features we need to cover,
- recent mutation workload before the validation checkpoint,
- expected-state manifest generation,
- and how we compare the resulting state against the manifest.

Without this, a representative run may prove that the workflow executes, but it will not give us a credible timing or correctness signal.
```

## Follow-Up Message After the 1:1

```text
Thanks for the discussion. To make the testing strategy concrete, could you own a short strategy doc covering:

- the test ladder across fast local, short-lived integration, and long-lived representative environments,
- mapping of existing and new test cases to the right environment,
- must-have coverage for this quarter,
- correctness checks for the resulting state,
- the minimum representative-environment manual validation we need this quarter,
- the production-shaped data load required for meaningful measurement,
- automation work that is realistic this quarter,
- and the post-quarter representative automation plan.

The main constraint is that full create-run-teardown automation for the representative environment should not be assumed as this quarter's blocker unless we explicitly decide what to deprioritize. The goal is to make quality risks explicit and create a practical path from manual validation to automation.
```
