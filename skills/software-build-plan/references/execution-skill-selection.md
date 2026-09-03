# Execution Skill Selection

Use this reference after inspecting the repository and bounding the build, but
before drafting the implementation strategy. The goal is complete execution
coverage, not the shortest possible skill list.

## Assessment Workflow

1. Inspect the skills available in the current session. Do not assume a skill
   exists because it was available in another workspace or earlier run.
2. Derive the build's implementation workstreams and material risks from the
   live repository, ticket, and proposed change surface.
3. Assess every applicable execution stage in the table below. Select a skill
   when its instructions will change how that stage is performed or verified.
4. Read each selected `SKILL.md` completely before finalizing the plan. Read
   only the supporting references that the selected skill requires for this
   task.
5. Map every selected skill to a concrete plan step and an observable output,
   test, audit result, or gate.
6. Record why an obviously relevant available skill was not selected when the
   omission would otherwise be surprising.

## Execution Coverage

| Stage | Questions to ask | Typical skill categories |
|---|---|---|
| Context and rules | Are workspace rules, prior implementation patterns, or coupled repositories needed before coding? | Rules retrieval, codebase understanding, guidelines |
| Architecture and domain | Does the build introduce or move an interface, state owner, module seam, or domain concept? | Codebase design, domain modeling, prototype |
| Stack implementation | Does the language, framework, CLI, API, database, or deployment target have a dedicated implementation skill? | Language, framework, CLI, frontend, infrastructure |
| Behavior and tests | Should the change be driven by examples, contracts, failing tests, or a particular test layer? | TDD/BDD, language testing, contract testing |
| Risk hardening | Which failure, recovery, concurrency, transaction, migration, security, temporal, or workflow risks need specialized treatment? | Failure paths, workflow invariants, concurrency, transaction safety |
| Developer interface | Does the change affect commands, public interfaces, errors, logs, setup, docs, or generated artifacts? | DX, CLI parity, README, API ergonomics |
| Focused audits | Which likely defect classes should be checked after implementation? | Drift, dead code, errors, boundaries, abstractions, names, idioms |
| Review and remediation | What review skill owns the complete-diff gate before the pull request? | The review workflow required by this skill variant |

Stages that are genuinely irrelevant are outside the execution map. Every stage
determined to be applicable must have at least one selected available skill;
repository-native commands supplement that coverage but do not replace it.

## Selection Standard

Include a skill only when all of these are true:

- it is available in the current session;
- its trigger matches the actual change or a material risk;
- its instructions affect at least one implementation, test, audit, or review
  step; and
- the plan states when it runs and what evidence it must produce.

Prefer complete, non-redundant coverage. Do not trade away an uncovered
implementation or hardening stage merely to keep the list short. When two
skills substantially overlap, choose the one that best matches the concrete
risk or explain their distinct responsibilities.

## Plan Integration

The `Skills To Use` section is an execution map, not a catalog. For each skill,
state:

- **stage**: when it is used;
- **responsibility**: what decision or work it governs; and
- **evidence**: which test, artifact, audit result, or gate demonstrates that it
  was applied.

Carry the selection into the rest of the plan:

- design skills must shape `Component Design`;
- testing skills must shape the vertical slices and `Test Plan`;
- hardening skills must shape `Failure And Recovery Rules` and negative tests;
- interface and audit skills must appear in the implementation or pre-review
  flow and `Definition Of Done`; and
- review skills must appear in `Branch And PR Flow` with freshness rules.

If a language- or framework-specific skill would materially help but none is
available, identify which selected available skill covers that execution stage
and note the limitation. If no available skill can cover an applicable stage,
the plan is not implementation-ready; surface the coverage gap instead of
presenting the stage as covered.

## Anti-Patterns

- Listing only the final review skill.
- Naming design or review skills while leaving implementation and test work
  unguided.
- Treating a repository command or an explanation as skill coverage for an
  applicable execution stage.
- Copying every available audit skill into the plan without a concrete risk.
- Naming a skill without reading its instructions or integrating its required
  workflow.
- Listing a skill that is unavailable in the current session.
- Treating a skill name as evidence that its audit, test, or review has already
  happened.
