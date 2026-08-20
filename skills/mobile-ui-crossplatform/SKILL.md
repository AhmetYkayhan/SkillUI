---
name: mobile-ui-crossplatform
description: Use when building or changing mobile UI from one shared codebase targeting both iOS and Android - Flutter, React Native, Kotlin Multiplatform, or Compose Multiplatform - and deciding what stays shared, what adapts per platform, and when a platform branch is justified. Not for single-platform native apps, web, or game UI.
---

# Cross-Platform Mobile UI

Shared-codebase specialization of `mobile-ui-design`. Core and generic mobile rules still apply and are not repeated here. This is not a merge of the iOS and Android skills — it answers one question: how much of this UI stays shared, what genuinely has to adapt, and when does a platform layer need to be brought in?

## Applies when

One codebase targets more than one mobile platform. Confirm the technology from project evidence — a package manifest, framework imports, dependency configuration, a shared project structure, or an explicit statement. Never assume Flutter, React Native, Kotlin Multiplatform, or Compose Multiplatform.

**Shared logic is not shared UI.** A Kotlin Multiplatform project may share business logic while its screens are native SwiftUI and Compose. In that case the native platform skill is the right layer for a UI task, not this one. Determine which architecture the project actually uses: fully shared UI, shared UI with adaptations, shared logic with separate native presentation, or hybrid.

Insert into the priority ladder: the project's existing cross-platform strategy, shared product semantics, native behavior where it materially matters, minimal divergence, accessibility parity, and maintainable branching all outrank visual polish. Pixel parity ranks last unless the user asks for it.

## 1. Bringing in the platform layers

This skill is sufficient for ordinary shared UI work. Pull in a platform layer only when the task actually reaches platform mechanics:

- `mobile-ui-ios` — an iOS-specific bug, iOS system behavior, or iOS presentation divergence.
- `mobile-ui-android` — an Android-specific bug, system back, insets, IME, or Android divergence.
- Both — only when the task is explicitly a two-platform comparison ("how should this behave differently on each?", "fix the navigation differences across both"). This is the exception, not the default.

Do not load both platform layers for a normal shared screen task. It wastes context and pushes toward branching that the task never needed.

## 2. Shared first, adapt when it earns it

```
shared by default → adapt where platform value is real → branch only when necessary
```

Shared-first does not mean the platforms must behave identically. The goal is one product with platform-appropriate behavior — a consistent product, not cloned operating systems.

Two failure modes bound this, and both are real:

- **Lowest common denominator** — reducing the UI to what both platforms do most simply, sacrificing capability and usability to keep code shared.
- **Two apps in one repo** — forking for every small difference until the codebase carries two implementations of everything.

Brand identity, tokens, content hierarchy, domain components, screen purpose, information architecture, and product terminology are usually cheap to share — but the project's existing architecture decides, not this list.

## 3. Follow the project's strategy

Determine the codebase's current stance before writing anything, and keep it. Documentation, the theme structure, and the existing components reveal it. This skill does not impose a preferred strategy, and a UI task never migrates frameworks or restructures the shared/native split.

If the user states a strategy — "keep them identical", "make each feel native" — treat it as a project constraint. It does not override accessibility or system behavior that would break under it; raise the conflict instead.

## 4. Branching discipline

Before adding a platform branch, ask:

1. Is there a real platform behavior difference?
2. Will the user notice it, or will usability suffer without it?
3. Can a shared abstraction express the difference cleanly instead?
4. Does the project already have an adaptation pattern for this?

If not, implement it shared.

**Branch on capability or layout need before branching on OS name.** If available width or window size explains the difference, use that — an OS check is the wrong tool for a size problem. Reserve OS branches for cases where the operating system's own convention is the actual reason.

Keep divergence localized. Platform conditionals must not spread through the UI tree; prefer a shared contract with a contained platform specialization at one boundary. The exact mechanism — a platform-specific module, a separate file variant, a project helper — comes from the codebase, not from a rule here. Where the project already uses per-platform file separation, follow it; do not create a platform pair for every component that does not actually diverge.

Every divergence costs maintenance and test burden. Weigh user value, native necessity, implementation complexity, and future upkeep — and skip cosmetic divergence.

## 5. Abstraction discipline

Do not manufacture a platform-abstraction layer by reflex — a wrapper around every native primitive is overhead, not architecture. Build one only for repeated divergence with a stable semantic boundary and a clear maintenance benefit.

Shared components should express product semantics — role, state, action, priority — not platform mechanics. When a shared component's API fills with platform flags, or platform behavior is steered by a growing pile of booleans, the boundary is in the wrong place. That is a signal to reconsider the split, not to add another flag — though recognizing it does not authorize a large refactor inside a scoped task.

A shared system may allow a platform-specific implementation as an escape hatch. It must not become the normal path.

Signals worth noticing when they are in scope: many platform conditionals, diverging navigation semantics, diverging state contracts, diverging component structure, and platform flags leaking through a shared API.

## 6. Parity means behavior, not pixels

Separate visual parity from behavioral parity. Screens do not need to match pixel for pixel; the product intent and the outcome of an interaction do need to match.

The useful test is: same task, same hierarchy, same semantic state, platform-correct behavior. Screenshot comparison alone is not a parity test — verify navigation result, state, accessibility, keyboard behavior, system back, permission behavior, and safe-area handling per platform.

Typography renders differently across platforms. Preserve semantic hierarchy, readability, and brand intent rather than chasing identical metrics.

