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

### 4. Assess and select execution skills

Read [references/execution-skill-selection.md](references/execution-skill-selection.md)
and assess the skills available in the current session after the repository and
build scope are understood.

- Evaluate coverage across context and rules, architecture, stack
  implementation, behavior and tests, risk hardening, developer interfaces,
  focused audits, and final review. Every applicable stage requires selected
  skill coverage.
- Read each selected skill's `SKILL.md` completely before finalizing the plan,
  including only the supporting references required for this task.
- Select a skill only when it materially changes a concrete implementation,
  test, audit, or review step. State when it runs, what it governs, and what
  evidence it must produce.
- If an obviously relevant available skill is omitted because another selected
  skill covers the stage, record the reason when the omission would otherwise
  be surprising. If a useful language- or framework-specific skill is
  unavailable, name the available skill that will cover that execution stage
  and note the limitation.
- Prefer complete, non-redundant execution coverage. Do not optimize for the
  smallest list while leaving implementation or hardening work unguided.
- Use future tense and never imply that listing a skill means its work has
  already happened.

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

### 6. Require pre-PR code review and remediation

Every build plan must include a review gate before opening a pull request.

- Pin one fixed point and review the complete change set against it, including
  committed, staged, unstaged, and untracked files.
- Run the `code-review` skill available in the current agent against that fixed
  point. Preserve its separate Standards and Spec findings.
- Reproduce every reported finding at the current head before changing code.
  Compare and deduplicate overlapping root causes, mark false positives with
  evidence, and resolve every confirmed actionable finding before opening the
  pull request. Any deliberate exception requires explicit user approval and
  must remain visible in the plan and PR.
- After remediation, run the affected tests and full repository verification,
  then rerun `code-review` against the complete change set. Any later code
  change makes the review evidence stale. Continue only while the loop is
  making progress; otherwise stop with the remaining findings and evidence.

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
Present the selected skills as an execution map with the stage, skill, and
concrete responsibility in this build. Every applicable stage identified by
the execution-skill assessment must have selected skill coverage. Add a short
coverage note when an obvious skill is omitted because another skill covers the
stage or when an unavailable stack-specific skill limits that coverage.

Every selected skill must also appear where it changes the plan: design skills
in `Component Design`, testing and hardening skills in the vertical slices and
failure tests, audit skills in the pre-review flow and `Definition Of Done`, and
review skills in `Branch And PR Flow`.

### Scope

Separate `In scope` and `Out of scope`. Link deferred work to its owning ticket
or build when known.

### Package Layout / File-to-Task Mapping

Map each current or proposed file/module to one responsibility. Do not use this
section as a speculative file dump.

When two or more mapped files interact at runtime, add a Mermaid `flowchart`
showing that interaction. The mapping list states what each file owns; the
diagram states the path a request or command takes through them. Arrows carry
runtime call or data flow: a request, a value, a write. They are not import
edges, not directory nesting, and not authoring relationships such as an
example or template file a person copies before the system runs.

The diagram supplements the list and never replaces it. The list remains the
complete mapping, so the diagram carries only the files that take part in the
runtime interaction: every node has at least one edge, and a file that owns no
runtime interaction, such as a lockfile, a README, or an example config, stays
in the list and out of the diagram. Mark new files as new, and label file
nodes with literal paths; Mermaid drops `<angle-bracket>` placeholders as
unknown tags and renders an empty box. A runtime collaborator that is not a
file in this build, such as a datastore, queue, or external service, may
appear as a node when the flow is unreadable without it; label it as the
service it is rather than as a path. When the build touches a single file, or
no two mapped files interact at runtime, state that instead of drawing a
diagram.

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
ticket-closing or linking reference. Include mandatory `code-review`, finding
remediation, verification, and review freshness before the step that opens the
pull request. Do not claim the branch or PR exists unless it actually does.

### Test Plan

Include:

- focused behavior tests
- negative and regression cases
- integration or concurrency coverage when needed
- exact repository-native verification commands
- the shared review fixed point and mandatory `code-review` process
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
- The plan uses `Skills To Use` section.
- The available skills were assessed after the repository, stack, change
  surface, and material risks were understood.
- Every applicable execution stage has skill coverage.
- Each selected skill's instructions were read and the plan names when it runs,
  what it governs, and what evidence it produces.
- The selected skills materially shape the implementation, testing, hardening,
  audits, or review; the list is neither review-only nor decorative.
- Any obvious skill omission or unavailable language/framework skill is noted
  when it affects execution confidence.
- The live authority and baseline were inspected.
- In-scope and deferred behavior have clear owners.
- File paths refer to real seams or are clearly marked new.
- Every node in the file-interaction diagram has at least one edge and its
  arrows are runtime flow, or the plan states why no diagram applies.
- Public contracts are explicit enough to test.
- Test seams and failure paths are named.
- The commit plan follows vertical, behavior-closed slices.
- Verification commands come from the repository.
- The Branch and PR flow includes mandatory `code-review`.
- The plan requires completed-review findings to be reproduced, compared,
  deduplicated, and resolved before a pull request is opened.
- Definition of Done is observable.
- Planned, draft, approved, implemented, reviewed, and merged states are not
  conflated.

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
