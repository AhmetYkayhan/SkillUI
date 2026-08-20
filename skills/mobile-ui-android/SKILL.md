---
name: mobile-ui-android
description: Use when implementing or fixing UI in a native Android app - Jetpack Compose or Android Views screens, system bars and window insets, back behavior, IME and keyboard handling, Material components and theming, large-screen adaptation, or Android platform conventions in a Gradle Android project. Not for iOS, Flutter or React Native shared UI, or web.
---

# Android UI Implementation

Android specialization of `mobile-ui-design`. Core and generic mobile rules still apply and are not repeated here. This file answers one question: how does a mobile UI decision take its correct native form on Android?

## Applies when

The project builds a native Android app — a Gradle Android module, `AndroidManifest.xml`, `res/` resources, Compose or View code. Kotlin or Java alone is not evidence; a JVM or backend project is not Android UI. A shared Flutter, React Native, or Kotlin Multiplatform screen is not this skill's job; it becomes relevant only when the task is specifically about Android-native behavior inside such a project.

**Framework.** Determine Compose, Views/XML, or hybrid from the code before writing anything. Never assume Compose. Do not impose Compose architecture on a Views project or push a Compose project back toward XML, and preserve existing boundaries in a hybrid. Migrating a screen between them is a separate task.

**Version.** Never assume an Android version, API level, minSdk, targetSdk, or a Compose, Material, Kotlin, or Gradle version. Read the Gradle configuration, version catalog, manifest, and existing sources. If you cannot confirm an API exists on the project's minimum SDK, do not use it — express the intent and follow the pattern the codebase already uses.

Insert into the priority ladder: existing project conventions, the actual framework and SDK context, native system behavior, existing navigation and back architecture, inset and IME correctness, and font-scaling resilience all outrank visual polish.

## 1. Project before platform

```
existing Android design system → existing components → existing navigation architecture →
existing presentation patterns → existing typography and icon conventions →
platform or project primitive → custom implementation
```

Where a strong platform pattern exists for navigation, lists, forms, dialogs, sheets, menus, selection, system bars, or back behavior, evaluate it before building custom interaction.

Material Design is a reference, not a rule engine. If the project does not use Material, do not impose a Material look; if it does, follow its own token layer rather than component defaults. A screen is not defective because it differs from Material defaults — ask instead whether the interaction is understandable, system behavior is respected, accessibility holds, and the project stays consistent. Never restate a current Material guideline or measurement from memory; verify it.

## 2. System bars and insets

Account for the status bar, navigation bar and gesture region, display cutouts, the IME, and system overlays through the project's actual window-inset handling. Never hardcode a status-bar, navigation-bar, cutout, or keyboard dimension, and never fix a layout by adding arbitrary top or bottom padding — check window insets, the existing scaffold or layout primitive, and any project helper first.

Edge-to-edge is a project decision, not a default to apply everywhere. Where the project is edge-to-edge, content may extend behind the bars while interactive controls stay inset-safe, system-bar contrast stays readable, and IME behavior stays correct. Extending under the bars is never a styling shortcut.

Keep controls and custom gestures clear of the system gesture regions, working from the real inset values. Do not take over a system edge gesture unless the task genuinely requires it.

## 3. Back behavior

Back is a core Android expectation. Follow the app's existing navigation architecture, and never let custom handling produce an unexpected destination, lost state, a duplicated screen, or a broken system back.

A back affordance in the UI and the system back must express the same navigation semantics; if they differ, confirm that is intentional and supported by the project. Back is not just a toolbar icon.

Newer back behaviors are version- and configuration-dependent. If the project already adopts them, preserve its pattern; do not introduce or assume them from memory without SDK and configuration evidence.

## 4. Navigation

Identify the existing architecture — single-activity, multi-activity, fragments, Compose navigation, a custom router, or hybrid — and keep it. Do not restructure navigation for a UI task, and fix a navigation bug at the screen level before touching the graph or router.

Separate top-level destinations from nested navigation. Choose a top-level pattern from information architecture, window size, destination stability, and existing convention — "Android app means bottom navigation" is not a rule. Top-level items are stable sections, not actions, and item counts come from the project or a verified source, not from memory.

A navigation rail, drawer, or two-pane structure can suit larger windows, but a phone layout does not automatically convert to one because a tablet exists. The project's adaptive strategy decides.

## 5. Presentation and feedback

Bottom sheets are common on Android but are not the default container for every secondary flow. Weigh context preservation, task complexity, content volume, dismissal, and interaction frequency. Modal and persistent sheets are different interactions — keep the semantics the project already uses rather than inventing a new pattern.

- Dialogs are for short, high-attention decisions, not for every validation, error, or success.
- Snackbars suit brief transient feedback, an optional undo, or non-blocking status — not every event, and not when the project has its own feedback language.
- Do not make a toast the default channel for critical or actionable feedback.
- Overflow, dropdown, and contextual menus carry secondary actions; never bury a critical action there.

Choose among these by severity, actionability, and project convention — not because a component is associated with the platform.

## 6. Lists and forms