## 7. Navigation, presentation, feedback

Keep the shared product information architecture while letting presentation and back behavior follow each platform. The strong pattern is a **shared destination and intent with platform-appropriate presentation** — adopt it where the project already separates them, do not impose it as a new architecture.

Do not build two entirely separate navigation architectures because the platforms differ. If the difference is only presentation, do not duplicate the model.

The same semantic task may use different native presentation per platform — that does not justify two separate screens. Prefer shared content and state with adaptive presentation. A dialog must carry the same decision, consequence, and core content everywhere; its presentation may differ.

Error and feedback semantics are shared; the presentation follows each platform's own conventions. Do not manufacture one platform's feedback component on the other for symmetry — semantic outcome parity is what matters. Destructive-action consequences are product behavior; whether a confirmation is required comes from consequence and domain, never from a platform difference.

## 8. System services and permissions

Permissions, sharing, media and file pickers, camera, biometrics, system settings, date and time selection, and notifications are where native adaptation genuinely pays. Prefer the native capability over a custom cross-platform imitation — and never reimplement a system permission prompt as a shared custom dialog.

Permission capability and the product explanation are shared. The actual request mechanics belong to the platform layer. Do not replace a system-native service with a custom one purely so both platforms look the same; system integration, security, and usability outweigh visual symmetry.

If a capability exists on one platform and not the other, do not fake parity. Ask first whether the shared experience survives through a different native mechanism; if it cannot, graceful divergence driven by product context is the honest answer.

## 9. Design system, tokens, assets

Work through the project's existing token or theme system; do not invent a new token architecture. Keep tokens semantic: a platform name is not a semantic role, so a per-platform duplicate is justified only when the value or meaning genuinely differs. Where a platform override is needed, keep it localized, reasoned, and under a shared semantic contract.

Shared brand typography and iconography are normal. Platform-specific icon semantics can matter for back, share, and other system actions — that does not mean duplicating the whole icon set. Brand assets are shared; platform system and launcher assets are not, and asset strategy is not redesigned during a UI task.

Do not force a shared token onto a native control where it breaks the control's usability.

## 10. Accessibility parity

Accessibility is part of the shared feature contract. A feature that is accessible on one platform and not the other has not achieved parity.

A shared custom component must produce meaningful semantics on both platforms — a clean abstraction on one side that degrades on the other is a defect. Shared typography must respect each platform's text-scaling behavior. The platform APIs and their details stay in the platform layers.

## 11. Layout and platform surfaces

Reason about available space rather than device or OS identity. Where the project supports tablets, foldables, or large screens, look for shared expanded-layout semantics first — do not merely enlarge the phone screen, and do not start with two separate tablet UIs. Split presentation only where the platforms genuinely need it. Do not assume portrait-only.

Do not implement a single hardcoded shared strategy for safe areas, system insets, or keyboard height. Each platform's real geometry comes from its own framework mechanism; the shared layer keeps the input flow and layout intent consistent while the platform layer supplies the mechanics.

## 12. Dependencies and native code

Before adding a package to solve a cross-platform problem, check the framework's own capability, existing project components, and current dependencies. A new package carries maintenance, platform support, version compatibility, and native-dependency cost — and a heavy shared package for one platform difference is a bad trade.

Where native capability is genuinely required, use the project's existing bridge, plugin, or native-module architecture. Do not build new native infrastructure for a visual outcome, and do not fork shared UI to native on a generic assumption that native is faster — that needs real evidence.

## 13. Verification

A change to a shared component affects every platform. Before altering shared code to fix a platform-specific bug, ask whether it changes the other platform — a localized override is often the safer fix. Equally, do not fork a shared component over one small bug before checking whether a contained fix exists.

Verify shared changes on the platforms they affect, not on one and not across an exhaustive device matrix. A platform-specific branch change does not require a full regression pass on the other platform when the shared contract is untouched.

Hot reload, a preview, or one emulator looking correct does not prove native behavior on both platforms. For a platform-specific problem, the corresponding runtime is the stronger evidence. Use simulator, emulator, framework tooling, logs, or screenshots where they genuinely exist — never assume they do.

Where a design source covers both platforms, read it as product intent, hierarchy, and brand, not as a pixel-perfect instruction for two operating systems. Where it contains separate per-platform designs, classify the difference — visual only, system behavior, navigation, or native component — before duplicating any code.

## 14. Cross-platform anti-patterns — do not

- Load both platform layers for every shared task.
- Duplicate every screen per platform.
- Scatter OS checks through the component tree.
- Wrap every primitive in a platform abstraction.
- Chase pixel-perfect parity across platforms.
- Impose one platform's design language or navigation semantics on the other.
- Branch on OS when available width would solve it.
- Add platform flags until a shared component's API is unreadable.
- Rebuild a system picker or prompt as a shared custom component for visual symmetry.
- Share so aggressively that native usability is lost, or adapt so aggressively that one codebase becomes two apps.

## 15. Scope

Keep the change inside the requested scope: a platform-specific inset bug is not a reason to revisit the other platform's navigation, the shared typography system, or the component library.

Read in order, stopping when you have enough: `target shared screen` → `shared components it uses` → `theme and tokens` → `existing platform adaptation points` → `platform-specific code only when the task reaches it`.

A comprehensive parity audit is a review task — combine this skill with the review skill and the relevant platform layer rather than growing an audit checklist here.
