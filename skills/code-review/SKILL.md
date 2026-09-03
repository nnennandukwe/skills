---
name: code-review
description: "Review a complete local change set against a fixed point on two separate axes: repository Standards and the originating Spec. Use before opening a pull request or when the user asks to review a branch, diff, or work in progress. Report only evidence-backed, actionable findings and keep the two axes distinct."
---

# Code Review

Review the complete local change set against one pinned fixed point. Keep two
questions separate:

- **Standards**: does the change follow the repository's documented rules and
  established implementation patterns?
- **Spec**: does the change completely and correctly implement the controlling
  ticket, brief, or specification without unrequested behavior?

This skill is self-contained. Do not require an external review service, model,
account, or repository-specific setup command.

## Review Boundary

- Review is read-only. Do not modify code, resolve findings, commit, push, or
  open a pull request unless the user separately authorizes those actions.
- Review the complete change set, including committed, staged, unstaged, and
  relevant untracked files.
- Treat test results and other reviewer output as evidence, not authority.
- Report a finding only when the current change introduces or exposes a
  concrete, actionable defect. Do not report preferences, speculative risks, or
  issues unrelated to the change.

## Process

### 1. Pin the fixed point

Use the commit, branch, tag, or merge-base the user supplied. If none was
supplied, infer the repository's normal base from its upstream or default remote
branch only when that choice is unambiguous; otherwise ask for the fixed point.

Confirm the ref resolves, record its commit SHA, and compute its merge-base with
`HEAD`. Review tracked changes from that merge-base through the working tree,
not only the committed range. List untracked files separately and inspect those
that belong to the change. Stop if the ref is invalid or the change set is
empty.

### 2. Resolve the controlling spec

Find the current source of intended behavior in this order:

1. a ticket, brief, PRD, or path supplied by the user;
2. an issue reference in the branch name or commit messages;
3. a matching file under the repository's documentation or specification
   directories;
4. the active user request, when it is sufficiently explicit.

Use live issue content when the repository's available tooling can read it.
Never invent inaccessible requirements. If no spec can be found, state that the
Spec axis is `NOT ASSESSED` and explain what source is missing; do not present it
as passed.

### 3. Resolve repository standards

Read all applicable instruction and contribution files for the changed paths,
including nested variants. Typical sources include `AGENTS.md`, `CLAUDE.md`,
`CONTRIBUTING.md`, coding standards, formatter and linter configuration, and
nearby tests that establish local patterns.

Repository rules outrank generic preferences. Do not duplicate findings that a
required formatter, compiler, linter, or test already reports unless the
failure reveals a distinct behavioral problem.

### 4. Run two independent passes

Run the Standards and Spec passes independently. Use isolated subagents in
parallel when the current agent supports them; otherwise run the passes
sequentially without allowing conclusions from the first axis to substitute for
analysis on the second.

For the **Standards pass**:

- inspect each changed file against the applicable repository instructions;
- check ownership, boundaries, error handling, names, duplication, and
  consistency with directly owning code only where the diff gives concrete
  evidence of a defect; and
- cite the controlling rule or established repository pattern for each
  finding.

For the **Spec pass**:

- map every accepted requirement to implementation and proof;
- identify missing or partial requirements, incorrect behavior, compatibility
  breaks, and scope creep; and
- quote or precisely paraphrase the controlling requirement for each finding.

Inspect tests as proof of intent, but verify the production behavior rather
than assuming a passing test is sufficient. Run only focused, non-mutating
checks needed to reproduce a suspected finding unless the user asked for the
full suite.

### 5. Validate and report findings

Before reporting a finding:

- reproduce it against the current working tree where practical;
- confirm it is caused by or materially affected by the reviewed change;
- identify the smallest concrete failure scenario;
- cite the exact file and tight line range; and
- state the requirement, rule, or observable behavior that is violated.

Use these priorities:

- **P0**: release-blocking data loss, security compromise, or broadly
  catastrophic behavior.
- **P1**: a required behavior is broken or a common path cannot safely ship.
- **P2**: a real defect with bounded impact or a meaningful maintainability
  failure in the changed design.
- **P3**: minor but actionable correctness or developer-experience defect.

Do not inflate severity. If evidence is insufficient, record the question under
`Verification Gaps` rather than turning it into a finding.

## Output Contract

Return:

```markdown
## Review Scope

- Fixed point: `<ref>` (`<sha>`)
- Change set: <tracked and untracked scope>
- Spec source: <source or NOT ASSESSED>
- Standards sources: <files>

## Standards

<findings ordered P0 to P3, or CLEAN>

## Spec

<findings ordered P0 to P3, CLEAN, or NOT ASSESSED>

## Verification Gaps

<unverified claims and the command or source that would settle each one, or None>

## Summary

<finding count per axis and the highest priority in each>
```

Each finding must include priority, title, file and line, evidence, impact, and
the smallest useful remediation direction. Do not merge or rerank Standards and
Spec findings across axes. If no actionable finding remains after validation,
say `CLEAN` explicitly for that axis.

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
