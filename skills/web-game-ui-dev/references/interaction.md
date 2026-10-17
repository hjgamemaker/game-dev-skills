# Interaction and gameplay projection

Use this reference for cards, skills, items, targeting, drag, keyboard input, previews, and disabled controls.

## Authority

The gameplay package owns action candidates, source piles, legality, previews, effects, and pending-action stages.
The frontend consumes an interaction plan or equivalent public API. It must not inspect effect definitions, ability inputs, range fields, costs, or IDs to invent candidates.

The frontend may own pointer capture, drag threshold, focus, selection history, hover, open dialogs, and presentation state.
It submits an intent only through the gameplay command path.

Visible-but-illegal choices can remain visible and explanatory. They remain non-actionable.
Do not hide invalid candidates when the product needs to teach the rule. Do not make a click or release bypass the legal-action check.

## Action state machine

Use one explicit machine for drag actions:

```text
idle
→ pressed
→ dragging-outside
→ dragging-inside-none
→ dragging-inside-illegal
→ dragging-inside-legal
```

Only `dragging-inside-legal` may submit on `pointerup`.
Release outside the board, pointer cancel, Escape, and action replacement return to `idle` without committing gameplay.
Leaving the board clears target preview and action-ready presentation without producing a rule error.

Entering or changing a tile resolves the current tile through the gameplay preview/legal API.
Pointer movement inside the same tile may reuse its plan and preview.
Release hit-tests the final tile. Reuse the preview only when the final tile is unchanged.
An illegal or non-candidate release may show the gameplay-owned error, then discards the drag action.

Keep prompts, highlights, projected effects, and release affordances as projections of the machine state.
Do not keep separate booleans that can disagree about board membership and submit eligibility.

## Selection and back

Activate the current selection step immediately. Static and deferred input should use the same selection surface when their public plan says so.
Back is step-local: restore the prior candidate list, then the earlier input step, then cancel.
Do not rerun effects to recreate a previous selection.
Keep working previews isolated until final confirmation. Only the command path commits state.

Use the same contract for cards, active skills, and usable items. Passive or unavailable actions do not enter `pressed`.
Keyboard navigation and pointer hit testing should resolve through the same plan and geometry helpers.
Expose one focusable board interaction surface when Canvas owns the board. Enter and Space activate the current legal target.

## Projected values

Render projected unit, hero, resource, impact, facing, and affected-target values directly from the plan.
Use a documented fallback only when the plan explicitly omits that field.
Do not recalculate self-damage, retaliation, healing, occupancy, range, or target legality in the UI.

If the authoritative API cannot provide a value the UI needs, report the missing contract or change the owning projection within scope.

## Modal and overlay input

An overlay's layer does not automatically implement input behavior.
Declare whether a surface blocks the background, traps focus, closes on Escape, dismisses on backdrop, or allows active-layer hints.
Keep intended controls above a modal or cover in the declared higher layer and verify pointer interaction, not only mask geometry.
Do not let hidden or exiting surfaces receive input.
