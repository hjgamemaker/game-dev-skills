# Assets, typography, and player content

Use this reference for art, sprite sheets, fonts, localization, aria text, and generated runtime assets.

## Asset ownership

Keep runtime assets in the game frontend or its established asset package.
The gameplay package owns stable content identities and rule facts. It must not own image paths, font files, frame indexes, or presentation timers.

Use semantic asset IDs mapped to module-imported URLs or typed asset records.
Components consume resolved records or semantic slots. Do not scatter path assembly or host-specific path detection through components.
Use the project's declared loading policy for remote assets. Bundle required assets when offline operation is a product requirement.
Do not fall back to an internal ID as visible text.
Keep source frames separate from deterministic generated strips, atlases, and manifests when regeneration is needed.
Do not hand-maintain large generated import lists.

Prepare only assets needed by the current route or playback batch. Asset failure uses a structural placeholder and cannot block gameplay, settlement, or restore.

## Pixel and sprite rules

- Disable Canvas image smoothing explicitly for pixel art. CSS `image-rendering` is not enough for Canvas.
- Keep aspect ratio for borders and pixel geometry. Use fixed logical height, proportional sizing, or nine-slice behavior.
- Use one anchor per character across clips. Align it to a context-owned ground or portrait point.
- Do not trim, recenter, or independently scale each frame. Transparent frame boxes can otherwise jitter between clips.
- Keep action vocabulary semantic, for example `idle`, `run`, `attack`, `damaged`, and `death`.
- Store frame count, dimensions, anchor, scale, fps or frame durations, loop policy, and semantic markers in the manifest.
- Markers align presentation cues. They never commit gameplay.
- Generate runtime assets from source changes and validate source, strip, and manifest together.

Use an asset-specific manifest when a sprite set needs non-default clips or facing. Keep the renderer independent from source directory layout.
Already delivered grid sheets may be used directly with a manifest. Do not split and regenerate them without a concrete need.
Validate unique asset keys, referenced files, sheet dimensions, frame bounds, and required markers.
Check asset URL resolution in each supported build when asset integration changes. A development URL does not prove packaged resolution.

## Typography

Use the target product's declared typography roles and scale. Components should consume semantic roles such as body, label, title, value, and outcome.
Do not add arbitrary intermediate font sizes, synthetic weights, utility classes, or page-level overrides.
Keep controls responsible for their own typography. Screens choose placement and content.
Test glyph coverage, long text, line breaking, and overflow through localization or visual review where needed.

## Localization

Player-facing strings include buttons, titles, errors, tooltips, logs, placeholders, metadata, aria labels, and helper-generated copy.
Put them in locale/config resources. Use semantic IDs and complete message templates with named placeholders.
Keep locale resource structures and placeholder sets aligned. Use the shared formatter for plural, select, number, currency, and date rules.
Use one active locale authority. Preserve the project's source locale, published locale set, and fallback policy.
Do not publish an incomplete locale by silently spreading source-language strings into its resources.
Do not branch on translated words in formatters or assemble grammar from fragments.
Do not make workflow tests depend on full sentences that change often. Test resource shape, placeholder parity, and stable semantic behavior.

## Accessible content

Provide semantic labels and keyboard behavior for interactive controls, Canvas actions, dialogs, and dynamic status.
Keep accessibility text in the same content ownership path as visible copy.
Do not expose internal IDs, file names, or debug labels to players.

## Worked change: add an animated unit

Define semantic clips, an anchor, display scale, markers, and loading policy before importing frames.
Register the asset and resolve it by content identity in the frontend.
Map committed presentation cues to the semantic clip in the character state machine.
Sample frames from the shared cursor. Do not add an interval in the unit component.
