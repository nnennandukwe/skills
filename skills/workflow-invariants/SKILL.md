---
name: workflow-invariants
description: Design, enforce, and test workflow state-machine invariants for multi-stage pipelines. Use when workflow bugs involve stale stage selection, invalid transitions, missing guards, failed-run reuse, or downstream actions starting before upstream approvals complete.
---

# Workflow Invariants

Model workflow behavior as explicit states and allowed transitions, then enforce it in code and tests.

## Do This First

1. Identify the canonical state object and transition points.
2. Enumerate valid transitions and reject everything else.
3. Identify cross-workflow gates that must block downstream commands.

Use this template:

```text
state: awaiting_review
allowed: approve -> awaiting_resume
blocked: social, prep, publish
error: "Draft is awaiting final_output approval."
recovery: "Run draft approve + draft resume."
```

## Implementation Pattern

1. Put transition logic in one place.
2. Return structured failure payloads for invalid transitions.
3. Carry recovery instructions in all blocked-path errors.
4. Remove stale artifacts when moving into blocked states.
5. Emit an event on every transition for traceability.

## Test Pattern

1. Add one positive-path test per transition.
2. Add one negative-path test per blocked transition.
3. Add regression tests for known stale-selection bugs.
4. Assert both behavior and operator-facing message text.

## Invariant Checklist

- A failed run never becomes the active run.
- A run from another week never drives current guidance.
- Downstream flows are blocked for `quality_blocked` and `awaiting_final_output_review`.
- Final artifacts do not exist before approval+resume completes.
- Help text and guidance reference the same transition names.

## Output Format

When reporting work, include:

- Transition table added/updated
- Guards added
- Tests added
- Residual risk

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
