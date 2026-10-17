# Architecture and integration

Use this reference for controllers, routes, gameplay integration, sessions, or persistence.
The responsibilities below apply even when the project keeps them in one package.

## Responsibility map

| Owner | Responsibility | Excludes |
| --- | --- | --- |
| Gameplay domain | State transitions, legality, previews, outcomes, ordered events | DOM, audio, asset paths, presentation timers |
| Game frontend | Projection, screens, local sessions, pending input, playback, copy, assets | Reimplementation of rules or server settlement |
| Reusable frontend runtime | Geometry, layer mounts, clocks, lifecycle, browser ports | Game routes, content identities, rule semantics |
| Platform host | Mounting, physical window, native capabilities, application integration | Game component internals |

Use public gameplay and platform APIs. Avoid imports from host internals, server internals, or unrelated game implementations.
Browser code must not depend on Node-only or native SDK modules.
Share a component when consumers need the same contract, not merely similar screenshots.
Local play does not require accounts, gateways, or backend services.

## Controller and projection

Controllers connect route lifecycle, sessions, pending input, and playback to presentational components.
Split a controller when these responsibilities interfere with independent changes.
Do not add a second authoritative game state to simplify rendering.

- Read committed snapshots, legal-action APIs, interaction plans, and previews from the gameplay owner.
- Submit intents through the public command/session API. Do not mutate a snapshot in a click handler.
- Keep command availability distinct from frontend blocking, such as an open dialog or active transition.
- Keep preview state distinct from committed state. Clear previews when their source action or session changes.
- Preserve error categories through UI adapters. Resolve their visible messages through localization.
- In online flows, use the project's authoritative response or snapshot path. Notifications alone do not establish settlement.

If a UI feature needs a missing gameplay projection, extend that public projection within the authorized scope.
If the task permits only UI changes, report the missing contract instead of approximating the rule in the frontend.

## State lifecycles

Distinguish these lifecycles:

- authoritative gameplay state and event history;
- local selection, focus, hover, and dialog state;
- tutorial progress and product preferences;
- playback cursor, rendered snapshot, and pending route handoff;
- persisted session document.

Keep ephemeral state at the smallest owning component. Lift it only for coordination or persistence.
When route choice depends on game progression, consume committed progression data from gameplay.
Keep the temporary wait for animation and route transfer in the frontend.

Cross-route playback is staged once and consumed once by the destination.
Do not play destination events invisibly on the outgoing screen.
A fresh restore without pending playback should render the restored committed snapshot directly.
Do not infer missing events by comparing visible unit lists.

## Persistence

- Version keys and payloads. Validate parsed shape and version before hydration.
- Use the project's storage abstraction. Handle unavailable storage and failed writes explicitly.
- Separate compact persisted data from rich runtime views when their contents differ.
- Store stable content identities and dynamic state. Resolve static catalogs during hydration through the gameplay API.
- Reject, migrate, or clear incompatible data deliberately. Do not fabricate missing authoritative fields.
- Keep transient animations and route transfers out of saves unless resumable presentation is an explicit product feature.
- Keep tutorial progress and preferences separate when their lifecycle differs from a game session.

A successful in-memory transition does not prove that storage succeeded.
Check write errors before diagnosing a route that returns to an older saved state.

## Platform integration

Show native-only controls only when the host exposes their capabilities.
Use narrow injected adapters for window mode, file access, or platform services.
Reuse the game frontend across supported hosts without duplicating assets or rule logic.
When changing exported types in a compiled workspace, rebuild producer declarations before diagnosing consumer type errors.
