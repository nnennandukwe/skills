---
name: dx-audit
description: This skill should be used after modifying any code that forms part of the developer interface of a project — including CLI commands, public APIs, error handling, logging, setup scripts, docs, or module structure. Audits Developer Experience across nine dimensions grouped into surface DX (CLI ergonomics, error handling, setup friction, output/feedback, docs-versus-runtime agreement) and code DX (API ergonomics, debugging/observability, testability, extensibility). Produces a prioritized report of findings plus an explicit list of claims it could not verify without executing the interface. Does not modify code.
argument-hint: "[--focus surface|code|cli|api|errors|setup|output|docs|debug|testability|architecture]"
allowed-tools: Read, Glob, Grep, Bash(git diff*), Bash(find *)
---

# DX Audit Skill

You are performing a Developer Experience audit. Your job is to surface friction — not fix it. Do not modify any files. Do not suggest adding features. Audit what exists and report what a developer would hit.

## Step 1: Determine scope

Determine whether a `--focus` value was supplied. Agents pass skill arguments differently — check the argument variable your agent substitutes if it has one, and otherwise read the focus directly from the user's request text. If no focus is given, run everything.

Focus values:
- `--focus surface` → audit CLI Ergonomics, Error Handling, Setup & Onboarding, Output & Feedback, Docs & Runtime Agreement only
- `--focus code` → audit API Ergonomics, Debugging & Observability, Testability, Extensibility & Architecture only
- `--focus cli` → CLI Ergonomics only
- `--focus api` → API / Function Ergonomics only
- `--focus errors` → Error Handling only
- `--focus setup` → Setup & Onboarding only
- `--focus output` → Output & Feedback only
- `--focus docs` → Docs & Runtime Agreement only
- `--focus debug` → Debugging & Observability only
- `--focus testability` → Testability only
- `--focus architecture` → Extensibility & Architecture only
- No flag → run all nine dimensions

Find changed files:
```bash
git diff --name-only
git diff --cached --name-only
```

If both return nothing, ask the user: "No git diff found. Which files should I audit?"

Identify project type by scanning the changed files and their entry points:
- **CLI tool**: has `bin/`, or uses `commander` / `argparse` / `click` / `typer`
- **Library/SDK**: has a public `__init__.py` or `index.ts` with exports
- **Both**: audit both surfaces (like a CLI tool with a Python core)

Only read files from the diff plus their directly related entry points. Do not sweep the whole codebase.

If the repository has a root `DX-AUDIT.md`, `RUNBOOK.md`, or similar audit checklist, read it before auditing and treat it as the repo-specific standard. Repo-local expectations outrank the generic signals below when they conflict.

## Step 2: Nine audit dimensions

For each dimension in scope, read the relevant changed files and their entry points. Look for the specific signals listed. Record every finding with file path and line number.

---

### Surface DX

#### CLI Ergonomics

Look for:
- Required arguments that have an obvious default the tool could detect automatically (e.g., current working directory, current git diff, current branch name) — flag as auto-detection opportunity
- Commands that require the user to complete more than one manual prerequisite step before the command produces value
- `--help` output that is just a flag dump with no descriptions, examples, or guidance on common usage
- Flag names that are not discoverable without reading source code (cryptic abbreviations, internal jargon)
- Capability reachable from the interactive path but not the scriptable one, or the reverse — a script user should not have to drop into an interactive session to reach a feature, and an interactive user should not have to hand-write flags an interactive prompt could collect

#### Error Handling

Look for:
- Error messages that state what went wrong but give no guidance on what the developer should do next
- Code paths that exit without a non-zero exit code on failure (especially in CLI entry points)
- Silent failures: paths where the process exits 0 but nothing happened and nothing was reported
- Error output that leaks internal paths, raw stack traces, or implementation details to the end user
- Bare `except Exception` or equivalent catch-all blocks that swallow errors and emit only a generic message

