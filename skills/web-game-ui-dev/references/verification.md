# Verification and review

Use this reference when choosing tests or reporting completion.

## Lowest stable layer

| Risk | Preferred check | Assert |
| --- | --- | --- |
| Pure projection, geometry, formatter, or interaction machine | Unit | Input/output, transition, candidate shape |
| Controller, route, storage, pending input, playback reducer | Frontend integration | Route, payload, state, storage shape, enablement, open/close |
| Gateway or host boundary | Contract | DTO, event envelope, error category, mapping |
| Critical player journey | E2E | Small number of stable route and outcome anchors |
| CSS geometry, responsive output, stacking, art, motion feel | Explicit browser/manual review | Visual result at declared project viewports |

Do not move a pure mechanism test to E2E because the result is visible on a page.
Do not claim that unit, type, or integration checks prove fixed-canvas geometry, stacking, or visual quality.

## Stable assertions

Prefer route reachability, request payloads and call counts, state transitions, command enablement, dialog lifecycle, pending-action submit/no-submit paths, storage/session/replay shape, interaction-plan projection, and presentation descriptors at synthetic cursor positions.
Use semantic selectors in this order: stable `data-testid`, pathname or protocol field, stable role and accessible name, then a short phrase for a focused copy test.
Add test IDs only for stable contract fields. Do not create test-only branches or state.

## Avoid brittle assertions

Do not make routine gates depend on full copy, current tooltips, CSS class names, descendant wrappers, fixed pixels, screenshot output, production IDs, catalog counts, balance, default order, real timers, animation duration, or a transient frame.
Use synthetic gameplay events, plans, states, manifests, and fake clocks.
If an exact number, string, or order is a stable product or protocol contract, state that intent in the test name or comment.

## Required interaction checks

For a target-selection or drag change, cover legal release and no-submission paths for outside, none, and illegal states.
For a preview change, cover the authoritative candidate and the projected value. Do not duplicate the rule in the fixture.
For a layer or dialog change, cover opening, closing, blocking input, and intended controls above the mask.
For playback, cover cue mapping, cursor boundaries, pause/resume, cancellation, and stale-generation protection as applicable.
For storage, cover version and shape validation and the failure path. For provider runtimes, cover root `StrictMode` replay and true unmount cleanup.

## Conditional rendering failures

Keep React Hooks before conditional returns. Cover empty-to-ready rerenders when changing conditional data branches.
For measured layout written back into state, compare in one coordinate space with a suitable tolerance.
Alternating subpixel measurements must not cause an unbounded render-measure loop.
Test these mechanisms with controlled inputs. Actual glyph clipping and computed CSS geometry still need visual evidence.

## Visual review

Browser or manual review is separate and requires explicit user authorization under the target project policy.
When authorized, review at the project's declared viewports and inspect safe-area geometry, backgrounds, world visibility, text overflow, focus, stacking, pointer hit testing, sprite anchors, and motion lifecycle.
Do not start a server or browser for a UI task unless the user requests that verification.

## Report

State the changed contract, focused checks run, broader checks run, and browser/manual review status.
Call out unresolved dependencies such as missing gameplay projections, stale generated declarations, unavailable assets, or unverified physical layout.
