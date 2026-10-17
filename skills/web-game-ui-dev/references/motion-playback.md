# Motion, playback, audio, and lifecycle

Use this reference for gameplay feedback, route transitions, sprites, Canvas effects, audio cues, and runtime cleanup.

## Event-driven presentation

Use this directional flow:

```text
committed events + before/after state
→ presentation cues
→ semantic storyboard descriptors
→ renderer and audio output
```

Gameplay events are the only history. Presentation may split an event into windup, release, flight, impact, damage, recovery, or movement segments.
It may delay visibility of a committed event. It must not create, remove, reorder, or change gameplay events.
Interpret raw events once. Downstream renderers consume semantic presentation kinds and descriptors, not raw event names or full gameplay state.
Keep storyboard planning pure. Supply inputs explicitly instead of reading the DOM, clock, or mutable component state.
Build playback frames once per submitted batch. Reuse immutable state for timing-only steps instead of cloning the full state repeatedly.
When performance is affected, measure command execution and playback planning separately.

## One presentation clock

Use one route-scoped or surface-scoped presentation cursor for gameplay-facing timing.
The cursor drives playback state commits, sprites, DOM/WAAPI motion, Canvas effects, projectiles, highlights, and sound cues.
It supports pause, resume, seek, visibility handling, and cursor restore according to the project contract.

React controllers observe semantic boundaries such as action, segment, rendered state, blocking, and completion.
Do not expose a page-level elapsed-time counter or use independent sprite intervals, effect timers, or wall-clock timestamps.
Frame subscriptions are demand-driven. Stop them when no visible high-frequency descriptor needs drawing.
Hidden surfaces pause logical presentation time if the project contract requires it, so resume does not skip cues.

Asset markers align a cue inside an already committed segment. They never trigger gameplay settlement.
If a cue can be crossed between frames, attach it to a durable semantic segment rather than a zero-duration step.
Asset preparation may delay presentation until required images decode. It cannot delay authoritative commitment or persistence.
Cancel stale preparation by generation after route exit, undo, or batch replacement. Failed assets must not leave playback blocked.

## Presence lifecycle

Presence-controlled surfaces follow:

```text
absent → entering → entered → exiting → absent
```

Entering and exiting surfaces are inert unless they explicitly own a blocking contract.
Keep an exiting surface mounted until completion or cancellation. Use a generation or request identity so stale completions cannot remove a newer surface.
Keep the outgoing route mounted during its exit when the route transition requires it.
Do not let CSS completion, a cleanup timeout, or an unmounted component become the route authority.

Motion descriptors own interpolation and timing semantics. Stable layout remains owned by the scene or component when motion completes or is disabled.
Reduced motion may collapse visual feedback when the target product allows it. It must never change gameplay state, event order, or settlement.
Give each animated property one stable target owner. Avoid competing inline styles, imperative writes, and animation cleanup that restores stale values.
Trigger motion from its semantic segment or cue. Unrelated later segments must not restart it because snapshots still differ.
Wait for the relevant playback completion before showing a blocking outcome panel or executing its route handoff.

## Character and movement presentation

Use a frontend character state machine with at least `idle`, `move`, `attack`, `hit`, and `death`.
State owns the clip, state-enter time, and movement or attack payload. It derives from the cursor and committed events.
`idle` remains active when no playback queue exists and samples the shared clock.
Represent a multi-step path as one continuous movement state. Intermediate events update committed history without restarting easing or the movement clip.
Render units from destination layout rectangles. Calculate travel vectors from unscaled board-local geometry, not transformed client distances.

Attack delivery exposes a stable semantic kind such as melee, instant, projectile, beam, or area.
Resource paths, frame numbers, and presentation durations remain frontend-owned.

## Audio

Map committed semantic cues to music and sound through the same presentation cursor.
The gameplay package never imports media, chooses files, or schedules audio.
Pause and route lifecycle control active media through the shared runtime. Autoplay failures must not alter gameplay or route state.
Do not use button clicks to stand in for a gameplay result.
Define cue delivery for replay, seek, undo, restore, and route handoff. Prevent duplicate delivery within one playback pass.
Replay may intentionally play cues again. Seeking or restoring must not accidentally replay already-consumed cues.

## Runtime cleanup

Provider-owned clocks, stores, playback, and audio must survive root `StrictMode` effect replay and must clean up on a real unmount.
Test the runtime at the root `createRoot` boundary when this contract matters. Avoid irreversible disposal during a development-only effect replay.

## Worked change: add a battle effect

Add a semantic cue in the event interpreter, then add a pure storyboard descriptor.
Let a renderer sample that descriptor from the shared cursor.
Keep assets, DOM geometry, Canvas drawing, and audio mapping in presentation adapters.
Test cue identity and cursor boundaries with synthetic events and a fake clock.
