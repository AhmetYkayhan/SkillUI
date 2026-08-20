---
name: game-ui-godot
description: Use when implementing or fixing UI in a Godot project - Control and Container layout, anchors and offsets, Theme and StyleBox styling, CanvasLayer ordering, focus and controller navigation, viewport and stretch behavior, or UI scene structure in .tscn and .gd files. Not for Unity, Unreal, non-Godot engines, or game logic and other non-visual Godot changes with no UI impact.
---

# Godot UI Implementation

Godot specialization of `game-ui-design`. Core and generic game rules still apply and are not repeated here. This file answers one question: which Godot systems express a game UI decision correctly?

## Applies when

The project is Godot — `project.godot`, `.tscn`/`.tres`/`.gd` files, or an explicit statement. A game project alone is not evidence of Godot.

**Version safety.** Never assume the engine version. Node class names, property names, and behavior differ between major versions. Before using any exact API, confirm it from the project's own scenes and scripts, from installed engine documentation or tooling, or from official docs if reachable. When unsure, express the intent and follow the pattern the codebase already uses rather than naming a property from memory.

Insert into the priority ladder: existing Godot project conventions and scene reuse, the correct layout primitive, focus reliability, theme consistency, and scene integrity all outrank visual polish.

## 1. Godot-native first

```
existing project UI pattern → existing Control/Container structure → existing Theme →
existing reusable scene → Godot-native primitive → custom layout or logic
```

Do not port web or CSS layout thinking into Godot, and do not treat manual coordinate math as the default. Determine whether the project builds UI scene-driven, code-driven, or hybrid, and follow it. Converting one architecture to the other is a separate task, never a side effect.

Learn scene and node naming, script placement, theme organization, signal style, autoload usage, and folder layout from the project. Do not invent conventions, and do not assume any particular directory layout.

## 2. Control nodes

Screen-space UI is built from `Control`-derived nodes. Do not build menus or HUD from world-space nodes and manual world coordinates unless the UI genuinely lives in the world. If the project deliberately uses a different abstraction, keep it.

## 3. Layout

Anchors and containers solve different problems: anchors express placement relative to the parent or screen; containers arrange siblings. Do not force everything through one of them.

- Use a container or the project's layout primitive for grouped arrangement — vertical, horizontal, grid, margin, centered, scrolling — instead of reproducing it with offsets.
- Use the **shallowest layout tree that expresses the relationship correctly**. A chain of margin-and-box wrappers nested five deep is a defect, not a structure.
- Offsets are for local adjustment and intentional fixed distances. They are not a resolution strategy — never build a system of manual offsets to survive different resolutions.
- No hardcoded screen coordinates for HUD or menu layout. Absolute placement is for genuine overlay or specialized visual composition only.
- Set a minimum size only where the control truly has one. Do not use it to force a container into a shape.
- Give each child the fill, expand, or shrink behavior its role implies — a content-sized label, a flexible spacer, a stretching panel, a fixed icon, and scroll content do not share one setting.
- Do not stretch every control to fill its parent. A full-rect anchor preset is not "responsive" by itself; it interacts with offsets, minimum size, and container behavior, and it is wrong for content-sized elements.
- Center a control only when centering is the layout intent, and use the layout primitive rather than half-width math. Centering an entire menu is not automatically correct.
- Prefer container separation, theme constants, or margin wrappers over empty nodes used as spacers. If the project uses spacer nodes, do not multiply them.

## 4. Theme and styling

Where the project has a Theme or shared styling system, use it before adding node-level overrides.

```
existing Theme → existing variation or style class → node-level override only when justified
```

- A repeated per-node override — font, size, color, panel style, button style, focus style, spacing constant — is a signal to find the theme entry, not to keep overriding.
- A node-level override is reasonable for a true one-off, a prototype, or a deliberate semantic exception.
- Introduce a theme variation for a real, reused specialization. Not for every button that looks slightly different, and do not create a whole new Theme for one component.
- Reuse shared style resources rather than embedding a duplicate on each node — but know that mutating a shared resource at runtime changes every user of it. Understand shared-versus-local ownership before mutating; do not invent runtime style mutation the project does not do.
- Typography belongs in the Theme where the project manages it there. Do not scatter font sizes across nodes, and do not build fixed layouts that break when text grows.

