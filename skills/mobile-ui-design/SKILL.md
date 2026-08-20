---
name: mobile-ui-design
description: Use when designing or changing UI in a native or cross-platform mobile app - screens, navigation, sheets, lists, forms, touch interaction, safe areas, keyboard behavior, or adaptive layout in SwiftUI, UIKit, Jetpack Compose, Android Views, Flutter, or React Native. Not for responsive websites viewed on a phone, game UI, or non-visual app logic.
---

# Mobile UI Design

Mobile-application specialization of `ui-design-core`. Core rules still apply and are not repeated here. This file answers one question: how does a general UI principle behave in a touch-first app running on a device?

## Applies when

The project builds a mobile application. A responsive website at phone width is not this — that is web, no matter how narrow the viewport. A game HUD, menu, or inventory is the game skill's domain even inside a mobile app.

Insert into the core priority ladder: target platform, native interaction expectations, touch usability, safe and adaptive layout, and input usability outrank visual polish, and rank below existing project conventions.

## 1. Platform evidence

Determine the actual target from the project — project files, source language, manifests, existing screens. Do not infer it from the word "mobile" in the task.

- iOS project → iOS expectations. Android project → Android expectations. Never carry one platform's conventions onto the other by default.
- Cross-platform project → read the codebase's strategy first: one shared visual system, platform-adaptive UI, or a hybrid. Do not invent a strategy, and do not assume the platforms must look identical.
- Platform unclear → stay at generic mobile principles and make no OS-specific claim.

Do not state exact platform numbers — point sizes, target sizes, navigation item limits, spacing values, system metrics — from memory. Use the project's own values, a verified source, or stay at the principle level.

In cross-platform code, branch on platform only for a real difference: interaction convention, system behavior, native expectation, or capability. Not for cosmetic variation.

## 2. Mobile is not a shrunken web page

Do not translate desktop or dashboard layouts onto a device screen. Specifically, do not default to a tile grid of metrics, a desktop-style sidebar, dense equal-weight toolbar rows, a wide table compressed to phone width, tiny secondary controls, or a marketing hero on a functional screen.

## 3. Native conventions before novelty

Before inventing an interaction, work down: existing project convention → platform convention → native or system primitive → custom. Custom interaction needs a concrete usability gain. Familiarity outranks novelty.

Do not replace a native primitive with a custom one just to restyle it; restyle the native one where the framework allows.

## 4. Touch and targets

Touch is imprecise and there is no hover to lean on.

- Actions, labels, meaning, status, and navigation must all be available without hover. Some devices have pointers; the flow must not need one.
- The tap target is the control, not the icon glyph. Icon buttons, close controls, checkboxes, and trailing row actions need a target larger than their visible art.
- Keep adjacent targets from overlapping, especially near destructive actions.
- Separate destructive actions from the primary positive action visually and spatially. Do not add a confirmation step to every destructive action either — follow project convention and consequence severity.

## 5. Gestures

A gesture may be a real platform pattern, but a hidden gesture should not be the only discoverable path to an important action unless the platform convention clearly establishes it. Swipe, long press, and drag are conveniences layered over a visible path.

Do not place custom gestures or controls where they fight system navigation, home, or edge gestures.

## 6. Safe areas and screen assumptions

Lay out against the safe area and system insets, never against the raw screen rectangle. Account for status and system bars, cutouts, home and navigation indicators, gesture regions, and the keyboard.

- Use the framework's inset mechanism. Never solve an inset problem with hardcoded top or bottom padding.
- Do not hardcode a device width, height, bar height, indicator size, or keyboard height.
- Do not build screen layout from absolute coordinates. Absolute positioning is for overlays, anchored elements, and deliberate visual composition only.
- Override system UI presentation only when the product genuinely calls for it, and keep the content/system-UI relationship readable and predictable.

## 7. Software keyboard

Any screen with text input must stay usable while the keyboard is open. The focused field, its validation message, the primary action, and any persistent bottom controls must remain reachable — not hidden behind the keyboard.

