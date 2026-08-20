---
name: mobile-ui-ios
description: Use when implementing or fixing UI in a native iOS or iPadOS app - SwiftUI or UIKit screens, navigation and sheets, safe areas, Dynamic Type, SF Symbols, keyboard behavior, iPad adaptation, or Apple platform conventions in an Xcode project. Not for Android or web, and not for non-visual logic changes with no UI impact. In a Flutter or React Native codebase, use only when the task is specifically about iOS-native behavior.
---

# iOS UI Implementation

Apple-platform specialization of `mobile-ui-design`. Core and generic mobile rules still apply and are not repeated here. This file answers one question: how does a mobile UI decision take its correct native form on iOS and iPadOS?

## Applies when

The project builds a native iOS or iPadOS app — an Xcode project, Swift sources, SwiftUI or UIKit view code. A shared Flutter, React Native, or Kotlin Multiplatform screen is not this skill's job; it becomes relevant only when the task is specifically about iOS-native behavior inside such a project.

**Framework.** Determine SwiftUI, UIKit, or hybrid from the code before writing anything. Never assume SwiftUI. Do not impose SwiftUI patterns on a UIKit project or drag UIKit architecture into a SwiftUI one, and preserve the existing boundaries in a hybrid.

**Version.** Never assume an OS version, Swift version, or deployment target. Read project settings, package manifests, existing availability checks, and current source usage. If you cannot confirm an API exists on the project's minimum target, do not use it — express the intent and follow the pattern the codebase already uses.

Insert into the priority ladder: existing project conventions, the actual framework and deployment target, native interaction behavior, existing navigation architecture, safe-area and keyboard correctness, and Dynamic Type resilience all outrank visual polish.

## 1. Project before platform

```
existing iOS design system → existing components → existing navigation architecture →
existing presentation patterns → existing typography and icon conventions →
Apple-native primitive → custom implementation
```

Where a strong native pattern exists for navigation, lists, forms, sheets, alerts, menus, search, selection, or controls, evaluate it before building custom behavior. Native does not mean generic — the visual language can come from the project while the interaction behavior stays familiar.

Apple's Human Interface Guidelines are a source of platform expectation, not a rule engine. A screen is not defective merely because it does not look like Apple's default. Ask instead: is the interaction still familiar, is accessibility intact, is system behavior respected, is the project consistent? Do not "make it more iOS" as a goal, and never restate a guideline number or requirement from memory — verify it.

## 2. Safe areas and system chrome

Use the framework's native safe-area and layout-guide mechanisms. Never hardcode status-bar, home-indicator, notch, or keyboard dimensions.

Content may extend beyond the safe area deliberately — a background, full-bleed media, decorative artwork. Interactive controls and critical content may not. Ignoring the safe area is never a styling shortcut.

Do not override status-bar or system-chrome appearance for visual preference alone; readability and contrast come first, and immersive treatment is a project decision. Keep bottom controls clear of the home-indicator and system-gesture region through the environment's own inset values.

## 3. Navigation

Follow the app's existing navigation architecture — hierarchical, tab or sidebar top-level, modal, split, or a custom coordinator. Do not introduce a new navigation model, or change top-level structure, unless the task requires it. Let project evidence decide the concrete navigation type; do not name one from memory.

- Preserve native back semantics and the system back gesture. Replace them only when the flow genuinely needs different semantics — not for a custom-styled button.
- Title prominence follows screen hierarchy, content role, and the project's existing convention. Do not vary it for visual preference, and do not put a large title on every screen.
- Top-level destinations are stable sections, not actions. Do not restructure top-level navigation based on screen count.
- Fix a navigation bug at the screen or presentation level first; do not rewrite the app's router or coordinator for it.

## 4. Presentation

Sheets are a genuine platform pattern, not the default container for every secondary flow. Choose between sheet and full-screen presentation from flow semantics — context preservation, task length, dismissibility, content volume — never from visual preference.

Where the project uses sheet sizing, check the content against it: minimum useful height, keyboard interaction, scrolling, and reachable primary actions. Do not attach arbitrary sizes, and do not hardcode version-dependent presentation properties.

If interactive dismissal can discard user work, follow the project's existing handling. Do not invent a confirmation flow the product does not have.

- Alerts are for short, high-attention decisions. Not for every error or success — inline feedback or the project's existing feedback primitive is usually right.
- Use the project's existing pattern for contextual action lists, and keep destructive actions clearly presented.
- Context menus carry secondary, optional actions. Never hide a primary action behind a long press.

## 5. Lists and forms

