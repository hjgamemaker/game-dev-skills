# Stable test design

Read this reference for fixture selection, assertions, test doubles, or failing-test repair.

## Mechanism versus content

Keep these purposes separate:

| Purpose | Input | Assertions |
| --- | --- | --- |
| Mechanism behavior | Minimal synthetic config, state, and commands | Execution, legality, events, bounds, errors |
| Content loading | Actual resources | Parsing, schema, normalization, required fields |
| Content integrity | Actual catalogs and registries | Unique IDs, valid references, supported action/effect vocabulary |
| Localization integrity | Locale resources and semantic references | Key and placeholder parity, required values, configured fallback |
| Stable product exception | Explicitly documented resource contract | The specific promised value or identity only |

A catalog scan does not prove that effects execute correctly.
A mechanism test should survive changes to production names, IDs, card counts, default decks, costs, and balance.
Do not freeze a current constant merely because it is exported.
When configuration changes are the feature, assert the declared requirement narrowly instead of snapshotting the catalog.

## Fixtures and independent expectations

Use existing builders when they expose the inputs the test needs.
Choose values that make incorrect behavior distinguishable from correct behavior.
An assertion such as “health increased” is too weak if the contract requires a specific configured amount or cap.

Example fixture:

```text
Rule: healing adds the configured amount, capped at maximum health.
Fixture A: health 4, maximum 10, heal amount 3 → expected health 7.
Fixture B: health 9, maximum 10, heal amount 3 → expected health 10.
Observe the documented event payload as well when event delivery is part of the contract.
```

These exact expectations belong to synthetic input, not current production balance.
Do not calculate the expected result by calling the helper being tested.
Do not copy a complex algorithm into the test. Use a small case with a transparent expected result.
Deterministic equality alone is insufficient: a no-op can produce the same result twice.

Prefer injecting config and registries through supported APIs.
If a legacy test must temporarily mutate shared config, restore it in `finally` or guaranteed cleanup.
Do not run conflicting catalog mutations concurrently. Prefer test-local registries over global mutation.
Use test-local clocks, random sources, and stores. Restore spies, timers, subscriptions, and globals after each test.

## Doubles and the claimed boundary

Use a fake for a controlled clock, media API, storage port, or external client when that dependency is outside the objective.
Keep real rule execution in gameplay tests and real collaboration in integration tests that claim that boundary.
A UI projection unit test may accept a synthetic plan. It proves plan consumption, not gameplay legality.
A persistence integration test needs the actual persistence behavior. A repository mock cannot prove transactions.

For public contracts, validate consumer mapping and producer output where both are in scope.
Checking a fixture against the same locally invented type proves neither side implements the contract.
Use stable state/output assertions over incidental private method calls.
Call count and payload assertions are useful when the public side-effect boundary is the contract.

## Repairing brittle or flaky tests

Classify the failure before editing:

| Failure | Response |
| --- | --- |
| Valid contract violated | Preserve the assertion. Fix implementation only within authorized scope. |
| Production content or copy changed | Replace accidental content coupling with a synthetic fixture or semantic assertion. |
| Schema, missing reference, unsupported vocabulary | Keep the integrity failure. Diagnose the content or loader. |
| Wall-clock or scheduling race | Use controlled time and explicit lifecycle boundaries. |
| Shared mutation or leaked subscription | Isolate fixtures and restore resources. |
| Import, stale declaration, missing dependency, or setup failure | Repair setup within scope and report that behavior did not run. |
| Deliberate public contract change | Update the documented contract and affected tests together. |

Do not fix flakes by increasing arbitrary sleeps, adding retries, deleting assertions, or skipping the failing test.
Do not assert only that an operation succeeds when its output can be wrong.
Keep the smallest regression that distinguishes the fault from the intended behavior.
Do not rebuild unrelated test infrastructure for a local failure.

## Review questions

- Can the test fail when the named behavior is wrong?
- Does the expectation come from a contract rather than copied implementation output?
- Does the test exercise the mechanism it claims to prove?
- Can routine content, copy, layout, or balance changes leave the mechanism test intact?
- Does cleanup run even when an assertion or setup step fails?
- Would the result remain meaningful in isolation and in the supported suite order?
