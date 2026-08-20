---
name: game-ui-design
description: Use when designing or changing game interface - HUD, pause menu, inventory, quest log, map or minimap, skill tree, scoreboard, settings and key rebinding, crosshair, gameplay overlays, or controller focus navigation - in Godot, Unity, Unreal, any custom engine or game framework, or a game rendered in a browser or on a mobile device. Not for business dashboards, productivity apps, or game logic without a UI change.
---

# Game UI Design

Game specialization of `ui-design-core`. Core rules still apply and are not repeated here. This file answers one question: how does a general UI principle behave in a real-time, input-variable game?

## Applies when

The interface belongs to a game: HUD, menus, overlays, or any surface layered on interactive gameplay. Domain outranks technology — a game rendered in a browser or running on a phone is still game UI, and web-dashboard or native-app patterns do not transfer to it. Conversely, a productivity app is not game UI just because it is built in an engine.

Insert into the core priority ladder: gameplay visibility, input reliability, focus navigation, and resolution resilience outrank visual polish, and rank below existing project conventions.

## 1. Evidence before assumption

Read the project before deciding. Never assume from memory: engine, genre, input methods, controller or touch support, target resolution, supported aspect ratios, console or TV context, safe-zone requirements, multiplayer, or any game mechanic.

- Engine comes from project files or an explicit statement. Unknown engine → stay engine-neutral.
- Genre conventions apply only with project evidence. Do not fit a generic HUD — health, ammo, minimap, quest tracker — onto an unknown game.
- Never invent mechanics or systems to fill a screen: health, stamina, mana, XP, levels, currency, inventory, quests, crafting, timers, scores, party, or match state. Use what the project actually has, and ask when the UI decision truly depends on missing information.

Do not modify combat, inventory, AI, quest, economy, movement, networking, or save logic. Integrate with existing state only as far as displaying it requires.

## 2. Gameplay UI is not menu UI

Two contexts with different rules.

**Gameplay UI** — fast, glanceable, non-blocking where possible, contextual, low cognitive load. Do not give it menu-level density.

**Menu UI** — navigable, structured, readable, focus-safe, denser where the content justifies it. Do not design it as a gameplay overlay.

## 3. HUD information priority

Not everything belongs on screen at once. Sort information into persistent (frequently needed state), contextual (relevant only in a specific state or interaction), and transient (event feedback). Only persistent information earns permanent HUD space; event feedback does not become a permanent element.

Show what matters when it matters. Contextual systems — ammo, interaction prompt, vehicle controls, stealth state, build controls, quest hint — appear with their context, but hiding one must never cost the player a critical state they still need.

## 4. Gameplay occlusion

The world is the primary surface. Keep large UI off the aiming area, enemy visibility, navigation path, interactive objects, and critical world cues.

Screen center is expensive space: keep large text, notifications, quest updates, damage banners, and prompts out of it unless they are brief and critical.

Peripheral placement often suits persistent HUD, but it is not a universal rule — decide from eye travel, camera movement, combat pace, viewing distance, and how urgent the information is.

## 5. Input methods

Determine supported inputs from the project. Do not design for a pointer, a controller, or touch that the project has not confirmed.

When several inputs are supported, the UI must survive switching between them mid-session: focus model, on-screen prompts, cursor visibility, and control hints all stay consistent with the device actually in use. Never leave a keyboard prompt on screen after the player picked up a controller.

Prompts are contextual and brief. Do not paper the screen with button hints, and do not keep re-teaching controls the player already knows.

Affordance is per input: a pointer reads hover and cursor, a controller reads focus and selection, touch reads a tappable surface. One visual state does not automatically serve all three.

## 6. Focus navigation

If a controller or keyboard is supported, no menu may be pointer-only.

- Every required action must be reachable by directional navigation, with no dead ends and no trapped focus.
- Focus is a persistent selection, not a transient hover. Do not model it as hover, and do not conflate hover, selection, focus, and active states in mixed-input UI.
- The focused element must be immediately locatable, stable, and unambiguous. A faint color shift alone is not enough.
- Spatial layouts need a deliberate focus graph, not a linear sequence: grids, inventories, skill trees, loadouts, and multi-column settings must move up, down, left, and right in a way that matches what the player sees. Empty and disabled slots must not break the traversal.
- Returning from a submenu should restore a sensible focus rather than dumping the player back at the first item — within the project's existing state model.