Native list and table patterns are strong, but not every collection of content should be forced into a system list. Weigh scanability, selection, navigation, grouping, swipe actions, and editing against the project's visual language.

Row interaction must be unambiguous. When a row combines navigation, a toggle, swipe actions, a context menu, and a trailing button, check for conflict — making the whole row tappable is not always right. Swipe actions suit secondary contextual actions; they must not be the only discoverable path to a critical or destructive one. Follow existing conventions for edit, reorder, and delete rather than inventing a mode.

Forms should reuse the project's native form or list pattern rather than a desktop-style grid. For text input, set the keyboard configuration, content type, return action, secure entry, and autocorrect behavior that the field's real semantics call for — do not infer a field's meaning without evidence. A previous/next/done input accessory is not mandatory; add it only where the flow needs it.

Use the system and framework keyboard-avoidance mechanism the project already uses. Never hardcode a keyboard height.

## 6. Typography and text

Text scales. Preserve the project's Dynamic Type strategy and never defeat it: no fixed heights around scalable text, no shrinking text to protect a layout, no clipping at accessibility sizes.

Where the project uses system semantic text styles, prefer them over new point values. Where it has a custom typography system, keep it — and carry its Dynamic Type behavior with it. A fixed size can be intentional; verify it does not break scaling.

Route new UI text through the project's localization pattern instead of hardcoding production copy, and do not build a localization system where none exists. Layout must survive both translated strings and enlarged text — do not size buttons and titles to short English.

If the project supports right-to-left, keep direction-aware leading and trailing semantics; do not introduce fixed left/right assumptions for visual alignment.

## 7. Color and materials

If the project supports Dark Mode, use its semantic colors, color assets, or tokens rather than hardcoded values. If it does not, do not build a theming architecture for a UI task. A custom brand palette does not need to become system colors — but contrast and state semantics must hold in every appearance.

Blur, translucency, and system materials belong to the platform's visual language, but not on every surface. Use one for real depth, context separation, or system-chrome relationship — not to manufacture a glass effect.

## 8. Symbols, icons, assets

Follow the project's existing icon vocabulary. SF Symbols are a native option, not an obligation; if the project ships a custom icon set, keep it. Choose between a symbol and a custom asset from semantic fit, brand requirement, existing convention, and availability on the deployment target.

Never invent a symbol name or assume a symbol exists on a given OS version — take it from existing usage or a verified source. A system icon does not by itself make a control's meaning clear: icon-only controls still need recognizable context and an accessible name.

Use the project's asset catalog and its existing vector-versus-raster strategy. Do not add hardcoded file paths or duplicate imports.

## 9. iPhone, iPad, and adaptation

Do not assume iPad support — confirm it from project targets. Where it exists, iPad is not a large iPhone: consider split views, a sidebar, persistent secondary context, multi-column navigation, and different presentation behavior for popovers. Adapt structure only where the extra space genuinely improves the task; do not split unrelated screens into two columns because space allows.

Drive layout from available space and the project's existing adaptive model, not from device models. Never branch on a specific iPhone or iPad model. Follow the project's size-class or environment-based adaptation rather than introducing a new one.

Where the app supports multitasking or resizable windows, do not assume a single full-screen size. Confirm supported orientations from project settings rather than assuming portrait; if landscape is supported, re-check keyboard, navigation, content width, toolbars, and safe areas.

Hardware keyboard, pointer, and drag-and-drop matter in iPad and productivity contexts — treat them as enhancements on evidence, never as assumptions, and never let pointer or hover thinking weaken the touch interaction underneath.

Toolbar capacity is limited on iPhone: prioritize primary and frequent actions and move secondary ones into the project's existing menu pattern, without burying anything critical. A bottom-anchored primary action is sometimes right, not a rule — weigh safe area, keyboard, flow semantics, and navigation chrome together.

## 10. Accessibility specialization

- Keep layouts resilient at large Dynamic Type sizes: no clipping, no truncated buttons, no fixed card heights, no crowded horizontal control rows.
- Custom controls must retain system semantics — meaningful VoiceOver labels, roles, and actions, matching the project's accessibility approach.
- Keep accessibility traversal order aligned with logical order, especially with overlays, floating controls, and complex layouts.
- Respect Reduce Motion and contrast-related system settings where the project supports them. Reduced motion does not mean no transition — provide a meaningful alternative.

Detailed VoiceOver API guidance and full audits belong elsewhere.

## 11. Feedback and haptics

Follow the project's existing haptic language, and only for a semantic role: selection, confirmation, warning, or a meaningful boundary. Not every tap. Avoid stacking system feedback, a custom toast, a haptic, and an animation onto one action.

