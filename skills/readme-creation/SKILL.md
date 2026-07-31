---
name: readme-creation
description: Create or rewrite software READMEs in a direct, literal, succinct, engineer-first style. Use when a user asks for a new README, a major README rewrite, clearer product documentation, or documentation that explains exact outputs, users, downstream use, setup, and workflow boundaries without marketing language.
---

# README Creation

Create the shortest README that lets an engineer understand the software,
decide whether it fits, run it correctly, and know what happens next.

Lead with literal behavior. Name exact outputs. Explain downstream use. Keep
generated, approved, adopted, executed, and enforced states separate.

Direct does not mean incomplete. Succinct does not mean surface-level.

## Operating Boundary

- A request for a draft is chat-only. Do not edit files.
- A request to create or rewrite a repository README authorizes editing only
  the requested documentation unless broader changes are explicitly requested.
- Preserve unrelated worktree changes.
- Inspect current behavior before writing factual claims.
- Do not branch, commit, push, or open a pull request unless the user asks.

## Creation Workflow

### 1. Identify the README's job

Determine:

- the primary audience;
- the repository or deployment state they are in;
- the decision they need to make;
- the first workflow they need to complete; and
- the exact artifact, API, state change, or capability they receive.

Prefer one primary audience and workflow in the opening. Add secondary
audiences later when they materially change setup or use.

### 2. Build the factual product contract

Inspect the smallest authoritative source set:

- current README and repository instructions;
- CLI help or command registration;
- public APIs and service entry points;
- generated files and schemas;
- installation and release configuration;
- examples, fixtures, and smoke tests;
- documentation contract and parity tests;
- architecture, safety, and contribution docs; and
- live release or integration state when the README makes current claims.

Write down:

- what the software accepts or inspects;
- what it does;
- exactly what it returns, generates, changes, exposes, or operates;
- where those outputs live;
- what the user does with them;
- what the software deliberately does not do; and
- what current evidence proves each important claim.

If a source is unavailable, omit the claim or label the gap. Do not imply that
the source was checked.

### 3. Choose the product shape

Use the structure that matches the software.

#### Generator or workflow tool

Name the generated files, artifacts, state transitions, and approval steps.

#### CLI

Name the operation, exact commands, inputs, outputs, exit behavior, and changed
paths.

#### Library or SDK

Name the imported API, caller, inputs, returned values, errors, and one minimal
integration.

#### Service or daemon

Name accepted requests or events, returned or emitted output, stored state,
operating requirements, and failure behavior.

#### Framework

Name the application structure, runtime behavior, extension points, and what
developers build with it.

#### Integration or plugin

Name both systems, the action performed between them, authorization needs,
resulting state, and synchronization or lifecycle boundaries.

Combine shapes when the repository genuinely contains multiple public
surfaces. Do not force every README into a CLI template.

### 4. Write the opening literally

The first sentence must state the product's behavior with familiar technical
nouns.

Use one of these forms:

```text
<Tool> generates <exact artifacts> for <target repository or system>.
<Tool> reads <input> and produces <output> for <specific use>.
<Library> exposes <exact API or capability> for <specific callers>.
<Service> accepts <input> and returns, stores, or emits <output>.
<Integration> connects <system A> to <system B> and performs <exact action>.
```

For AI-assisted software-development tools, name the actual context artifacts
and lifecycle use. For example:

```text
<Tool> generates coding-convention rules, Agent Skills, and a root AGENTS.md
for future AI-assisted planning, code generation, testing, and review.
```

Do not open with:

- an undefined internal term such as "standards pack";
- implementation form alone, such as "a CLI and framework";
- "helps developers," "makes repositories agent-friendly," or another vague
  benefit;
- broad AI or industry context;
- architecture or safety internals; or
- marketing language.

Define branded or internal vocabulary only after the reader understands the
concrete behavior.

### 5. Explain exact outputs or interfaces

Immediately after the opening, give each primary output or interface its own
literal description.

For generated artifacts, state:

- exact path or artifact type;
- what it contains;
- what consumes it;
- whether it is canonical or derived;
- whether it is always generated, conditional, or optional; and
- whether the user must review or approve it.

Use a table when several outputs need the same mapping:

| Output | Contains | Used by | State |
|---|---|---|---|
| Exact path or artifact | Literal contents | Exact consumer | Proposed, generated, approved, or optional |

For APIs and services, map the equivalent input, output, caller, and state.

### 6. Explain downstream use

Describe how the software changes later work, not only what happens during the
first command.

For developer tools, consider:

