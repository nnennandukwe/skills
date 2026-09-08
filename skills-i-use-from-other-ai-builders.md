# Skills I use from other AI builders

I use agent skills from other builders for research, architecture, testing,
debugging, review, and technical writing. This page collects the ones in my
workflow, explains where they help, and links to the original creators' work.

After I shared my own skills, Tuvya Khatter asked which skills from other
builders I reach for when working on projects. Thanks to Tuvya for prompting
this list and a closer look at the people behind my setup.

Some of my installed copies have local adaptations. The links below point to
the original projects so you can read their instructions and follow their
installation guidance.

## Research, design, testing, and review

| Skill | Creator / maintainer | Where it helps |
|---|---|---|
| [research](https://github.com/mattpocock/skills/tree/main/skills/engineering/research) | [Matt Pocock](https://github.com/mattpocock) | Investigating technical questions through primary sources before making implementation decisions. |
| [domain-modeling](https://github.com/mattpocock/skills/tree/main/skills/engineering/domain-modeling) | [Matt Pocock](https://github.com/mattpocock) | Clarifying terminology, challenging assumptions, and recording architectural decisions. |
| [codebase-design](https://github.com/mattpocock/skills/tree/main/skills/engineering/codebase-design) | [Matt Pocock](https://github.com/mattpocock) | Designing module boundaries and interfaces that make code easier to understand and test. |
| [tdd](https://github.com/mattpocock/skills/tree/main/skills/engineering/tdd) | [Matt Pocock](https://github.com/mattpocock) | Building through a red-green-refactor loop, with tests focused on observable behavior. |
| [diagnosing-bugs](https://github.com/mattpocock/skills/tree/main/skills/engineering/diagnosing-bugs) | [Matt Pocock](https://github.com/mattpocock) | Establishing a reliable reproduction, testing hypotheses, and verifying a fix. |
| [code-review](https://github.com/mattpocock/skills/tree/main/skills/engineering/code-review) | [Matt Pocock](https://github.com/mattpocock) | Reviewing repository standards and the intended specification as separate concerns. |
| [golang-testing](https://github.com/samber/cc-skills-golang/tree/main/skills/golang-testing) | [Samuel Berthe (`samber`)](https://github.com/samber) | Writing and reviewing Go tests, including concurrency and integration scenarios. |
| [golang-cli](https://github.com/samber/cc-skills-golang/tree/main/skills/golang-cli) | [Samuel Berthe (`samber`)](https://github.com/samber) | Working through Go command structure, flags, configuration, exit behavior, and testing. |
| [gh-fix-ci](https://github.com/openai/skills/tree/main/skills/.curated/gh-fix-ci) | [OpenAI](https://github.com/openai/skills) | Investigating failing GitHub Actions checks and connecting logs to a concrete fix. |

## Browser workflows

| Skill | Creator / maintainer | Where it helps |
|---|---|---|
| [agent-browser](https://github.com/vercel-labs/agent-browser/tree/main/skills/agent-browser) | [Vercel Labs](https://github.com/vercel-labs/agent-browser) | Navigating websites, interacting with forms, and inspecting browser behavior. |
| [playwright](https://github.com/openai/skills/tree/main/skills/.curated/playwright) | OpenAI; adapted from [Microsoft's Playwright CLI skill](https://github.com/microsoft/playwright-cli/tree/main/skills/playwright-cli) | Driving browser flows from the terminal for inspection and debugging. |

## Qodo's skills

I also use [Qodo's skills](https://github.com/qodo-ai/qodo-skills) across the
development workflow. Credit belongs to Qodo and its contributors. I've
[contributed to the repository's Codex setup documentation](https://github.com/qodo-ai/qodo-skills/commit/9fc49d5f4c8824b7781b1c6a822868657cf6bafc).

| Skill | Creator / maintainer | Where it helps |
|---|---|---|
| [qodo-get-rules](https://github.com/qodo-ai/qodo-skills/tree/main/skills/qodo-get-rules) | [Qodo and contributors](https://github.com/qodo-ai/qodo-skills) | Retrieving coding rules relevant to the current task. |
| [qodo-codebase-wisdom](https://github.com/qodo-ai/qodo-skills/tree/main/skills/qodo-codebase-wisdom) | [Qodo and contributors](https://github.com/qodo-ai/qodo-skills) | Investigating codebase behavior, history, and relationships across repositories. |
| [qodo-review](https://github.com/qodo-ai/qodo-skills/tree/main/skills/qodo-review) | [Qodo and contributors](https://github.com/qodo-ai/qodo-skills) | Reviewing local changes before opening a pull request. |
| [qodo-review-resolver](https://github.com/qodo-ai/qodo-skills/tree/main/skills/qodo-review-resolver) | [Qodo and contributors](https://github.com/qodo-ai/qodo-skills) | Inspecting and working through PR review findings. |

## Technical writing

| Skill | Creator / maintainer | Where it helps |
|---|---|---|
| [avoid-ai-writing](https://github.com/conorbronsdon/avoid-ai-writing) | [Conor Bronsdon](https://github.com/conorbronsdon) | Reviewing technical prose for formulaic language and making the writing more direct. My copy is locally adapted. |
| [curiosity-gap](https://github.com/filiphric/skills/tree/main/skills/curiosity-gap) | [Filip Hric](https://github.com/filiphric) | Checking whether an opening gives readers a concrete reason to continue and whether the content delivers on that promise. |

`curiosity-gap` is a recent addition: I installed it on September 8, 2026, and
adapted it into my editorial workflow. It belongs here with that context,
alongside the skills I've used over a longer period.

## Getting started

Start with the skill that addresses a problem you already encounter. Read its
instructions, then follow the original project's installation guidance.

The source links identify the work credited here, including when different
repositories use the same skill name. Upstream versions may differ from my
installed copies. Each original project retains its own attribution and
license terms.

*Sources and links checked September 9, 2026.*