Use the project or framework's keyboard and inset handling. Do not patch it with fixed padding.

Set the input's keyboard and content type and the field-advance behavior where the platform offers it, so the sequence through a form is natural.

## 8. Forms

A mobile form is not a desktop form grid scaled down. Do not force several fields side by side into a narrow width.

Prioritize a clear field sequence, readable persistent labels, comfortable input, validation next to the field it concerns, a visible primary action, and low cognitive load per step.

Long forms do not automatically become multi-step flows. Decide from field count, logical grouping, dependencies between fields, and the risk of losing progress. Split only when it genuinely simplifies the task.

## 9. Navigation

Navigation should express the app's information architecture. Read the existing navigation architecture before changing anything; do not decide between tabs, a drawer, or a stack without seeing what the project already uses.

- Persistent top-level navigation represents stable top-level destinations. Not every action or feature earns one.
- Keep back behavior predictable: no surprise destination, no lost state, no duplicated screen, no broken system back gesture.
- Choose depth from the task structure. Neither a long screen-after-screen chain nor cramming everything onto one screen is the default.
- Do not change navigation architecture unless the task requires it.

## 10. Presentation and dismissal

Sheets, dialogs, and full-screen presentation are different tools. Choose from task importance, complexity, how much context must be preserved, content volume, and dismissal expectations.

"The screen looks simpler" and "the content is secondary" are not reasons to present modally. A modal adds interaction cost — do not fragment the main workflow across modal layers, and do not compress a long multi-step flow into a small sheet.

Dismissal must be obvious and predictable. Where dismissal can discard user input, account for accidental dismissal — without inventing confirmation rules the project does not have.

## 11. Lists and rows

List items need a readable hierarchy between primary content, secondary content, state, available actions, and any navigation affordance.

Do not turn every row into a large rounded card when native or simple grouping conveys the same structure.

When a row carries several interactions at once — row tap, trailing actions, selection, swipe actions, a menu — check they do not conflict, and keep the primary behavior obvious.

## 12. Scrolling

Vertical scrolling is normal. Do not shrink type, compress controls, strip labels, or over-densify a layout to fit one viewport — scroll instead.

Avoid nested scroll regions unless the framework primitive supports the pattern deliberately; gesture ownership must be unambiguous. Horizontal scrolling suits carousels, category strips, and wide specialized content, and its presence must be visible — do not slice primary vertical content into horizontal discovery.

## 13. Bottom actions and reach

A persistent bottom action is not a default for every screen. When used, it must coexist with the safe area, keyboard, system navigation, gesture regions, and the scrolling content beneath it.

Place frequent primary actions where they are comfortable to reach, judged together with platform convention, screen structure, and task frequency — not by a blanket rule that primary buttons live at the bottom. Treat one-handed use as a priority only when the task's real usage context calls for it; do not invent that context.

## 14. Text sizing and truncation

Text is not fixed. The UI must survive enlarged accessibility text, multi-line content, translated strings, and long user values. Never shrink text below its accessible size to make it fit.

Avoid fixed heights on containers holding dynamic text or user content; prefer content-driven sizing. If a fixed height causes clipping, truncation, or breaks text scaling, it is the wrong solution.

Do not truncate just to keep a layout tidy. First ask whether the full value matters, whether it can wrap, whether the hierarchy should change, or whether the detail is available elsewhere.

## 15. Screen adaptation

Design for a range of sizes, not one device. Consider small and large phones, tablet or expanded widths where the project supports them, orientation if the app rotates, and enlarged text — but only as far as the task's scope requires.

Reason about available space, safe area, content priority, and the project's own size or window class system. Do not tie layout to a specific device model unless the project or user specifies one. Do not assume rotation or tablet support without evidence; if landscape is supported, remember vertical space and control placement change too, not just width.

Expanded layouts adapt structure, they do not scale pixels. A phone layout centered on a tablet is not a tablet layout — consider additional columns, persistent secondary context, or a split navigation/content structure. Do not import a desktop web dashboard pattern instead.

## 16. Feedback, loading, and connectivity

