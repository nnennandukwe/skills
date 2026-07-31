---
name: software-build-plan
description: Create an evidence-backed, implementation-ready Software Build Plan for a coding ticket, feature, fix, migration, or other bounded software change. Use when the user wants to plan implementation before coding, asks for a build plan, or wants a structured pre-implementation planning document. Keep planning distinct from implementation unless the user explicitly asks for both.
---

# Software Build Plan

Produce a consistent build plan that another capable coding agent can execute
without rediscovering the ticket boundary, contracts, file seams, or proof
strategy.

A build is a coherent, reviewable unit of software work. It is not defined by
the number of hours or days it may take.

## Required Terminology

- Name this skill and planning method **Software Build Plan**.
- Title the artifact `# <Ticket or Build Name> Build Plan`.
- Use `## Skills To Use`, because the plan describes future execution.
- Never call the artifact a "Single-Day Build Plan" or use another time-boxed
  name unless the user explicitly requests one.
- Keep implementation status out of the plan. Planned work is not completed
  work.

## Operating Boundary

- Planning is read-only by default.
- Do not edit product code, create a branch, commit, push, open a pull request,
  or update a ticket unless the user also asks for implementation or a write.
- Read-only inspection, fetching current remote state, and running safe
  diagnostics are allowed when needed to ground the plan.
- Return the plan in the conversation by default.
- Save a plan only when the user asks. If it is saved in a repository, follow
  that repository's instructions and state whether the file is tracked,
  ignored, or local-only.

## Planning Workflow

### 1. Resolve the authority

Identify the exact repository, ticket, specification, pull request, or user
brief that defines the build.

- Read the applicable repo-local agent instruction files, contribution guidance,
  and local workflow instructions. Check every convention the repository
  actually uses, including `AGENTS.md`, `CLAUDE.md`, `.cursorrules`,
  `CONTRIBUTING.md`, and any nested per-directory equivalents.
- Re-open live tickets or specifications instead of relying on older summaries.
- Inspect the current base branch or target revision.
- Preserve the distinction between accepted requirements, assumptions, and
  later follow-up work.

### 2. Inspect before naming files

Find the real implementation seams before producing `Package Layout /
File-to-Task Mapping`.

- Identify current public interfaces, domain types, persistence paths, command
  registration, tests, docs, and build scripts relevant to the ticket.
- Prefer current source over historical plans when they disagree.
- Name exact files only when current repository evidence supports them.
- If a file will probably be new, mark it as new rather than implying it
  already exists.

### 3. Bound one coherent build

The plan should normally fit one reviewable pull request.

- Separate in-scope work from explicit non-goals.
- Identify which later tickets or builds own deferred behavior.
- Do not hide a multi-PR program behind a single build plan.
- If the requested work is too broad, recommend a build sequence and write the
  plan only for the first independently reviewable build unless the user asks
  for the whole sequence.
- Avoid implementation-day estimates unless the user requests estimates.

### 4. Select future skills deliberately

Populate `Skills To Use` only with skills that should materially shape
execution.

- State why each skill applies.
- Use future tense.
- Do not claim that a skill has already reviewed or verified unimplemented
  work.
- Prefer a small set of relevant skills over an exhaustive catalog.

### 5. Lock contracts and test seams

Before describing implementation steps:

- Define the public behavior, commands, APIs, schemas, state transitions, or
  artifact shapes that the build changes.
- Name compatibility promises and intentional breaking changes.
- Identify the public seams where behavior will be tested.
- Prefer vertical behavior slices: one failing behavior, the smallest passing
  implementation, then the next slice.
- Include negative paths, concurrency paths, rollback behavior, and operator
  recovery when relevant.

### 6. Require dual pre-PR review and remediation

Every build plan must include a review gate before opening a pull request.

- Pin one fixed point and review the complete change set against it, including
  committed, staged, unstaged, and untracked files.
The gate requires two reviews from **different model families**. A second pass
from the same model that wrote the code shares its blind spots and is not an
independent review.

**Reviewer A — the primary review.** Run the `code-review` skill available in
the current agent against the fixed point. Preserve its separate Standards and
Spec findings.

**Reviewer B — the independent review.** Invoke a different agent, in a
different model family, read-only, against the same fixed point, standards
sources, specification, and complete change set.

- Invoke Reviewer B non-interactively. Most agent CLIs expose a one-shot mode
  with a model selector, a reasoning-effort setting, and a read-only or
  plan-only permission mode. The shape is:

  ```bash
  <other-agent-cli> --print \
    --model <pinned-model-id> \
    --effort high \
    --permission-mode plan \
    "<focused read-only code-review prompt>"
  ```

  Substitute the CLI, flags, and model id your environment actually provides.
  What matters is that the reviewer is a different model family from the
  author, runs read-only, and is pinned rather than left to default.
