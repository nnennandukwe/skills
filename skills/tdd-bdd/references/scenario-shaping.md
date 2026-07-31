# Scenario Shaping

Use this reference when requirements are underspecified, bug reports mix symptoms with guesses, or a feature request needs to be broken into small Given/When/Then scenarios.

## Scenario Template

Use one concrete rule per scenario:

```text
Given <relevant starting state>
When <single trigger>
Then <observable outcome>
```

Good scenarios are:

- Observable
- Small
- Deterministic
- Free of implementation detail

## Start With The Simplest Example

Choose the smallest example that proves the rule.

Examples:

- Empty cart gets free-shipping banner removed
- Invalid token returns 401
- Duplicate username is rejected
- Archived record is hidden from active lists

Only add richer fixtures after the simplest example is covered.

## Slicing Heuristics

Split the behavior when the story contains:

- `and` in the `When`
- Multiple outcomes in the `Then` that can fail independently
- Setup that does not matter to the rule being tested
- UI narration that hides the actual business rule

Prefer:

```text
Given a suspended account
When the user signs in
Then access is denied
```

Over:

```text
Given a suspended account with three pending invoices and a profile image
When the user opens the app, clicks Sign In, submits email and password, waits for redirect, and opens billing
Then access is denied, invoices stay pending, and a warning banner is shown
```

## Bug Regression Template

Turn bug reports into this shape:

1. Remove any guessed cause from the scenario.
2. Keep only the triggering conditions.
3. State the expected outcome and the actual wrong outcome.
4. Write the expected outcome as the new test.

Example:

- Report: "CSV import crashes when a row has a blank date because the parser assumes ISO strings."
- Scenario:

```text
Given an import row with a blank date
When the import runs
Then the row is rejected with a validation error instead of crashing
```

## Acceptance Criteria Template

For feature work, derive:

1. One happy-path scenario
2. One validation or gating scenario
3. One edge case if the domain obviously demands it

Example:

```text
Given an unpublished draft
When an editor clicks Publish
Then the draft becomes publicly visible

Given a draft missing a title
When an editor clicks Publish
Then publishing is blocked with a title-required message
```

## Naming Tests

Prefer names that state behavior, not implementation:

- `rejects_blank_dates_with_validation_error`
- `shows_free_shipping_only_above_threshold`
- `blocks_publish_when_title_missing`

Avoid names like:

- `test_parser_handles_case_3`
- `calls_validate_then_save`
- `works_correctly`