Native transitions are usually sufficient. Custom motion needs a purpose — state relationship, continuity, feedback, or orientation.

## 12. Permissions and system services

Do not invent a permission requirement. Where the project genuinely has the capability, ask in context, explain the value when useful, handle denial as a real state, and do not nag. Never imitate a system permission prompt with a fake dialog, and do not add a custom pre-prompt for every permission. Route to Settings after denial only where it is genuinely useful and matches project behavior.

Use system services — share, photo picker, camera, maps, authentication — only when the task actually involves them, preferring the system component over an imitation once availability and project requirements are confirmed. Do not invent entitlements, capabilities, or plist keys during a UI task.

## 13. Framework discipline

**SwiftUI projects.** Prefer declarative, adaptive layout over device-specific geometry math. Geometry measurement is for cases that genuinely need layout information — not for centering, equal splits, or safe-area handling that the layout system already expresses. Compose with the project's existing layout components rather than nesting stacks indefinitely.

**UIKit projects.** Follow the existing layout system and its constraint helpers. Manual frame math is justified only where the codebase or a specialized case requires it, and it is fragile under Dynamic Type, rotation, and iPad adaptation. Reuse existing layout constants instead of scattering one-off values.

In a hybrid project, follow the existing interop boundary. Do not migrate a screen between frameworks unless the task asks for it.

The app's architecture and state model are not this skill's to change. Use the project's existing screen architecture, state handling, async and loading model, and navigation plumbing. Represent the current state correctly; do not add optimistic behavior the data layer does not support.

## 14. API and version discipline

Do not adopt a newer platform API to feel modern. Check the deployment target, project compatibility, existing platform strategy, and fallback needs first, and use the project's existing availability approach rather than adding OS branching for a UI change. Newer system chrome and appearance behaviors are especially version-dependent — the project's current implementation is stronger evidence than memory.

When you need exact Apple behavior, verify it from existing project usage, the installed SDK and tooling, or official documentation if reachable.

## 15. Do not import other platforms

Do not carry Android patterns onto iOS without evidence: Material navigation behavior, Android back assumptions, Material component styling, Android-specific permission UX, or a floating action button as the default primary action. Likewise do not carry web patterns: hover-dependent actions, a desktop sidebar on iPhone, dense tiny toolbars, breadcrumb navigation, breakpoint thinking, or desktop modal forms.

Conversely, if the project already uses a working custom feedback primitive, do not replace it just because a system equivalent exists.

## 16. iOS anti-patterns — do not

- Give every screen a large navigation title, or open a sheet for every action.
- Make every list row a rounded card, or put a material or blur on every surface.
- Use SF Symbols without semantic fit, or invent a symbol name.
- Hardcode safe-area padding, keyboard height, or device dimensions.
- Branch layout on a device model.
- Set fixed font sizes that break Dynamic Type.
- Stretch an iPhone layout onto iPad and call it iPad support.
- Rewrite a UIKit project in SwiftUI, or bridge a SwiftUI project into UIKit, without being asked.
- Use a newer API without deployment-target evidence.
- Replace native back behavior with a custom one.
- Imitate a system permission prompt with a fake dialog.

Adding blur, large titles, symbols, rounded rectangles, and sheets does not make an app native. Native comes from interaction, behavior, and platform consistency.

## 17. Verification and discipline

Verify what you changed: the screen renders, navigation works, presentation behaves, the keyboard does not obscure required UI, layout holds at larger Dynamic Type sizes, safe areas are respected, and the expanded or iPad layout works where supported. Not the whole app.

Where simulator or device tooling is genuinely available, use it — rendered output settles safe-area bugs, keyboard overlap, sheet sizing, Dynamic Type clipping, iPad adaptation, and navigation chrome far better than reading code. Never assume such tooling exists. Where the project has UI or snapshot tests, follow its testing strategy; do not introduce a testing framework for a small UI change.

Keep diffs proportional: no unrelated formatting churn, no mass view rewrites, no asset reordering, and no edits to Xcode project configuration files unless the task truly requires them. Deployment target, orientation support, capabilities, and plist keys are project-wide — do not change them for a local UI problem.

## 18. Investigation budget

Read in order, stopping when you have enough: `target screen` → `nearby components` → `design system, colors, typography` → `navigation or presentation layer if relevant` → `project settings only if relevant`.

Keep the change inside the requested scope. A sheet fix on one screen is not a reason to touch navigation architecture, the typography system, other screens, or the deployment target.