List behavior matters more than the implementation used. Rows must be scannable, state-clear, and consistent in interaction. Do not make every row an elevated or rounded card when simpler rows carry the hierarchy.

Swipe actions are viable where the project uses them, but must not be the only discoverable path to a critical or destructive action; check gesture conflicts and accidental activation. For large lists, represent the real state model — selection, scroll position, loading, existing paging, empty and error — without inventing a paging or refresh feature. Add pull-to-refresh only where the data flow and product actually call for it.

In forms, follow the project's field labeling, error, and submit conventions rather than shrinking a desktop grid onto a phone. For text input, set the keyboard type, IME action, secure entry, capitalization, autocorrect, and content hints that the field's real semantics call for — do not give every field the same IME action, and do not infer a field's meaning without evidence.

## 7. IME

While the keyboard is open, the focused field, its validation message, the primary action, and any bottom UI must remain reachable. Use the project's and framework's current IME and window-inset handling — never a hardcoded keyboard height, and never manual screen-height math.

Keyboard dismissal should follow the project's existing interaction pattern; dismissing on any background tap is not a universal rule.

## 8. Sizing and typography

Use density-independent sizing and the project's existing dimension and typography tokens. Do not lay out ordinary UI in raw pixels, and do not assume a device density.

Do not impose universal values such as "always 16dp" or "always 48dp". Take a number from project tokens, a verified platform requirement, or an existing component convention.

Text scales with user font and display settings. Avoid fixed-height text containers, clipping, shrinking text to protect a layout, and horizontal control rows that collapse at larger text sizes. If the project uses a custom typography system or custom fonts, keep them and preserve their scaling behavior rather than reverting to system or Material defaults.

## 9. Theme, color, and surfaces

If the project supports a dark theme or a semantic color layer, use it rather than hardcoded colors; if it does not, do not build a theming architecture for a UI task. Do not understand a global theme change as a fix for a local screen.

Dynamic color is an Android capability, not a default. Adopt or preserve it only where the project intends it; do not convert a brand palette into dynamic color because the platform supports it.

Cards, elevation, and surfaces carry layering and interaction meaning — not decoration. Do not add a surface for every group or a shadow to every component, and do not treat a specific elevation or shape scale as universal. Where the project has a shape system, reuse it instead of rounding everything identically.

## 10. Icons and floating actions

Use the project's icon system. Material icons are an option, not an obligation, and a custom brand set stays. Never invent an icon name or assume availability — take it from existing assets and dependencies.

A floating action button fits a high-priority, frequent, screen-level action, and only where it matches the project's conventions. It is not the default home for every primary action, and multiple or extended floating actions need a real reason — prominence must track importance.

## 11. Large screens and windows

Android is not only phones. Where the project supports tablets, foldables, or large screens, consider two-pane layouts, a navigation rail, persistent secondary context, adaptive width, and resizable windows. Where it does not, do not invent the requirement.

Drive layout from the available window size and the project's existing adaptive system — never from device model names. Confirm supported orientations from configuration rather than assuming portrait; where landscape is supported, re-check navigation, IME, vertical space, toolbars, and multi-column behavior. Where multi-window or resizable contexts are supported, drop any fixed portrait-phone assumption.

Foldable posture and hinge handling apply only where the project already targets them. Adaptive APIs in this area are version- and dependency-specific: use what the project depends on rather than naming a package from memory.

An app bar's behavior should follow screen hierarchy, navigation, and action priority; not every screen needs the same treatment, and collapsing behavior needs a content reason rather than an aesthetic one. On a phone toolbar, prioritize the primary and frequent actions and move the rest into the project's menu pattern without hiding anything critical. Search earns prominence from its role — do not add a search bar to every list.

## 12. Accessibility specialization

- Custom controls must keep meaningful TalkBack semantics: roles, states, and accessible names. Icon-only controls need a content description.
- Distinguish decorative imagery from meaningful content; do not attach a spoken label to every decorative asset.
- Keep accessibility traversal aligned with logical order, especially with overlays, grids, reordered content, and floating actions.
- Decide deliberately whether a complex custom component reads as one semantic unit or several.
- Keep layouts intact under enlarged font and display scaling.
- Respect system contrast and motion settings where the project supports them.

Detailed TalkBack API guidance and full audits belong elsewhere.

## 13. Motion, gestures, haptics

Native and project transitions are usually sufficient; custom motion needs a purpose — state relationship, continuity, feedback, or orientation. Follow the project's haptic language and use haptics for a semantic role, not for every interaction.

Do not replace native scroll or fling behavior for visual novelty, and keep scroll ownership unambiguous where a list sits inside a scrolling container, a collapsing bar drives a child scroll, or a sheet contains its own list — follow the project's existing pattern for those. Custom gesture detectors should not re-implement standard click, scroll, and back behavior, and must not conflict with system gestures.

## 14. Permissions and system services

Do not invent a permission requirement. Where the project genuinely uses a capability, ask in context, explain the value when useful, handle denial as a real state, and respect a permanent denial — following the existing flow. Never imitate a system prompt with a fake dialog, do not add a custom rationale screen for every permission, and route to system settings only where it genuinely helps.

