---
name: readme-audit
description: Audit a software README for engineer-first clarity, factual accuracy, literal product and output descriptions, usable setup instructions, and explicit workflow boundaries. Use when a user asks whether a README is clear, complete, too hand-wavy, too marketing-heavy, or ready for developers. Report findings by default; edit only when explicitly asked.
---

# README Audit

Audit whether an engineer can understand, evaluate, and use the software from
the README without guessing what internal terms mean.

Engineers should not have to reverse-engineer the product from branding,
architecture, implementation details, or vague outcomes. Require literal
nouns, exact artifacts, real commands, and honest state boundaries.

Do not modify files unless the user explicitly asks for a rewrite or fix.

## Core Standard

A strong README answers these questions in this order:

1. What does the software do?
2. What exact thing does it return, generate, change, expose, or operate?
3. Who is it for?
4. What can they use the result for?
5. How do they install and run it?
6. What must they review, approve, configure, or operate themselves?
7. What is automated, deterministic, advisory, optional, or out of scope?
8. Where should they go for deeper technical detail?

The README may include additional repository-specific sections. These
questions are the common spine, not a fixed universal template.

## Audit Workflow

### 1. Establish the factual contract

Read the current README and the smallest set of sources needed to verify it:

- CLI help and command registration;
- public APIs or service entry points;
- generated artifact paths;
- schemas and configuration examples;
- tests that enforce documentation or command parity;
- architecture, installation, release, and contribution docs; and
- applicable `AGENTS.md` or repository instructions.

Do not infer current behavior from the README alone. Do not claim that a
release, integration, check, or workflow exists without current evidence.

Record any source you could not inspect.

### 2. Apply the thirty-second test

From the opening section alone, determine whether an engineer can state:

- the product category;
- the literal behavior;
- the exact primary outputs or interfaces;
- the intended repository, system, or audience;
- what the output is used for; and
- the human or system boundary after generation or execution.

Treat a missing answer as a finding. Do not excuse it because a later section
eventually explains the product.

### 3. Audit the first sentence

The first sentence must use familiar technical nouns and a concrete verb.

Good patterns:

```text
<Tool> generates <exact artifacts> for <target system or repository>.
<Library> exposes <exact API or capability> for <specific callers>.
<Service> accepts <input> and returns, stores, or emits <output>.
<Integration> connects <system A> to <system B> and performs <exact action>.
```

Flag openings that:

- introduce an undefined internal term before explaining it;
- say only that the software "helps," "enables," or "makes X easier";
- call a repository or workflow "AI-ready," "agent-friendly," or "modern"
  without naming what changes;
- identify implementation form, such as "a CLI and framework," without
  stating the actual behavior;
- lead with architecture, safety internals, or market context; or
- require the next paragraph to reveal what the product produces.

An internal label may appear after the concrete behavior is understood.

### 4. Audit exact outputs and interfaces

For generators and workflow tools, require the README to name:

- every primary generated artifact;
- exact paths or artifact types when stable;
- which files are canonical versus derived;
- which outputs are always generated, conditional, or optional; and
- what a developer does with each output.

For libraries, services, frameworks, and integrations, apply the equivalent
contract:

- imported API and returned values;
- accepted request and emitted response or event;
- runtime behavior and extension points; or
- connected systems and resulting state changes.

Flag category labels such as "rules pack," "platform," "control plane," or
"developer infrastructure" when the README does not immediately define their
contents.

### 5. Audit the user and future use

Require a literal target user and repository state. "Developers" alone is not
enough when the tool is meant for maintainers, platform teams, existing
repositories, greenfield projects, operators, or AI-assisted workflows.

Require the README to explain what happens after the first successful run:

- how generated files are consumed;
- how an API is called in later work;
- how a service changes an operating workflow;
- how results affect planning, implementation, testing, review, maintenance,
  or release; and
- which step constitutes adoption, activation, publication, or completion.

Flag outputs that are described without their downstream use.

### 6. Audit guidance, proof, and approval boundaries

Require the README to separate:

- generated from approved;
- draft from adopted;
- mapped command from executed command;
- guidance from deterministic enforcement;
- optional output from default output;
- local state from published or activated state; and
- tool responsibility from host, developer, or operator responsibility.

The phrase "deterministic guardrail" is valid only when the README identifies
the existing mechanism that enforces the rule or property. A tool that merely
records a command, writes instructions, or generates configuration must not
claim enforcement.

### 7. Audit setup and first use

Verify that the README provides:

- current prerequisites;
- a valid installation path;
- exact commands that can be copied safely;
- required host, plugin, skill, service, or environment setup;
- one minimal first-use example;
- the expected output or changed paths; and
- the next developer action.

Run the smallest safe verification available, such as `--help`, documentation
contract tests, link checks, or a dry run. Do not execute destructive or
state-changing examples unless authorized.

### 8. Audit language and structure

Prefer:

- literal technical nouns;
- direct verbs;
- short paragraphs;
- tables for artifact-to-purpose or mode-to-behavior mappings;
- repeated use of the clearest noun;
- examples immediately after abstract claims; and
- deeper docs for schema, exhaustive flags, and implementation detail.

Flag:

- marketing claims and hype;
- metaphors in place of behavior;
- fluffy adjectives;
- synonym cycling;
- broad industry setup before the product definition;
- paragraphs that mix product, implementation, safety, and adoption;
- headings that hide the developer question they answer; and
- exhaustive internal detail that obscures first use.

Succinct does not mean shallow. Remove text that does not change a developer's
understanding or decision. Keep the details needed to use the software safely.

## Severity

- `P1`: the README misstates behavior, hides the primary output, gives invalid
  commands, collapses proof states, or leaves the reader unable to use the
  software.
- `P2`: the audience, downstream use, approval boundary, important prerequisite,
  or canonical artifact is unclear.
- `P3`: wordiness, ordering, terminology, or formatting creates avoidable
  friction without changing the factual contract.

Do not assign arbitrary numeric scores.

## Report Format

```markdown
## README Audit

**Bottom line:** One direct sentence stating whether an engineer can understand
and use the software from the README.

**Evidence checked:** README plus the exact code, help, tests, or docs inspected.
**Coverage limits:** Sources or behavior that could not be verified.

### P1

- `README.md:line` Finding.
  Why it matters: ...
  Required change: ...

### P2

- ...

### P3

- ...

### Replacement opening

Provide a literal replacement only when the current opening fails.

### Required verification

- Commands or checks needed after a rewrite.
```

If there are no findings, say `No findings.` and list any unverified claims or
residual risks.

## Rewrite Mode

When the user explicitly asks for fixes:

1. Complete the audit first.
2. Preserve verified product behavior and repository-specific requirements.
3. Rewrite using the core standard and the appropriate product shape.
4. Keep deep implementation detail in linked docs unless it is needed for
   correct first use.
5. Run documentation contract tests, CLI parity tests, link checks, and
   repository verification that apply to the change.
6. Report the changed files and any claims that remain unverified.

Use the `readme-creation` skill for a full rewrite when it is available.

## Hard Gates

Do not pass a README when any of these remain:

- the first sentence does not state literal behavior;
- primary outputs or interfaces are unnamed;
- an internal term is required to understand the product;
- the target user or repository is ambiguous;
- downstream use is missing;
- setup instructions are not executable;
- generated, approved, adopted, and enforced states are collapsed;
- deterministic enforcement is claimed without a named mechanism; or
- current factual claims were not checked against available source evidence.

<!-- Source: https://github.com/nnennandukwe/skills · Author: Nnenna Ndukwe · Apache-2.0 -->