#### Setup & Onboarding

Look for:
- Count the steps from `git clone` to the first working command — flag if more than 3 steps are required
- Required environment variables with no `.env.example`, no inline documentation, and no error message that names the missing variable
- External dependencies (Docker, specific system tools, global npm packages) that must be installed manually and are not covered by the setup script or README
- Commands that depend on infrastructure (a running service, a database, a vector store) with no check that the prerequisite is available before proceeding

#### Output & Feedback

Look for:
- Interactive prompts or progress messages written to stdout instead of stderr (this breaks piping and scripting)
- Operations that take more than ~2 seconds with no progress signal (spinner, log line, status update)
- Human-readable prose and machine-readable data (JSON, JSONL) mixed on the same stream
- Operations that write files or artifacts with no confirmation of what was written and where it landed

#### Docs & Runtime Agreement

Compare what the documentation promises against what the changed code now does. A doc that describes last month's behavior is a defect, not a wording nit — it costs a developer the same time as a bug and erodes trust in every other doc in the repo.

Look for:
- README, runbook, or operator-guide steps that no longer match current runtime behavior
- Documented usage that contradicts the flags, arguments, or subcommands the parser actually accepts
- `.env.example` missing variables the code now requires, or still listing variables it no longer reads
- Architecture or state-boundary docs naming file locations, state paths, or artifact directories the code has moved
- Docs, help text, or error messages that reference a command, flag, or script that no longer exists
- A changed workflow documented in one surface but not the others — README updated while the runbook, `--help`, and error text still describe the old flow

---

### Code DX

#### API / Function Ergonomics

Look for:
- Function signatures that require the caller to understand internal implementation details to call correctly
- Parameters that are effectively always the same value but are still required — should be optional with a sensible default
- Return types that force callers to inspect internals (returning a raw dict when a typed dataclass or named tuple would be self-documenting)
- Functions that do more than one distinct thing, making them hard to call in isolation or test independently
- Inconsistent naming conventions across related modules (e.g., `run_X` in one module, `execute_X` in another for the same pattern)

#### Debugging & Observability

Look for:
- Multi-step pipelines or operations with no logging or tracing at meaningful boundaries (entering a stage, completing a stage, key intermediate values)
- Exception handling that loses context: re-raising without chaining (`raise NewError(...)` instead of `raise NewError(...) from original`), or catching and logging only the message without the traceback
- No way to inspect intermediate state during a multi-step pipeline without adding print statements or a debugger
- Side effects (file writes, network calls, database mutations) with no audit trail or log entry

#### Testability

Look for:
- Modules with hidden dependencies: imports inside functions, global mutable state, singleton instances created at module load time
- Functions that mix I/O with logic, making unit testing require real infrastructure (a running database, a real filesystem, a live API)
- Missing injectable seams: no way to pass in a fake, mock, or stub for an external dependency (hardcoded client instantiation, no dependency injection)
- Side effects in constructors or at module import time that make the module hard to import in a test context

#### Extensibility & Architecture

Look for:
- Adding a new command, skill, handler, or plugin requires touching many files rather than one registration point
- Parallel structures that accomplish the same goal in two different ways with no shared abstraction enforcing consistency
- Tight coupling between modules that should be independent (a module importing from another that it has no logical relationship to)
- A pattern that has been applied inconsistently: two ways to do the same thing exist in the codebase side by side

**Important:** For architecture findings, state the problem and why it matters. Do **not** propose a solution — these require human judgment.

---

## Step 3: Prioritize findings

Assign each finding a priority:

- **P1 — Friction**: A developer will hit this immediately and be blocked or confused. Includes: silent failures, missing exit codes, no way to know what to do next, required prerequisites with no check, operations that appear to succeed but don't.
- **P2 — Rough edges**: Noticeable after a few uses. Erodes confidence over time. Includes: inconsistent naming, missing progress signals, return types that require inspection, non-injectable dependencies.
- **P3 — Polish**: An experienced developer would appreciate the improvement. Includes: auto-detection opportunities, help text improvements, minor naming inconsistencies.