Permission behavior varies by API level; read the project's minSdk, targetSdk, and current implementation instead of recalling version rules.

Use system services — photo or file picker, share, date and time pickers, maps, biometric authentication — only when the task involves them, preferring the platform component over an imitation once availability is confirmed. A custom picker built for branding must justify its usability and accessibility cost.

## 15. Framework discipline

**Compose projects.** Follow the existing composition patterns and prefer adaptive composition over device-specific math — constraint-reading composables, raw window width, and device checks are not the first tool for a layout problem. Use lazy containers where data size warrants them, not because they are idiomatic. Do not turn a UI task into a recomposition-performance refactor without evidence.

**Views/XML projects.** Work with the existing layout system and its helpers. Express relationships through the constraint or layout system rather than fixed coordinates, and do not add constraint complexity where a simpler existing pattern fits. Do not modernize a Views screen into Compose because Compose is newer.

In a hybrid project, respect the existing interop boundary.

Custom views and canvas drawing are for genuine custom rendering or interaction, not for UI that standard controls and layouts express — and they carry accessibility and hit-testing consequences.

The app's architecture is not this skill's to change: use the project's existing screen architecture, state holders, lifecycle handling, and data flow. Represent current state correctly rather than introducing a new pattern because it is modern.

## 16. Resources, localization, direction

Before adding a color, dimension, string, drawable, or style, check for an existing semantic one — without forcing a wrong semantic match for the sake of reuse. In Views projects, follow the existing styles, themes, dimensions, and resource-qualifier strategy rather than introducing a new matrix. In Compose projects with a theme or design system, work through its token layer instead of component defaults.

Route production text through the project's string and localization pattern; Compose does not remove that need. Layouts must survive translated strings as well as scaled text. Where the project supports right-to-left, preserve start and end semantics and avoid hardcoded left/right alignment; where it does not, do not start that work.

## 17. API, dependency, and configuration discipline

Do not adopt a newer platform or Material API to feel current. Check the SDK levels, project dependencies, existing platform strategy, and fallback needs first, and follow the project's availability approach rather than adding OS branching for a UI change.

Adding a Material, Compose, or third-party dependency for a single component is not the default answer — use what the project already has, and evaluate compatibility and project standards if a new one is truly needed.

Project-wide configuration — minSdk, targetSdk, compileSdk, the manifest, Gradle dependencies, the theme hierarchy, global edge-to-edge setup, orientation configuration — must not change for a local UI problem. Do not declare new permissions, activities, services, or features during a UI task.

When exact platform behavior matters, verify it from existing project usage, the installed SDK and dependency context, or official documentation if reachable.

## 18. Do not import other platforms

Do not carry web patterns into Android: hover-dependent controls, breadcrumb navigation, desktop modal forms, dense tiny toolbar clusters, breakpoint thinking, or a dashboard grid squeezed onto a phone.

Parity decisions between Android and other platforms are not this skill's job; they belong to a cross-platform layer.

## 19. Android anti-patterns — do not

- Put a floating action button on every screen, or a Material card around every group.
- Make every feedback a snackbar, or every secondary flow a bottom sheet.
- Force Material components into a project with its own design system.
- Hardcode status-bar, navigation-bar, or keyboard padding, or lay out in raw pixels.
- Assume Compose in every Android project, or rewrite a Views project in Compose.
- Use a newer API without SDK evidence.
- Stretch a phone layout across a tablet and call it large-screen support.
- Ignore or override system back behavior.
- Let the IME cover the primary action.
- Enable dynamic color without project intent, or swap every icon for a Material one.
- Build fixed text containers that break font scaling.

Adding a floating action button, cards, sheets, and dynamic color does not make an app native. Native comes from system behavior, back handling, adaptive layout, input, accessibility, and project consistency.

## 20. Verification and discipline

Verify what you changed: the screen renders, navigation and back work, the IME does not block required UI, insets behave, font scaling survives, the large-screen layout works where supported, and the dark theme works where the project has one. Not the whole app.

Where emulator or device tooling is genuinely available, use it — rendered output settles IME overlap, system-bar overlap, font scaling, adaptive layout, dark theme, back flow, and sheet or dialog sizing far better than reading code. Never assume such tooling exists. A correct-looking preview does not prove correct runtime behavior across window sizes, font scales, and insets. Where the project has UI or screenshot tests, follow its strategy; do not add a testing framework for a small change.

Keep diffs proportional: no unrelated formatting churn, no mass XML rewrites, no resource renaming, no Gradle edits, no theme-wide changes.

## 21. Investigation budget

Read in order, stopping when you have enough: `target screen` → `nearby components` → `theme, styles, dimensions, colors` → `navigation or back handling if relevant` → `manifest and Gradle configuration only if relevant`.

Keep the change inside the requested scope. An IME fix on one screen is not a reason to touch navigation architecture, the app theme, the whole form system, other screens, or the minimum SDK.