Where touch is supported, check target size, thumb reach, occlusion by the hand, and conflicts with camera or movement controls. A virtual button layer bolted onto a controller UI is not a touch design.

## 7. Resolution, aspect ratio, and scale

Never build game UI from hardcoded coordinates or a single assumed screen size. No fixed x/y layout, no 1920×1080 assumption, no single pixel-density assumption.

Use the project's own anchoring, container, and scaling systems before manual coordinate math. Follow its reference resolution and viewport strategy rather than introducing a new scaling architecture.

Do not assume 16:9. Support the ratios the project actually targets, and remember that a wider screen does not mean the HUD should slide to the outer edges — eye travel and gameplay relevance decide placement; a constrained UI region is often correct.

If the project targets a safe presentation region, keep critical UI inside it. Do not invent safe-area or certification numbers. If windowed or resizable targets exist, the UI must survive a runtime resize.

## 8. Display context

Viewing distance changes readability requirements. Where the project targets TV or couch play, text, labels, focus indicators, and critical HUD values must read from a distance — do not size them for a desk monitor. Handheld targets have their own size-and-distance relationship, and a handheld game is still game UI, not a native app screen.

Apply a display context only with project evidence, and only the one that applies. Do not reason about every platform at once.

## 9. Game states, layering, and input ownership

Opening an overlay changes who owns input. Inventory, map, pause, dialogue choice, and crafting can take gameplay input partly or fully — that must be unambiguous: whether gameplay still receives input, whether the camera still moves, whether a cursor appears, and where focus lands.

Pause UI depends on whether the game actually pauses. Verify it; never design a pause interaction for an online or multiplayer game that cannot stop.

Layering must protect importance: critical UI is never covered by a lower-priority overlay. Follow the project's existing layer model rather than inventing a fixed stack.

Transient messages — achievement, loot, quest update, warning, system message — must not pile onto each other or bury gameplay when several arrive at once. Follow the project's queueing behavior; do not invent an event system.

Scene and menu transitions should leave the player certain about where they are going, whether input is locked, and whether loading is happening.

## 10. Feedback and urgency

Where combat or state feedback exists, damage, healing, status, cooldown, hit confirmation, and resource changes must read instantly. That does not mean adding a floating number, flash, shake, sound, and particle to every event — and never invent feedback the game's systems do not produce.

Preserve an urgency hierarchy. If every state is red, flashing, pulsing, large, and centered, nothing reads as critical. Raise visual priority only for states the game genuinely treats as critical.

HUD motion needs a purpose: a state change, urgency, appearance or disappearance, a relationship, or feedback. Constant pulsing and bouncing pull attention away from play.

Do not encode a gameplay state in color alone — pair it with shape, icon, position, text, pattern, or motion.

Camera shake, flashes, post-processing, and busy scenes can swallow critical UI. Check that critical state survives them. The effects themselves are out of scope.

## 11. Common game surfaces

Apply only what the project actually has.

- **Crosshair or reticle** — part of gameplay: visibility against any background, target feedback, weapon or mode state, and center-screen clutter all matter.
- **Map and minimap** — a persistent minimap must be justified by the existing design. Orientation, player position, objective prominence, legend clarity, and zoom readability are the real problems.
- **Inventory** — item identity, selection, comparison, equipped state, quantity, and available actions must be legible, with focus navigation that works. Show meaningful differences in a comparison rather than weighting every stat equally.
- **Skill trees and graph UI** — current selection, available paths, locked state, and dependencies must be clear, with a directional focus model that matches the visual graph.
- **Settings** — match the control to the value type: toggle, choice, range, binding, or destructive action. Not every numeric value is a slider; when an exact value matters, expose it.
- **Rebinding** — surface the current binding, the listening state, conflicts, a cancel path, and successful assignment. Only if the project has a binding system.
- **Save and load** — slot identity, available progress metadata, selection, overwrite risk, and loading state. Do not fabricate metadata.
- **Scoreboard and multiplayer surfaces** — scanability, comparison, player identity, team grouping, and the few stats that matter, not equal weight for all. Multiplayer UI only where multiplayer exists.
- **Loading screens** — follow existing loading behavior. Do not add tips, lore, or artwork by default, and never show precise progress the game cannot actually measure.
- **Error states** — connection loss, save failure, controller disconnect, unavailable content. Design only for failures the project can actually produce.

