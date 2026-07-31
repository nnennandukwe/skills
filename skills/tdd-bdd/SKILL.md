---
name: tdd-bdd
description: "Drive implementation and bug fixes from behavior and failing tests. Use when working in test-driven development or behavior-driven development style: translate requirements or bugs into Given/When/Then scenarios, choose the smallest useful test layer, write a failing test before production code, implement the minimum change to pass, and refactor with coverage preserved."
---

# TDD BDD

Drive changes from observable behavior, not from speculative implementation.

Inspect the repo's existing test stack and naming conventions first, then apply `RED -> GREEN -> REFACTOR` with behavior phrased in user-visible terms.

## Workflow

1. Identify the behavior to prove.
2. Rewrite it as one or more small scenarios with explicit outcomes.
3. Choose the lowest test layer that can fail for the right reason.
4. Write the smallest failing test or scenario first.
5. Run the targeted test to confirm it fails for the expected reason.
6. Write the minimum production code to make that test pass.
7. Re-run the targeted tests, then broader affected tests.
8. Refactor only while tests stay green.

## Behavior Framing

Describe behavior in terms of inputs, events, and observable outcomes.

Use this template:

```text
Given <starting state>
When <trigger or action>
Then <observable result>
```

Prefer one rule per scenario. Split multi-assertion stories into separate scenarios unless the assertions represent one observable outcome.

Read [scenario-shaping.md](./references/scenario-shaping.md) when requirements are vague, the bug report is noisy, or you need help slicing scenarios into small testable behaviors.

## Test Selection

Prefer the lowest layer that proves the behavior without coupling the test to irrelevant internals.

Default order:

- Unit test for pure decision logic and transformations.
- Integration test for database, filesystem, queue, or service collaboration.
- Contract test for request/response boundaries.
- End-to-end test only when the behavior matters across multiple layers and lower layers would miss the regression.

Read [test-layer-selection.md](./references/test-layer-selection.md) when choosing between unit, integration, contract, and end-to-end coverage.

## TDD Guardrails

1. Do not write production code until a targeted test demonstrates the gap.
2. Keep the first failing test as small as possible.
3. Make the failure meaningful. A syntax error, missing fixture, or harness crash is not a useful red state.
4. Avoid over-mocking. Prefer realistic boundaries over implementation-shaped doubles.
5. Do not introduce a BDD framework just to satisfy wording. If the repo does not already use Gherkin or feature files, express behavior in test names, tables, or comments instead.
6. Refactor only after the behavior is protected.
7. Add a regression for every confirmed bug before fixing it.

## BDD Guardrails

1. Use shared domain language from the product or bug report.
2. Prefer examples and rules over abstract acceptance-criteria prose.
3. Keep scenarios focused on outcomes, not click-by-click UI narration, unless the UI sequence is the behavior.
4. Cover at least one success path and one meaningful unhappy path when the feature has gating or validation logic.
5. When behavior depends on dates, permissions, retries, or external failures, encode those conditions explicitly in the scenario setup.

## Implementation Pattern

1. Start with the smallest scenario that would have caught the bug or proves the new rule.
2. Add only the production code required to satisfy that scenario.
3. If a second scenario exposes a new branch, add that as a new red test instead of expanding the first test until it becomes opaque.
4. Consolidate duplicated setup after green, not before.
5. Keep assertions at the level of behavior. Assert returned values, persisted records, emitted events, or rendered copy before asserting private helper usage.

## Validation Pattern

1. Run the new or changed targeted tests after each red/green cycle.
2. Run the smallest broader suite that exercises nearby behavior before finishing.
3. Verify the original bug or acceptance criteria against observable output, not only test internals.
4. If a flaky or slow end-to-end test is the only proof, add a lower-layer regression too when feasible.

## Output Format

When reporting work, include:

- Behaviors or scenarios added
- Test layer chosen and why
- Failing test written first
- Production change made to reach green
- Refactor performed or explicitly deferred
- Validation run
- Residual gaps or follow-up scenarios

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
