---
name: web-game-ui-dev
description: Design, implement, review, or debug browser game UI, including scenes, HUDs, layout, overlays, targeting, drag interactions, and event-driven animation. Use for DOM, Canvas, or mixed game frontends. Keep gameplay rules outside presentation. Not for pure gameplay-rule implementation or ordinary website design.
---

# Web Game UI Dev

Build game interfaces with explicit ownership of structure, geometry, interaction, and presentation time.
This skill is self-contained. It requires no particular framework, engine, package layout, or companion skill.

## Adapt to the target project

Read applicable project instructions and the affected module before editing.
Find its gameplay API, layout system, component library, asset registry, locale resources, and test commands.
Use existing equivalents of the concepts below. Do not install a new UI framework to match the examples.

Project contracts and explicit user scope take precedence over this skill's defaults.
Preserve the project's fixed-canvas or responsive layout policy. Do not impose a resolution, font, theme, or route structure.
If a required contract is absent, define the smallest local contract that supports the requested change.
For a small screen, ownership can remain explicit within one file. Separate packages and scene editors are optional.

## Read by task

Read only references relevant to the changed surface. All links resolve inside this skill.

| Task | Reference |
| --- | --- |
| Gameplay integration, routes, local sessions, persistence, dependency boundaries | [Architecture](references/architecture.md) |
| Scene structure, rectangles, layers, portals, styles, controls, Canvas coordinates | [Scene and layout](references/scene-layout.md) |
| Target selection, previews, drag, keyboard input, disabled states | [Interaction](references/interaction.md) |
| Playback, motion, sprites, audio, pause, route transitions, runtime cleanup | [Motion and playback](references/motion-playback.md) |
| Images, animation manifests, typography, player copy, localization | [Assets and content](references/assets-content.md) |
| Test selection, regression coverage, visual review, completion evidence | [Verification](references/verification.md) |

## Working method

1. State the affected surface, expected behavior, and verification criterion.
   Identify its semantic node, layer, coordinate space, layout owner, component, and motion track where applicable.
2. Trace authoritative state or committed events through projection, controller, scene, and renderer.
   Use gameplay-owned availability and previews for rule-dependent UI.
3. Change the owning declaration or API before its consumers.
   Keep one owner for each rectangle, conversion, style contract, layer boundary, asset binding, and clock.
4. Implement the smallest owning-region or component change.
   Reuse typed props, declared size policies, named slots, and existing semantic controls.
   Remove competing overrides made obsolete by this change. Preserve unrelated work.
5. Keep copy and assets in their registries. Keep transient interaction state near its owner.
   Do not add providers for static layout data or speculative theme support.
6. Verify the changed stable contract at its lowest useful layer.
   Report executed checks and unperformed visual review separately.

## Core boundaries

- Use this ownership model: physical shell → controller → scene → layer → region → component → primitive.
- State and semantic props flow down. User intent flows up through callbacks or commands.
- Gameplay owns legality, outcomes, and committed history. UI owns selection, presentation, and route orchestration.
- Do not reproduce damage, costs, target rules, rewards, or terminal conditions in rendering helpers.
- A scene or region owns placement. Components own their internal layout and visual states.
- Parent styles must not override child roots or descendants. Use the child's declared API.
- Global stacking belongs to the layer manager. Local component stacking cannot escape its mount.
- Keep board, world, logical UI, and physical viewport coordinates distinct. Name conversions between them.
- Use one presentation cursor for gameplay-facing feedback. Animation cannot commit or reorder gameplay events.
- Keep player-facing strings in locale/config resources, including errors, logs, accessible names, and fallbacks.
- Tests cover stable behavior and contracts. Production content, exact CSS pixels, and real animation waits are not default gates.

These boundaries describe responsibilities. They do not require a wrapper, class, provider, or file for every level.

## Debugging path

When a change has no visible effect, inspect the entire path before adding another override:

```text
authoritative candidates/state/events
→ pending interaction
→ projection and controller props
→ scene node and layer mount
→ layout geometry and coordinate conversion
→ renderer, CSS ownership, clipping, and stacking
→ locale/asset resolution
```

Check the active route and conditional branch. A correct component can still receive stale or incomplete data.
For timing problems, inspect cursor ownership, pause state, subscriptions, and generation cancellation.
For clipped text, inspect its own bounds and font metrics before assuming another element covers it.
For a missing target, inspect authoritative candidate generation before changing the highlight renderer.

## Delivery boundary

Do not start servers, browser automation, local page inspection, or runtime stacks unless the user requests that verification.
Existing authorization in the task remains valid. This skill does not require repeated confirmation.
Use static and focused automated checks within the authorized scope.
Describe unresolved visual behavior as unverified, even when type checking and component tests pass.
