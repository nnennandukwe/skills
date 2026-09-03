# <Ticket or Build Name> Build Plan

## Summary

- Outcome:
- Authority or ticket:
- Starting baseline:
- Review unit:
- Primary boundary:

## Skills To Use

| Execution stage | Skill | Responsibility and evidence |
|---|---|---|
| Before implementation | `<available-skill>` | <rules, repository context, or design input this skill supplies> |
| Architecture and implementation | `<available-skill>` | <decision or implementation work this skill governs, and where it appears below> |
| Behavior and hardening | `<available-skill>` | <test, failure mode, or risk this skill makes observable> |
| Interface and focused audits | `<available-skill>` | <changed surface to audit and the blocking finding threshold> |
| Final review | `code-review` | Review the complete change set against the fixed point across separate Standards and Spec axes. |

Add rows as needed so every applicable execution stage is represented by at
least one selected skill.

Coverage notes:

- <obvious overlapping skill deliberately omitted, unavailable stack-specific skill and alternate coverage, or "None">

## Scope

In scope:

- <behavior or deliverable>

Out of scope:

- <deferred behavior and its owner, when known>

## Package Layout / File-to-Task Mapping

- `<existing-or-new-path>`: <single responsibility in this build>

## Dependencies And Settings

- Runtime dependencies:
- Development dependencies:
- Schema or storage version:
- Environment and configuration:
- Toolchain constraints:

## Canonical Contracts

Define the exact public or machine-facing behavior introduced or changed by
this build.

```text
<command, API, schema, state transition, or artifact example>
```

Compatibility:

- <preserved, deprecated, or intentionally changed behavior>

## TDD And BDD Implementation Strategy

Test seams:

1. <public seam>

Vertical slices:

1. <failing behavior> -> <smallest implementation that makes it pass>
2. <next failing behavior> -> <smallest implementation that makes it pass>

## Component Design

- Ownership:
- Data flow:
- State or transaction boundary:
- Integration with current architecture:
- Abstractions deliberately not introduced:

## Failure And Recovery Rules

- Given <failure>, no <forbidden mutation or action> occurs.
- Return or persist <structured failure/blocked state>.
- Retain <diagnostic evidence>.
- Recovery: <specific caller or operator action>.

## Commit Plan

1. `<type(scope): behavior-closed change>`
2. `<type(scope): behavior-closed change>`

## Branch And PR Flow

1. Synchronize the repository-defined base branch.
2. Create `<proposed-branch>` from the verified baseline.
3. Implement the vertical slices in order.
4. Run focused checks after each slice and the full required suite before
   review.
5. Pin the review fixed point and run the current agent's `code-review` skill
   against the complete change set, preserving separate Standards and Spec
   findings.
6. Reproduce every finding at the current head, retain Standards/Spec axis
   provenance, deduplicate overlapping root causes, and resolve every confirmed
   actionable finding. Record evidence for false positives; require explicit
   user approval for any deliberate exception.
7. Run affected tests and the full repository suite, then rerun `code-review`
   against the complete change set. Any later code change makes the review
   evidence stale. Repeat until no confirmed actionable finding remains.
8. Open one pull request with the repository-required ticket link or closing
   reference.

## Test Plan

Focused behavior:

- <happy path>
- <blocked or negative path>
- <historical regression>
- <integration, persistence, or concurrency path>

Verification commands:

```bash
<repository-native command>
```

Manual acceptance:

- <manual proof that remains necessary, or "None">
- `code-review` findings were reproduced and merged into one remediation ledger
  without losing Standards/Spec provenance.

## Definition Of Done

- <observable capability or invariant>
- <observable compatibility result>
- <required automated checks pass>
- Mandatory `code-review` passes against the final complete change set with no
  confirmed actionable finding unresolved.
- <explicit non-goal has not leaked into the build>

## Assumptions And Defaults

- <decision made to keep execution deterministic>
- <default chosen when the ticket is silent>
