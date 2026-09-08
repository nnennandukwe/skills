# skills

Agent skills for planning, testing, auditing, and documenting software changes. Each skill is a `SKILL.md` file your coding agent reads to follow a specific procedure: plan a bounded change before writing code, audit a developer interface, write the failure-path test first, or check a README against what the software actually does.

For the third-party skills in my workflow, see [Skills I use from other AI builders](skills-i-use-from-other-ai-builders.md), with original-source links and credit to their creators.

## The skills

| Skill | What it does | Writes files |
|---|---|---|
| [`software-build-plan`](skills/software-build-plan) | Produces an implementation-ready build plan for one ticket, including a deep assessment of available execution skills, contracts, file seams, test strategy, and pre-PR proof gates | No — planning is read-only by default |
| [`code-review`](skills/code-review) | Reviews a complete local change set against a fixed point on separate repository Standards and originating Spec axes | No — review is read-only by default |
| [`dx-audit`](skills/dx-audit) | Audits the changed developer interface across nine dimensions and reports prioritized findings plus the claims it could not verify | No |
| [`workflow-invariants`](skills/workflow-invariants) | Designs and tests state-machine invariants for multi-stage pipelines: transition tables, guards, and the tests that hold them | Yes — guards and tests |
| [`failure-path-testing`](skills/failure-path-testing) | Writes failure-first integration tests for workflow gates and recovery paths, starting from the path that must stay blocked | Yes — tests |
| [`tdd-bdd`](skills/tdd-bdd) | Turns a requirement or bug into Given/When/Then scenarios, picks the smallest useful test layer, and drives the change from a failing test | Yes — tests and production code |
| [`readme-audit`](skills/readme-audit) | Audits a README for factual accuracy, literal output descriptions, usable setup steps, and explicit workflow boundaries | No — reports by default |
| [`readme-creation`](skills/readme-creation) | Writes or rewrites a README that names exact outputs, downstream use, intended user, and approval boundaries | Only when asked to edit the repository |

## What these have in common

Each skill enforces one form of evidence discipline. The through-line:

**Plan before code.** `software-build-plan` locks contracts, test seams, and a commit sequence before implementation starts, and keeps planning separate from writing.

**Select skills for execution, not decoration.** `software-build-plan` assesses the skills available in the active agent against the build's architecture, stack, tests, hardening risks, interfaces, audits, and review needs. Every selected skill must own a concrete step and observable evidence in the rest of the plan.

**Review standards and intent separately.** `code-review` keeps repository Standards and the originating Spec as independent axes so correct-looking code cannot mask the wrong behavior, and exact feature work cannot mask a repository-rule violation. The build plan requires this review before a pull request; no external review service or account is required.

**Test the failure path.** `failure-path-testing` and `workflow-invariants` start from what must *not* happen — the transition that should be refused, the stage that should stay blocked, the empty config that should fail loudly instead of resolving to a wrong default.

**Audit against what the repository actually committed to.** `dx-audit` reads a repo-local audit checklist first and treats it as the standard, above any generic signal. Repo-local expectations outrank received best practice.

**Say what you could not verify.** `dx-audit` is read-only and cannot execute the interface it audits, so its report ends with a required section listing the exact commands that would settle each unverified claim. An interface whose behavior cannot be predicted from its source is itself a finding, and a developer who knows which three commands to run has received something useful. That section can never be omitted — an empty one is a claim about audit completeness, so it has to be made deliberately.

## Install

Requires [Node.js](https://nodejs.org) 22.20 or newer — the installer declares that engine and older versions emit an `EBADENGINE` warning. Install every skill:

```bash
npx skills add nnennandukwe/skills
```

Install one:

```bash
npx skills add nnennandukwe/skills --skill software-build-plan
```

The installer is the [Vercel Labs `skills` CLI](https://github.com/vercel-labs/skills), which supports Claude Code, Codex, Cursor, OpenCode, and roughly seventy other agents. It copies the selected skill directories into the location your agent expects. Pass `--agent` to target a specific one, or run without flags to choose interactively.

To install by hand, copy any `skills/<name>/` directory into your agent's skills directory — `~/.claude/skills/` for Claude Code, `~/.codex/skills/` for Codex.

## First use

After installing, ask your agent to use a skill by name. For a build plan:

```
Use software-build-plan for this ticket.
```

The agent returns a build plan in the conversation. It does not edit code, create a branch, or open a pull request unless you separately ask for implementation. Save the plan only if you want it in the repository.

For a review of a complete local change set:

```
Use code-review against origin/main.
```

The reviewer pins the fixed point, includes committed and working-tree changes,
and reports Standards and Spec findings separately.

For an audit of work you just finished:

```
Use dx-audit on my current changes.
```

`dx-audit` reads your `git diff` and returns a prioritized report — P1 findings that block shipping, P2 rough edges, P3 polish — followed by the verification gaps it could not close without running your CLI.

## Who these are for

Engineers using a coding agent on a repository that already has conventions, tests, and a review process. The skills assume there is something to be consistent with: a base branch, a test suite, documented guidelines, and a reviewer.

**Not for** greenfield scaffolding or generating a project from nothing. `software-build-plan` inspects real implementation seams before naming files, and `dx-audit` scopes itself to a diff. On an empty repository both have nothing to read.

## Boundaries

The skills separate what an agent proposes from what a human accepts.

- A build plan is a proposal. Nothing in it is implemented, reviewed, or merged by writing it down.
- An audit reports findings. It does not fix them, and `dx-audit` never modifies files.
- The `code-review` gate is a documented requirement, not an automated enforcement mechanism. Running the review and resolving confirmed findings remains part of the implementation workflow.
- `tdd-bdd`, `failure-path-testing`, and `workflow-invariants` write tests and code. Review their changes as you would any contribution.

## Layout

```
skills/<name>/SKILL.md          the skill — required, every skill has one
skills/<name>/agents/           agent-specific interface metadata — every skill has one
skills/<name>/templates/        output templates — software-build-plan only
skills/<name>/references/       supporting detail loaded on demand — software-build-plan and tdd-bdd
```

`SKILL.md` is the only file an agent needs; the rest are loaded when the skill calls for them.

`skills/software-build-plan/templates/build-plan.md` is the canonical plan structure and its section order is required, not suggested. Its execution-skill reference defines how an agent assesses available skills and carries each selection into concrete implementation and proof steps. `skills/tdd-bdd/references/` holds test-layer selection and scenario-shaping guidance. Each `agents/openai.yaml` supplies a display name and default prompt for Codex; other agents ignore it.

## Limitations

`dx-audit` declares `allowed-tools` and `argument-hint`, which Claude Code honors and other agents ignore. Its `--focus` flag works anywhere, but agents pass skill arguments differently — if yours does not substitute an argument variable, state the focus in your request.

Skills are instructions, not code. They shape what an agent does; they do not constrain it the way a linter or a test does. Treat their output as a contribution to review, not a guarantee.

## License

[Apache-2.0](LICENSE). Each `SKILL.md` carries a provenance comment so it stays attributable after being copied into another repository.
