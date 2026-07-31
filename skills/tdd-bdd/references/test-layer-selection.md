# Test Layer Selection

Use this reference when deciding where a TDD or BDD scenario should live.

Core rule: choose the lowest layer that can fail for the intended reason.

## Fast Decision Guide

Choose a unit test when the behavior is:

- Pure branching logic
- Value transformation
- Parsing and validation with no real I/O
- Small policy rules

Choose an integration test when the behavior depends on:

- Database reads or writes
- Filesystem interactions
- Queue, cache, or message broker behavior
- Multiple components collaborating inside one service

Choose a contract test when the behavior depends on:

- HTTP request/response shape
- Serialized event payload shape
- SDK adapters or provider boundaries
- Compatibility with a specific external schema

Choose an end-to-end test when the behavior only matters if proven across:

- UI plus backend
- Multiple services or processes
- Authentication and routing layers together
- A critical operator journey where wiring is the risk

## Selection Questions

Ask:

1. What exact regression am I trying to catch?
2. Which layer would fail if that regression returned?
3. What is the cheapest realistic place to assert it?
4. Which dependencies should stay real for the failure to mean anything?

## Practical Defaults

- Start at unit level for pure rules.
- Move to integration when the rule spans persistence or component collaboration.
- Add contract tests at external boundaries that can drift independently.
- Use end-to-end sparingly and intentionally.

## Mocking Rule

Mock unstable or expensive boundaries. Keep the boundary under test real.

Examples:

- Parsing rule: no mocks, plain unit test
- Repository write behavior: real repository plus ephemeral database if practical
- External API adapter: real serializer, mocked network
- UI purchase flow: real UI and app wiring, mocked payment provider if the provider is not the subject

## Warning Signs

You chose too low a layer if:

- The test passes while the user-facing bug still exists
- The assertions mostly restate private helper behavior
- You need to mock half the call graph to get through setup

You chose too high a layer if:

- The failure signal is slow or noisy
- The test breaks for unrelated wiring changes
- A narrower test could prove the same rule
