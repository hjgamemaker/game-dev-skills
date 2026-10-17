# Frontend and presentation tests

Read this reference for screens, controllers, browser state, localization, playback, and lifecycle.

## Stable workflows

Assert route, public command/request payload, state transition, enablement, dialog open/close, and session/storage shape.
Use synthetic states, view models, interaction plans, and event batches.
Keep gameplay-rule verification in the gameplay owner.
Mock browser/platform ports and external clients when the objective is frontend integration.
Do not mock normal rule execution if the test claims real command-to-UI integration.

Preferred workflow anchors:

1. A stable test ID or domain interaction anchor already exposed by the product.
2. Pathname, payload, version, public state, or side-effect boundary.
3. Role and accessible name when that name is a stable contract.
4. A short phrase for a narrow copy-specific test.

A semantic test anchor may identify a real control. Do not add duplicate DOM, hidden state, or production branches for tests.
Keep recurring selectors in existing shared test helpers when several tests use the same workflow.

Do not make normal workflows depend on full translated sentences, tooltips, CSS classes, wrapper counts, visual card order, or production catalogs.
If focus behavior is an accessibility requirement, test it explicitly. It is not an incidental DOM-detail assertion.

## Interaction and storage cases

Select cases for the affected contract:

- Enabled control submits the intended command. Disabled control submits nothing.
- Illegal visible target remains non-actionable.
- Drag release outside, on no candidate, or on an illegal candidate submits nothing.
- Legal release uses the final target, including movement immediately before release.
- Cancel, replacement, and session changes clear stale preview state.
- Route handoff consumes committed flow data after required playback completion.
- Cross-route presentation is consumed once by the destination.
- Loading, failure, and ready states preserve the intended route and command boundaries.
- Storage rejects unsupported versions and malformed payloads before hydration.
- Failed writes do not claim saved progress or restore stale state as if a new command succeeded.

Use minimal sessions. Do not click through a long battle to reach a storage or dialog assertion.

## Motion and playback

Inject a numeric cursor or fake clock. Use synthetic events and asset manifests.
Assert cues, descriptors, rendered-state boundaries, and lifecycle output at controlled times.
Do not wait for a real beam, flash, sprite frame, toast, or delayed overlay to appear and disappear.
Reserve real timing tests for explicit timer, scheduler, or runtime requirements that cannot be proved with controlled time.

Cover relevant invariants:

- Asset markers align presentation with committed events without changing gameplay.
- Pause, resume, seek, visibility, and cursor restore preserve the specified cue behavior.
- A continuous movement path retains one state-enter time across intermediate events.
- Idle sampling works without an active action queue.
- Unrelated later segments do not restart a prior transition.
- Cancellation and completion reveal the current stable target, not an obsolete endpoint.
- A stale generation cannot close a newer panel or resume replaced playback.
- Reduced motion preserves committed state and ordering under the product's policy.
- Inactive frame consumers unsubscribe. High-frequency sampling does not rerender the screen controller on every tick.

Do not bind these tests to production sprite frame counts or current animation durations.

## React lifecycle regressions

When a provider owns a disposable clock, playback, store, audio, or lifecycle runtime, test root effect replay.
Mount `StrictMode` at the `createRoot` boundary.
After replay, exercise a real subscription or scheduled update to prove the runtime remains usable.
Then unmount and assert cleanup. A wrapper-only test or a “dispose was called” assertion is insufficient.

For conditional rendering, rerender from missing data to ready data on the same mount.
This can expose changed Hook ordering that separate fresh mounts miss.
For measurement feedback, supply alternating fractional rectangles and prove updates settle within the geometry contract's tolerance.
Do not treat the DOM test environment as a real layout engine.

## Localization, assets, and visual evidence

Test resource structure, placeholder parity, reference coverage, required values, and documented fallbacks separately from workflow copy.
Test formatters with synthetic messages and locale inputs. Do not compare an entire live screen's wording.
For assets, validate references and manifest structure. Packaging checks may establish emitted URL resolution.
Neither check proves decoding or rendering on a particular device.

Pure tests can prove scale calculations, coordinate conversion, layer order, mount identity, and hit-test math.
Actual stacking, clipping, fonts, safe-area appearance, and physical pointer behavior require authorized browser/manual evidence.
For a text-clipping bug, inspect its own bounds, overflow, line height, and font metrics before assigning a cause.
For overlay changes, visual review must include intended controls above the cover, not only the mask rectangle.
