---
name: failure-path-testing
description: Build failure-first integration tests for workflow gates and recovery paths. Use when regressions appear after happy-path changes, when reviews flag missing negative tests, or when downstream workflows should remain blocked until specific approvals or quality checks pass.
---

# Failure Path Testing

Prioritize tests that prove the system refuses unsafe progression.

## Test Design Workflow

1. Identify critical gates where progression must stop.
2. Define blocked state fixtures and expected recovery actions.
3. Write integration tests that execute realistic command flows.
4. Assert both state transitions and user-facing recovery text.
5. Add one test per historical regression.
6. When a bug spans parsing, persistence, and operator output, cover all three layers in one regression path.

## High-Value Negative Paths

1. Quality gate fails and blocks social/prep.
2. Approval pending and downstream command is rejected.
3. Invalid date/week input does not create runs.
4. Failed strategy/draft run is not selected as active guidance.
5. Malformed input returns file/line or other actionable diagnostics instead of a generic crash.
6. Startup/readiness construction failures fail closed before state mutation or service exposure.
7. Artifact write failures do not leave partial output behind or replace the good index.

## Assertions To Include

- No forbidden artifact written (for example, no final draft).
- Blocking state persisted with diagnostic metadata.
- Recovery command hints are explicit and executable.
- Downstream commands fail deterministically.
- Error text should tell the operator what to fix next, not just what went wrong.

## Coverage Heuristic

For each new workflow feature:

1. Add at least one happy-path test.
2. Add at least one blocked-path test.
3. Add one message-clarity assertion for operator recovery.
4. If the feature writes files or swaps state, add a regression for the failure point before the swap.

## Output Format

When reporting work, include:

- Negative paths added
- Regression cases covered
- Blocking guarantees verified
- Remaining untested gate conditions

## High-Signal Patterns

These are the failure paths most often left untested. Reach for this skill when the change touches any of them.

- Malformed or missing configuration, readiness probes, startup guards, and artifact staging.
- Anywhere a pipeline must *not* advance: after a failed ingest, a failed readiness check, or an unmet gate. The valuable test asserts the non-advance, not the happy path.
- An empty or absent setting value that could silently resolve to a wrong default rather than failing loudly.
- Startup or background-process failures that currently surface as a silent exit rather than a non-zero status.
- Anything a review has already caught once. When a review asks for a fix, write the test around the exact failure mode before the fix — one regression test per exposed mode: an unhandled exception type, a missing CLI option, an unsafe generated filename, a test-environment leak into a live path.

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