- Name the exact model id, effort level, and permission mode in the plan. Do
  not silently substitute an alias, a "latest" tag, or a different model — an
  alias can resolve to the same family that wrote the code, which collapses the
  independence the gate exists to provide. If the pinned reviewer is
  unavailable, the review gate stays blocked until the user explicitly changes
  the requirement.
- Bound Reviewer B's prompt to one focused pass over the fixed diff, applicable
  standards, specification, and directly owning code. Tell it not to edit, use
  the network, spawn subagents, or run the full test suite. Require only
  confirmed P0-P3 findings with file, line, and evidence, or `CLEAN`.
- Escalate to a stronger model for security, concurrency, schema or migration
  changes, or a disputed P0/P1 finding. Pin the escalated model the same way.
  Escalation is additional review evidence, not the default gate.

- Compare the two reviews, retain reviewer and axis provenance, deduplicate
  overlapping root causes, and produce one remediation ledger.
- Reproduce every reported finding at the current head before changing code.
  Mark false positives with evidence; resolve every confirmed actionable
  finding before opening the pull request. Any deliberate exception requires
  explicit user approval and must remain visible in the plan and PR.
- After remediation, run the affected tests and full repository verification,
  then rerun both reviews against the final change set. Repeat until no
  confirmed actionable finding remains.

Review output is evidence, not authority by itself. A passing test suite does
not prove that a reported finding is fixed, and a reviewer summary does not
replace reproducing the behavior at the actual head.

### 7. Use the canonical structure

Read `templates/build-plan.md` completely and use its headings in the same
order.

Keep every section. If a section has no changes, say so explicitly, for
example, "No new runtime dependencies or environment variables."

## Section Requirements

### Summary

State:

- the outcome of the build
- the authority or ticket it satisfies
- the starting baseline
- the intended review unit
- the most important boundary

### Skills To Use

Always include `code-review` for the mandatory final Standards/Spec review.
List any additional skills that should guide implementation or verification,
with one short reason each.

### Scope

Separate `In scope` and `Out of scope`. Link deferred work to its owning ticket
or build when known.

### Package Layout / File-to-Task Mapping

Map each current or proposed file/module to one responsibility. Do not use this
section as a speculative file dump.

### Dependencies And Settings

Name:

- dependencies added, removed, or deliberately unchanged
- schema or storage versions
- environment variables and configuration precedence
- runtime/toolchain constraints

### Canonical Contracts

Define exact user- or machine-facing behavior. Include concrete command,
request, response, schema, or state examples when precision matters.

### TDD And BDD Implementation Strategy

Name the agreed test seams and order the work as vertical red/green slices.
When TDD is not appropriate, explain the equivalent verification-first
strategy instead of removing the section.

### Component Design

Explain ownership, data flow, transaction boundaries, state transitions, and
how the change fits current architecture. Prefer one deep module over a set of
thin forwarding abstractions.

### Failure And Recovery Rules

For each material failure mode, state:

- what must not change
- what error or blocked state is returned
- what evidence is retained
- what the operator or caller does next

### Commit Plan

Propose a small sequence of reviewable, behavior-closed commits. Keep tests
with the behavior they prove instead of placing all tests in a final cleanup
commit.

### Branch And PR Flow

Apply repository-specific Git instructions. Name the proposed branch and the
ticket-closing or linking reference. Include the mandatory dual-review and
remediation loop — Reviewer A plus an independent Reviewer B in a different
model family, both named with their pinned model, effort, and permission mode —
before the step that opens the pull request. Do not claim the branch or PR
exists unless it actually does.

### Test Plan

Include:

- focused behavior tests
- negative and regression cases
- integration or concurrency coverage when needed
- exact repository-native verification commands
- the shared review fixed point, plus the exact invocation for each reviewer
  including pinned model, effort, and permission mode
- focused and full verification after review remediation
- any manual acceptance that automation cannot prove

### Definition Of Done

Use observable outcomes, not implementation activities. A reviewer should be
able to decide whether the build is complete from this list.

### Assumptions And Defaults

Record decisions made to keep the plan executable. If an unresolved choice
would materially change scope, contracts, or architecture, ask the user instead
of burying it as an assumption.

## Quality Checklist

Before returning a plan, confirm:

- The title ends in `Build Plan`.
- The plan uses `Skills To Use`, not `Skills Used`.
- No time-boxed naming remains unless requested.
- The live authority and baseline were inspected.
- In-scope and deferred behavior have clear owners.
- File paths refer to real seams or are clearly marked new.
- Public contracts are explicit enough to test.
- Test seams and failure paths are named.
- The commit plan follows vertical, behavior-closed slices.
- Verification commands come from the repository.
- The Branch and PR flow includes two reviews from different model families
  against the same complete change set, each with a pinned model, effort, and
  permission mode.
- The plan requires findings to be reproduced, compared, deduplicated, and
  resolved before a pull request is opened.
- The plan requires both reviews to be rerun after remediation and forbids a
  silent model fallback.
- Definition of Done is observable.
- Planned, draft, approved, implemented, reviewed, and merged states are not
  conflated.

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