## 12. Diegetic UI and immersion

World-integrated UI is a design decision, not a style upgrade. Weigh readability, interaction speed, camera conditions, and accessibility against the fiction. Critical information must not live only in a hard-to-read world element.

"Immersive" is not a reason to make the interface unreadable. A minimal HUD still has to deliver the feedback the player needs.

Match the project's existing visual language and art direction; do not pick a fantasy, sci-fi, retro, or military look without evidence, and never at the cost of readability.

## 13. Text

Avoid long prose during active gameplay — information must be readable at a glance, or the game should appropriately stop for it. Lore, help, and detail belong in a menu or dedicated view.

Game text expands: button prompts, objectives, item names, settings labels, and dialogue choices all grow with translation. Do not lock them into fixed widths. If the project supports text scaling, do not build layouts that break it.

## 14. Menus

A menu may be denser than the HUD, but hierarchy still governs it. Do not put every setting or item in its own large card, and do not import a desktop dashboard aesthetic into a game menu.

Menu background — world, blurred world, static art, or a solid surface — comes from the project's visual language. When a menu overlays live gameplay, make sure the busy scene behind it does not destroy text readability, using the project's own separation approach rather than a reflexive blur or glass panel.

## 15. Accessibility specialization

- Full controller navigability, with clearly visible focus.
- Gameplay cues that do not depend on color alone.
- Restraint with motion and flashing.
- Readability at the project's actual viewing distance.
- Awareness of input remapping and alternative input paths where a gesture or hold is otherwise required.
- Subtitle and text readability where the project has them.

Detailed game accessibility auditing belongs to a review skill or a dedicated specialization.

## 16. Tooling and evidence

Never assume an engine MCP, inspection tool, or runtime capture is available — verify it in the session, use it when it helps, and continue without it.

Sourcing priority:
```
existing project UI → existing engine UI system → verified tooling → new implementation
```

Rendered output often differs from what the code suggests. Where a screenshot, game view, or runtime inspection is available, use it for occlusion, focus traversal, aspect-ratio, and menu-flow problems. Do not infer mechanics or hidden flows from an image.

## 17. Runtime cost

Game UI runs inside a frame budget. Continuous blur, large transparent layers, always-on animation, and heavy dynamic effects need a concrete UI benefit. HUD elements do not need to update every frame — refresh on state change rather than churning. Engine-specific optimization is out of scope.

## 18. Game anti-patterns — do not

- Add a generic sci-fi HUD — glowing panels, hex frames, glass over gameplay — with no game context.
- Keep every stat permanently visible.
- Push all information into screen corners by reflex, or to ultrawide edges automatically.
- Fill the screen center with notifications.
- Build layout from hardcoded coordinates or a single assumed resolution or aspect ratio.
- Ship a pointer-only menu in a controller-supported game.
- Treat controller focus as hover, or leave a broken focus graph in a grid.
- Make menu items too small to navigate comfortably at the target viewing distance.
- Put every menu item in a rounded card, or reuse a desktop dashboard layout as a game menu.
- Invent stats, mechanics, or genre conventions to make a screen look complete.

## 19. Effort scaling and investigation

Scale investigation to the task. One HUD label's alignment touches that element only.

- Gamepad focus bug → the affected screen and its focus graph.
- New inventory UI → structure, input modes, resolution behavior, states.
- New HUD → gameplay priority, occlusion, input, aspect ratio.

Read order: `target scene or screen` → `nearby UI components` → `theme, tokens, layout primitives` → `input and focus code if relevant` → `game state integration if required` → `wider project only if necessary`.

Detailed UI and accessibility audits belong to a review skill, not here.