Touch gives no hover preview, so the result of an action must be legible: a state change, visual response, progress, confirmation, or error. Scale the feedback to the action's importance — not an animation or toast for every tap. Use haptics only for a meaningful confirmation, boundary, selection, or state transition.

Loading should preserve context: keep prior content while refreshing, use local or inline progress, use a skeleton matching the real shape, and block only when the task truly blocks. Do not invent networking, retry, caching, or optimistic-update behavior — the presentation follows the existing data flow.

Express offline, cached, or retry state only if the project already supports it. Do not design an offline capability the backend does not have. Add pull-to-refresh only where data can meaningfully refresh and the convention fits.

Not every success needs an alert. Match the interruption level to how much attention the event actually deserves.

## 17. Accessibility specialization

- Accessibility traversal order must follow the logical content order, even when the visual layout is reordered, floating, or overlaid.
- Controls must expose meaningful names and roles to the screen reader, especially icon-only and custom controls.
- Keep the layout resilient to enlarged system text.
- Where an interaction is gesture-only, provide an alternative path.
- Respect system accessibility preferences, including reduced motion, where the project supports them.

Detailed screen-reader API guidance belongs to a platform specialization, not here.

## 18. Components and external sources

Reuse the project's existing components, navigation and presentation primitives, tokens, and platform conventions before introducing anything new. Use the project's icon system; do not carry one platform's icon language onto the other, and never use emoji as interface icons.

Sourcing priority:
```
existing project component → project design system → verified external resource → new implementation
```

Never assume a design-system MCP, component registry, or Figma connection exists. Verify it is actually available in the session, use it when it helps, and continue normally without it.

When a design source is available, treat it as input, not as a pixel-for-pixel instruction. Reconcile it against safe areas, dynamic text, real content, platform behavior, and existing components, and raise the conflict rather than shipping a layout that breaks on device. A design source is not permission to redesign anything the user did not ask about.

Give media a known aspect ratio, sensible cropping, size adaptation, and defined loading behavior. Do not add imagery to fill a screen that looks empty.

## 19. State, motion, and cost

Do not lose the user's context across navigation, presentation, or a layout or orientation change — while working within the app's existing state model rather than redesigning it.

Motion needs a function: express a state relationship, orient the user through navigation, confirm an action, or preserve continuity. Native transitions are usually enough; do not replace them without reason, and do not animate every tap.

Heavy visual effects — blur, continuous animation, large media surfaces, layered effects — must earn their cost on device. Performance tuning itself is out of scope.

## 20. Mobile anti-patterns — do not

- Lay out against the raw screen instead of the safe area, or hardcode inset padding.
- Let the keyboard cover the focused field or the primary action.
- Use absolute coordinates or fixed device dimensions for screen layout.
- Depend on hover for anything.
- Make an icon-only control the only affordance for an important action.
- Present every flow as a modal, or every row as a card.
- Add a floating persistent CTA without a task reason.
- Build custom navigation where native behavior already works.
- Fight system gestures with custom ones.
- Ship the same layout for phone and tablet.
- Mix iOS and Android conventions on one platform.
- Use glassmorphism, blur, or a card inside a card as a default visual language.

## 21. Effort scaling and investigation

Scale investigation to the task. A padding fix touches the target component and nothing else.

- Keyboard-obscured form → input, scroll behavior, bottom action.
- New flow or onboarding → navigation, content priority, safe area, input, adaptation.
- New expanded or tablet layout → structure, width adaptation, navigation behavior.

Read order: `target screen` → `nearby components` → `tokens and styles` → `navigation/presentation primitive if relevant` → `wider project only if necessary`.

## 22. Do not invent

Never fabricate product decisions to fill a screen: tabs, onboarding steps, settings, account states, permissions, subscription tiers, filters, roles, or features. Use existing project and domain content; ask when the UI decision genuinely depends on missing information.

Do not add a camera, location, notification, or photo permission because a UI pattern suggests it. Follow the capability the project actually has, and its existing permission flow.

Detailed UI and accessibility audits belong to a review skill, not here.