Prefer the built-in styled states for hover, focus, pressed, and disabled over script-level color or scale hacks.

## 5. Layering

Study the existing `CanvasLayer` structure before adding one — a new layer is not the default fix. Screen-space UI and world-space UI are different; do not mix them.

Choose a layer value from its real relationship to existing HUD, menu, modal, and system overlays. Do not invent magic numbers, and do not mask an ordering problem with a huge z value — check scene hierarchy, layer structure, sibling order, and modal ownership first.

## 6. Scenes and reuse

Extract a reusable UI scene when there is a real boundary: a reused visual contract, reused interaction behavior, a semantic unit, or independent state. Not for every label-and-button pair.

When a similar scene exists, extend or reuse it — but do not pile unrelated options onto it until it becomes a god component. Follow the project's inheritance or composition pattern rather than imposing a new one.

Scripts often depend on node paths. When restructuring a UI subtree, do not break existing references; follow the project's referencing style rather than guessing a version-specific shorthand. Redesigning the referencing system is a separate task.

## 7. Focus and controller navigation

Where the project supports controller or keyboard, focus behavior is functionality, not polish.

- Preserve the project's existing focus configuration: which controls are focusable, where focus starts, how it leaves and returns.
- Let automatic directional navigation work where it is correct. Add explicit neighbor relationships where the layout defeats it — grids, inventories, asymmetric menus, multi-column settings, skill trees — not everywhere by reflex.
- Verify there are no dead ends: focus lost, focus landing on non-interactive nodes, no path back, unexpected diagonal jumps, focus trapped in a hidden subtree.
- Assign a deliberate default focus when a menu opens — the primary, previous, or contextual target, not blindly the first child.
- Restore a meaningful focus when a submenu or dialog closes, within the project's existing state model.
- Re-check the focus graph for hidden and disabled states. Hiding a control that currently holds focus breaks navigation unless focus is transferred.
- A disabled control must not look or behave as focusable and clickable. Confirm the actual property behavior from existing components.
- In a scrollable menu or inventory, focus movement must keep the focused item visible; otherwise controller navigation is unusable.
- The focus indicator must be clearly visible in the project's theme.

## 8. Input and pointer ownership

- Follow the project's existing input pattern. Do not pick between the general input callbacks and the control-level one at random — read how the codebase already routes UI input, and solve ownership at the right layer rather than with a global hack in a UI script.
- Whether a menu suppresses gameplay input follows the project's game-state architecture.
- A transparent full-screen control can silently intercept clicks. When input goes missing, inspect the scene hierarchy, hit areas, and each control's pointer-filtering behavior before changing anything else.
- Use the project's existing input actions. Do not invent new action names, and do not assume built-in UI actions exist without checking the project's input map and version.
- Use the project's input abstraction and glyph system for button prompts. Never hardcode controller-specific glyphs or per-brand mappings into UI, and do not invent a glyph system where none exists.

## 9. Viewport, resolution, aspect ratio

UI must work with the project's viewport and content-scaling strategy — read those settings before adding a screen if the layout depends on them. Terminology and behavior vary by version; do not guess.

Do not change project-wide stretch or viewport settings to fix one broken screen. Those settings affect the world, camera, every UI, pixel-art fidelity, and input coordinates. Investigate the local layout problem first.

Build screens that survive the aspect ratios the project actually supports, through anchors and containers rather than one assumed resolution — and do not claim support for ratios the project does not target.

The editor preview at one viewport size proves little. For a structural change, verify at more than one size, at runtime where possible.

## 10. Modals, windows, pause

Reuse the project's established modal primitive, manage focus and input ownership explicitly when it opens and closes, and avoid stacking unnecessary modal scenes. Let the project and version decide the exact class rather than picking one from memory.

An in-game dialog is not an OS window. Do not open a native window for something the project renders in-game.