- planning;
- implementation or code generation;
- testing;
- code review;
- security review;
- documentation;
- maintenance;
- operations; and
- release.

Include only supported lifecycle stages. State what context, constraint,
artifact, or decision the software supplies at each stage.

### 7. Name the user and fit

State who should use the software and the repository or system conditions that
make it useful.

Examples of useful distinctions:

- established repository versus greenfield project;
- maintainer versus application developer;
- platform team versus end user;
- local workflow versus hosted service;
- repository with existing conventions versus generic best-practice discovery.

Include a short "not for" statement when it prevents a likely category error.

### 8. Preserve responsibility and proof boundaries

State explicitly:

- what the tool does;
- what a host, developer, reviewer, or operator does;
- what is generated automatically;
- what requires review or approval;
- what event constitutes adoption, activation, publication, or completion;
- what is guidance;
- what is deterministically checked; and
- whether a command is mapped, executed, or proven to have passed.

Use "deterministic guardrail" only when a named mechanism enforces a bounded
property. Generated instructions and configuration are not enforcement by
themselves.

### 9. Provide a complete minimum path

The quick start must include:

1. prerequisites;
2. a current installation path;
3. required host or environment setup;
4. one exact first-use command or prompt;
5. the expected output or changed paths;
6. the required review or next action; and
7. cleanup, rerun, or recovery guidance when necessary.

Commands must be safe to copy. Avoid raw angle-bracket placeholders that a
shell interprets as redirection; use `/path/to/example` or quote placeholders.

### 10. Move depth to the right layer

Keep in the root README:

- literal product definition;
- exact primary outputs or interfaces;
- user and downstream use;
- minimum working setup;
- approval and proof boundaries;
- concise command or API reference;
- material safety constraints; and
- links to deeper documentation.

Move exhaustive schema fields, scoring formulas, internal module design,
benchmark methodology, complete flag matrices, and contributor-only detail to
linked docs unless a repository test or safe first use requires them in the
README.

Do not delete repository-specific release, security, compatibility, or
contribution requirements merely because they are not part of this common
spine.

## Default Section Order

Adapt this order to the product shape:

1. Name and literal one-sentence definition
2. Exact outputs, interfaces, or behavior
3. How the result is used
4. Who it is for and important non-goals
5. Human approval, state, and proof boundaries
6. Quick start
7. Concise CLI, API, configuration, or operating reference
8. Safety and limitations
9. Links to deeper docs
10. Development and contribution information

Do not begin with requirements, architecture, badges, a market problem, or
internal terminology unless repository-specific instructions require it.

## Engineer-First Language

Use:

- direct verbs: generates, reads, writes, validates, returns, stores, renders;
- familiar nouns: file, rule, command, request, response, check, skill;
- exact paths, command names, and state names;
- one idea per paragraph;
- short examples after abstract statements; and
- the same clear noun repeatedly when it remains the right noun.

Remove or rewrite:

- hand-wavy benefits;
- marketing adjectives;
- hype about AI, speed, transformation, or innovation;
- metaphors where a technical noun exists;
- undefined acronyms and branded categories;
- synonym cycling;
- canned binary corrections;
- broad setup before the product; and
- claims that do not change an engineer's understanding or decision.

Do not make the README sound like a landing page.

## Verification

Before returning or publishing the README:

1. Check every command against current help or source.
2. Verify every documented path exists or is genuinely generated.
3. Run documentation contract, parity, link, and example tests.
4. Run the smallest relevant repository verification.
5. Check the diff for accidental removal of required contracts.
6. Apply the `readme-audit` skill when available.
7. Report any current claim that could not be verified.

Passing tests do not prove the README is clear. The literal-output and
downstream-use gates still apply.

## Final Checklist

- The first sentence states literal behavior.
- Primary outputs or interfaces are named.
- Unfamiliar terms are defined after the behavior.
- The intended user and system state are explicit.
- Each output has a downstream use.
- Setup commands are current and safe to copy.
- Canonical, derived, conditional, and optional artifacts are distinguished.
- Generated, reviewed, approved, adopted, executed, and enforced states remain
  separate.
- Deterministic claims name the enforcing mechanism and bounded property.
- Safety and non-goals are concrete.
- Deep detail is linked without hiding required first-use information.
- The prose is direct, succinct, and free of marketing language.

## Response

When drafting in chat, return the README first and a short note naming any
unverified claims.

When editing a repository, report:

- files changed;
- the factual sources inspected;
- verification run and results; and
- any residual documentation or release-state gaps.

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
