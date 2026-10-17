# Scene, layout, and component ownership

Use this reference for structure, geometry, stacking, controls, styles, or Canvas sizing.

## Scene model

Represent meaningful surfaces with stable semantic node IDs and explicit parent, layer, and coordinate-space ownership.
Use the project's existing scene declarations. A small UI may use a typed local record instead of a manifest framework.
The renderer consumes this structure. JSX or DOM order alone must not decide global stacking.

| Level | Owns |
| --- | --- |
| Physical shell | Viewport fitting, safe area, clipping, output scale, physical background |
| Controller | Data loading, route lifecycle, orchestration, callbacks |
| Scene | Semantic tree, coordinate spaces, layer membership, named layout nodes |
| Global layer | Mounts, portal destinations, stacking, input boundary |
| Region | One rectangle, direct slots, clipping, slot placement |
| Component / prefab | Typed props, internal arrangement, assets, visual states, ephemeral UI state |
| Primitive | One reusable contract, such as button, icon, tooltip, or meter |

A prefab is a reusable presentational component with explicit slots and a size policy.
Lower levels must not inspect routes, ancestor selectors, or sibling DOM to choose their appearance.

## Layers and portals

Reuse the target layer order. If none exists, this is a useful starting order:

```text
background < world < hud < interaction < overlay < popup < modal < tooltip
```

Keep global sorting values in one manager. Keep component-local stacking within its mount.
Declare extensions once and reject duplicate names, unknown ordering references, and cycles.
Do not increase a tooltip's local z-index to repair a sibling stacking context.

Mount logical portals through the owning stage so they share its coordinate transform and lifetime.
Use a declared physical overlay for cross-space effects. Do not use an unscaled body portal as a layout shortcut.
Moving a mount between layers should preserve component identity, focus, and state when the surface remains the same.
Carry needed presentation classes and layout variables explicitly into portal content.

Layer names do not implement focus trapping, Escape handling, backdrop dismissal, or input blocking.
Declare those behaviors separately. Prevent input from reaching covered controls.
Keep intended controls above a cover usable, including pointer and keyboard paths.
Suppress hints from blocked content while allowing hints owned by the active dialog.
Keep page transitions from unintentionally moving dialogs, tooltips, or the physical background.

## One rectangle, one owner

Choose one placement mode for each node:

| Mode | Placement owner |
| --- | --- |
| `flow` | Parent Grid/Flex or equivalent layout |
| `anchor` | Parent-relative anchors and named offsets |
| `absolute` | Explicit rectangle in a declared coordinate space |
| `world` / `board` | Camera or board projection |
| `intrinsic` | Content with declared min/max constraints |

Edit the owner when geometry changes. Avoid compensating sibling margins, extra scaling wrappers, and second transforms.
Keep hover and motion transforms separate from stable layout geometry.
Use explicit grid rows or named areas when optional children occupy semantic slots.
Let a grid track stretch its child instead of adding ambiguous `height: 100%` sizing.
Keep automatic content layout inside the region so longer text cannot move unrelated controls.

## Display and coordinates

Preserve the declared display policy. Fixed-canvas rules below apply only to a fixed logical canvas.
A responsive game should keep its existing container rules and supported breakpoints.

For logical dimensions `Lw × Lh` and physical viewport `W × H`, centered uniform fitting uses:

```text
s = min(W / Lw, H / Lh)
offsetX = (W - Lw * s) / 2
offsetY = (H - Lh * s) / 2
logicalX = (clientX - viewportLeft - offsetX) / s
logicalY = (clientY - viewportTop - offsetY) / s
```

Require positive logical dimensions. Treat a zero-sized physical viewport as non-interactive instead of dividing by zero.
Reuse geometry helpers for pointers, tooltips, board hit testing, camera projection, and tutorial anchors.
For world coordinates, apply the inverse camera transform after the viewport conversion.

- Give each coordinate space one scale owner. Do not apply output scale twice.
- Keep viewport units in the physical shell, outside fixed logical descendants.
- Keep the logical canvas complete and centered. Use bounded panel scrolling for overflowing content.
- Where the world fills the output, calculate its visible extent separately from the UI safe area.
- Extra world visibility does not change map boundaries or gameplay coordinates.
- Resolve each endpoint of a cross-space drag or flight in its own space, then convert into one overlay space.
- Render each background and world surface once. Declare image fitting and empty-space fill explicitly.
- Do not use already-scaled client distances as translations inside a transformed board.

## Canvas

For logical drawing dimensions, backing-store size is logical size × output scale × device pixel ratio.
Normalize the drawing context back to the coordinate space used by its geometry helpers.
Refresh sizing on resize, fullscreen, zoom, and device-pixel-ratio changes.
If CSS dimensions already include output scale, do not multiply by that scale again.

Prefer Canvas for dense tiles and effects when it fits the existing renderer.
Keep semantic controls and accessible interaction in DOM or an explicit accessible Canvas adapter.
Hit-test with shared geometry, not one DOM node per tile.
Redraw on relevant presentation, asset, or size changes. Subscribe to frames only while visible animation needs them.
Pointer-only UI movement must not rebuild unchanged terrain or unrelated board presentation.

## Styles and control contracts

Parents own their root and direct slots. Children own their root, descendants, typography, borders, and states.
Use semantic props, named slots, and declared sizing APIs. Do not reach through descendants with parent selectors.
Keep stable styles in the owning stylesheet. Reserve inline styles for typed layout, runtime geometry, or asset bindings.
Treat a CSS custom property as an API only when its owner and meaning are documented.

Reuse existing control types such as primary command, selection, toggle, close, or content choice.
A control owns reset, typography, artwork, focus, hover, pressed, selected, and disabled appearance.
Screens supply semantic content, callbacks, state, and placement through declared inputs.
Use bounded variants within one stable contract. Use separate types when structure or interaction meaning differs.
Do not grow a universal control from unrelated page-specific props or color overrides.

Promote tokens when values must change together semantically, not because their current numbers match.
Add a theme provider only for an actual theme-switching requirement.

## Worked change: add a side panel

Declare the panel node, layer, coordinate space, and parent region.
Add its rectangle and direct slots in the owning layout.
Let the panel component own rows, icons, text clipping, and selection states.
Remove the superseded placement rule. Check that hiding an optional panel row does not move another region.
If an Inspector exists, keep its resolved rectangles and layer data aligned. Do not build one for this change alone.