Pause UI depends on the project's pause and processing architecture. Read it; do not assume the property names or the default behavior.

## 11. Text and media

Follow the project's existing choice of text control. Do not reach for a rich-text control just to style a short label — use it for genuinely formatted, multi-style, inline, or long content.

Dynamic text affects wrapping, minimum size, container expansion, and translation. Do not trap growing text in a fixed-size panel. If the project uses localization, route new UI text through it rather than hardcoding production strings — and do not build a localization or RTL system that the project does not have.

For images, match the stretch and aspect behavior to the intent instead of distorting the asset, and reuse the project's nine-patch or scalable panel setup for panel artwork rather than stretching a bitmap into distorted corners.

## 12. Motion

Use the project's existing animation approach — do not introduce a second animation system for one transition. Keep motion functional and avoid continuously animated controls that cost frames for decoration.

## 13. Boundaries

UI scripts display state and emit intent. Keep combat math, inventory rules, economy, quest progression, stat logic, save rules, and network authority out of them unless the project's architecture deliberately places them there.

- Do not create an autoload or singleton to solve a local UI problem.
- Do not reach across the tree by absolute node path to find unrelated systems; use the project's existing communication pattern.
- Bind the HUD through the project's existing signal, mediator, or state model rather than to a gameplay object's internals. Do not impose a new architecture.

## 14. Scene files and diffs

`.tscn` and `.tres` files are text but are editor-generated. Do not restructure them by blind text manipulation — resource ids, sub-resources, node paths, and external references break quietly. Prefer the project's own workflow or tooling where available.

In a scoped task, change only the nodes and properties the task requires. Do not rewrite ordering, metadata, or formatting the editor produced; keep the diff proportional to the change.

`project.godot` changes have project-wide reach. Do not modify it for a problem a local fix solves, and never change it without understanding what else it affects.

## 15. Verification and tooling

After a UI change, confirm the scene still loads and that node paths, resources, and signal connections resolve. A UI that fails to render is an error to fix, not a design finding — resolve parse errors, invalid properties, missing paths, resource load failures, and broken signal connections before touching visual polish.

Verify the behavior you changed — layout stability at supported sizes, focus movement, input ownership — not the whole game.

Never assume an engine MCP or runtime tool exists. Where one is genuinely available, it is worth using for focus navigation, HUD overlap, wrong anchors, scaling, hidden controls, pointer interception, menu state, and aspect ratio — problems static reading settles poorly. Without it, work from the scenes, scripts, and editor evidence you have.

## 16. Runtime cost

Do not update a UI value every frame when it changes on an event; use the project's signal or data-binding pattern. Do not rebuild an entire UI subtree on every state change — it costs frames and destroys focus and state.

Avoid large transparent layers, heavy shader effects, and many continuously animated controls without a concrete UI benefit. Do not micro-optimize node counts at the cost of correct structure without evidence of a real problem.

## 17. Godot anti-patterns — do not

- Position UI with hardcoded coordinates, or do manual layout math where a container expresses it.
- Use offsets as the strategy for surviving resolution changes.
- Apply a full-rect preset to every control.
- Nest layout wrappers deeper than the relationship requires.
- Repeat the same per-node theme override, or create a new Theme for one component.
- Invent CanvasLayer values, or bury a hierarchy problem under a huge z value.
- Leave a controller-supported game with a pointer-only menu, or map focus neighbors manually everywhere.
- Let focus vanish when a menu hides.
- Leave a transparent full-screen control eating input.
- Change project-wide viewport or stretch settings to fix one screen.
- Put gameplay rules in a UI script, or add an autoload for a local UI problem.
- Use a version-specific API without confirming the project's engine version.

## 18. Investigation budget

Read in order, stopping when you have enough: `target scene` → `its UI subtree` → `reusable scenes it uses` → `Theme` → `input and focus configuration if relevant` → `viewport or project settings only if relevant`.

Do not scan every scene by default. Keep the change inside the requested scope: a focus fix in one screen is not a reason to restructure the theme system, the HUD, global input, or project settings.
