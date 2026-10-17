# Deterministic gameplay tests

Read this reference for command handling, legality, effects, targeting, previews, events, and replay.

## Exercise the public rule path

Create a minimal state and config with explicit seed or injected randomness.
Submit a public command through the owning engine/session API.
Keep DOM, services, storage, environment variables, and presentation clocks outside pure rule tests.
Test isolated helpers where useful, but do not let helper-only coverage replace command registration and execution coverage.

For a new primitive, select applicable cases from this matrix:

| Contract | Evidence |
| --- | --- |
| Authoritative execution | Expected state transition using controlled inputs |
| Legality and legal-action discovery | Legal option present, illegal option rejected or marked as documented |
| Preview | Public target/value semantics agree with execution |
| Events | Correct payload, ownership, and relative order |
| Config vocabulary | Parser/registry recognizes the primitive and rejects unsupported forms |
| Failure protection | Stable error category and documented rejection safety |
| Replay/restore | Equivalent committed state and event history at the same cursor |

Do not mechanically add every case for every change. Cover the changed risk and exposed boundaries.
For a new rule family, final-state-only coverage is insufficient.

## Mechanism families

Use synthetic fixtures to distinguish:

- effect execution, accumulation, caps, and resolution order;
- trigger host, owner, activation time, and non-triggering cases;
- target shape, range boundary, filters, occupancy, and empty candidates;
- seeded selection and explicit random branches;
- command errors and guard behavior, including recursive resolution limits where implemented;
- action registration, pending input, and final commitment.

Use a non-triggering control when testing event-driven rules.
For an area effect, include an inside target, a boundary target, and an excluded target where relevant.
Keep exact coordinates and quantities fixture-local.

## Preview consistency

Preview and execute from equivalent initial states using the same controlled configuration and randomness contract.
Compare public targets, affected entities, and projected values with the committed result.
Check that preview leaves authoritative state and RNG unchanged when the preview contract is read-only.
Allow explicitly documented masking of hidden information. Do not require a preview to reveal secret data to match execution.

If UI candidates are missing, prove the authoritative candidate domain before testing the renderer.
A frontend highlight test cannot establish that gameplay provided the correct options.

## Errors and rejection

Use the documented category for malformed input, invalid target, unavailable action, missing input, or insufficient resources.
Do not accept a generic error when consumers depend on the distinction.
Verify the documented atomicity boundary. For atomic rejection, assert unchanged state, no committed events, and no consumed resources.
Keep provisional staged state distinct from final committed state when the engine supports it.
An orchestration failure must not masquerade as a player rule violation.

## Determinism, persistence, and replay

Run equivalent independent initial states with the same inputs and seed.
Assert a known meaningful transition as well as equal results and event ordering.
Do not require different seeds to always yield different outcomes.
Cover relevant random branches with controlled seeds or an injected source.

Where supported, test replay, undo/redo, and restore through public APIs.
Check state and history at the same cursor. Do not drive gameplay by waiting for presentation.
Keep compact save documents separate from hydrated runtime snapshots when their contracts differ.
Verify hydration restores required static definitions without copying complete catalogs into each save.

For state-selected configuration, create a fixture whose selected map or rules differ from the default.
Prove later actions and previews use the committed selection instead of re-resolving defaults.

## Performance regressions

Add performance coverage only for a changed or demonstrated performance risk.
Measure committed command execution and playback planning separately when both participate in the slow path.
Do not assume a fast preview proves a fast submission path.
Use the project's benchmark approach and representative synthetic workloads.
Avoid machine-sensitive wall-clock thresholds without a defined benchmark environment and budget.
