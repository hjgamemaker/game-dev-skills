---
name: game-test-dev
description: Write, review, or repair tests for deterministic game rules, game frontends, API contracts, and supporting services. Use for regression coverage, synthetic fixtures, preview/execution consistency, replay, UI lifecycle, idempotency, and brittle or flaky tests. Choose the lowest stable test layer and keep production content out of mechanism fixtures.
---

# Game Test Dev

Prove stable behavior at the lowest test layer that owns the risk.
This skill is self-contained. It requires no particular engine, framework, runner, repository layout, or companion skill.
Service and browser guidance applies only when the target project uses those boundaries.

## Before writing

Read applicable project instructions, the public contract, the implementation, and nearby tests.
Find the existing runner, fixture builders, test commands, and environment policy.
Preserve the user's scope, including tests-only work and restrictions on running tests.
Project contracts take precedence over these defaults. Do not add a new test framework to match an example.

Write a short test objective:

```text
Trigger: the action or failure that exercises the risk.
Contract: the observable result and required side effects or lack of side effects.
Owner: the module that implements the mechanism.
Evidence: the lowest test layer that can distinguish correct from incorrect behavior.
```

Find an expectation independent from the implementation under test.
Do not turn today's output into the expected result without checking the contract.

## Choose the test layer

| Changed risk | Preferred layer | Evidence |
| --- | --- | --- |
| Pure rules, legality, projection, geometry, formatter, state machine | Unit | Output, state transition, events, rejection safety |
| Controller, session, storage, adapter, application use case | Integration | Real collaboration across the affected boundary |
| Public request, response, notification, error, or serialized data | Contract | Producer/consumer agreement on supported shape and semantics |
| Small critical journey through real runtime components | E2E | Player outcome, route reachability, authoritative state |
| Appearance, physical layout, stacking, glyphs, motion feel | Explicit visual review | Rendered behavior in the required environment |

Do not repeat every rule at each layer. Add a higher layer only for risk the lower layer cannot establish.

## Read by task

| Task | Reference |
| --- | --- |
| Fixture design, assertions, doubles, test repair, content validation | [Stable test design](references/stable-test-design.md) |
| Rules, actions, effects, targets, RNG, preview, events, replay | [Gameplay tests](references/gameplay-tests.md) |
| UI workflows, storage, localization, playback, React lifecycle | [Frontend tests](references/frontend-tests.md) |
| APIs, services, idempotency, persistence, runtime setup, E2E | [Integration and E2E](references/integration-e2e.md) |

Read the relevant reference before changing that test layer. Do not load unrelated service or browser guidance for pure rules.

## Working method

1. Identify the contract and owning mechanism. Select the smallest fixture that reaches the failure or new behavior.
2. Exercise the real public path for that mechanism. Replace only dependencies outside the test's claimed boundary.
3. Cover the successful transition and relevant rejection or boundary case.
   Assert events or side effects when final state alone could hide an incorrect sequence.
4. For a regression, demonstrate the failure before the fix when authorized and practical.
   Report when this was not observed. Do not claim a reproduced failure from an unrun test.
5. Implement or repair tests within scope. Change production behavior only when the task authorizes it.
   Do not weaken a valid assertion to make an implementation defect pass.
6. Run the focused checks allowed by the task. Inspect failures and logs, not only the command exit code.
   Complete required project checks. Broaden testing when changed boundaries or unresolved failures justify it.
7. Report what the evidence proves and what remains unverified.

## Stable boundaries

- Test mechanisms, protocols, state transitions, and declared product invariants.
- Use minimal synthetic config for mechanism tests. Production catalogs are not default fixtures.
- Exported defaults and constants are not automatically stable contracts.
- Exact expected numbers are appropriate for controlled fixture inputs. Do not lock current production balance.
- Keep production-content validation separate from mechanism behavior coverage.
- Do not mock the mechanism under test and then claim its correctness.
- Do not add test-only production branches, duplicate state, bypasses, or diagnostic DOM to satisfy assertions.
- Control time, seeds, storage, and external dependencies. Restore mutable fixtures and runtime resources after each test.
- Keep default tests isolated from manually started services. External infrastructure belongs to explicit suites.
- Keep tests portable. Resolve paths from the module, workspace, or runner rather than a developer's absolute path.
- An exact production value, string, or order needs a documented contract and a narrow assertion.

## Execution and reporting

Inspect commands before running them when they can start services or reset data.
Do not start a server, browser, local page inspection, or runtime stack without an explicit request for that verification.
Existing authorization remains valid. Do not ask again when the requested verification already includes the action.
Use a dedicated test environment for database mutations and resets. Never default to development or production data.

Record cases, results, known gaps, and commands in the project's existing QA record when one is used.
Do not invent a milestone directory or require a new reporting system.
Separate focused checks, broader checks, and browser/manual review in the completion report.
Distinguish failed tests, collection/setup failures, and checks that were not run.
Do not claim gameplay correctness from UI mocks, visual correctness from DOM tests, or integration correctness from type checking.