If a dimension has no findings, write one line: `[Dimension]: No findings.`

---

## Step 4: Record verification gaps

This skill is read-only and cannot execute the interface it audits. Its tools cover reading files, searching, and `git diff` — not running the CLI, the test suite, or a setup script.

That boundary is deliberate, and it has a consequence you must make visible: some findings depend on runtime behavior you cannot observe. Reading an argument parser tells you what flags exist; it does not tell you what `--help` actually prints. Reading an error path tells you a message is constructed; it does not tell you the process exits non-zero.

For every such case:

- Do not assert the runtime outcome. State what the source implies and mark it unverified.
- Record the exact command that would settle it.
- Say which finding or which dimension the command resolves.

Verification gaps are audit output, not an apology. An interface whose behavior cannot be predicted from its source is itself a DX signal, and a developer who knows exactly which three commands to run has received something useful.

Common gaps worth naming explicitly:

- `--help` output quality and accuracy, for every changed command
- actual exit codes on the failure paths you identified
- whether a documented setup sequence still completes from a clean checkout
- whether generated artifacts land where the docs say they do
- whether progress output goes to stderr and data to stdout in practice

---

## Step 5: Write the report

Output the following format exactly:

```markdown
## DX Audit

**Changed files:** [list changed files]
**Dimensions audited:** [list dimensions audited, or "all eight"]

---

### 🔴 P1 — Friction

**[Dimension]** `file:line`
[What the problem is and why a developer hits it.]
**Fix:** [Specific, actionable recommendation — or "Architecture decision required" for structural findings]

---

### 🟡 P2 — Rough edges

[same format]

---

### 🔵 P3 — Polish

[same format]

---

### ⚪ Verification gaps

Not executed — this audit is read-only. Run these to confirm or retire the findings above.

- `[exact command]` → resolves [finding or dimension]
- `[exact command]` → resolves [finding or dimension]

---

*N findings total (X P1, Y P2, Z P3), plus G unverified claims. P1s should be addressed before shipping the changed interface.*
```

If a priority bucket has no findings, omit that section entirely. Never omit **Verification gaps** — if there are genuinely none, write `None — every finding above was confirmed from source.` An empty gaps section is a claim about audit completeness, so make it deliberately rather than by omission.

---

## Guidelines

- Only audit the changed files and their directly related entry points. Do not sweep the whole codebase.
- Do not suggest adding features. Surface friction in what already exists.
- Architecture findings: state the problem and why it matters; do not propose a solution.
- Be specific. Every finding must include a file path and line number (or range).
- Be concise. One clear paragraph per finding. No padding.
- P1s block shipping. P2s and P3s are improvements, not blockers.
- Prefer evidence you can actually read — source, diffs, docs, tests — over speculation. A finding that depends on runtime behavior belongs in Verification gaps, not in a priority bucket asserted as fact.
- A docs-versus-runtime mismatch is a release-quality issue, not a wording nit. Rank it on the time it costs a developer, the same as any other finding.

## High-Signal Patterns

These recur across CLI- and workflow-heavy repositories. When the diff is ambiguous about where to look, start here.

- When a workflow changes, audit its surfaces together: README, architecture docs, demo or example scripts, known-limitations docs, `.env.example`, and CI config. Workflows are almost never documented in one place.
- When a workflow writes files or launches a background process, check that its output names the artifact path, the workspace, or the recovery step. A developer who cannot find what a command produced has to go read the source.
- Settings drift is high-yield: an environment variable renamed in code but not in `.env.example`, a default changed in one layer only, or a path assumption that holds locally and breaks on install.
- Operator-facing recovery text is usually the weakest surface in a repository, because it is written once during implementation and never read again by its author.

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
