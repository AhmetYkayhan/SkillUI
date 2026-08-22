# Changelog

Entries marked **Routing** change which skills activate for a task, so they affect runtime behavior
rather than guidance. Every other entry changes what a skill says once it has already loaded.

## Unreleased

Nothing yet.

## 0.1.0 — 2026-08-22

Initial packaged release. Nine skills, distributed as a Claude Code plugin.

### Added

- `ui-design-core` — universal UI/UX rules: hierarchy, layout, spacing, typography, states,
  accessibility principles, scope preservation.
- `web-ui-design` — browser specialization: semantics, layout mechanics, responsive
  re-prioritization, forms, tables, scroll and overflow, overlays, component sourcing.
- `mobile-ui-design` — mobile application specialization: touch and gestures, safe areas, software
  keyboard, navigation and presentation, lists, adaptive layout, dynamic text.
- `mobile-ui-ios` — iOS and iPadOS implementation, framework-neutral across SwiftUI and UIKit.
- `mobile-ui-android` — Android implementation, framework-neutral across Compose and Views.
- `mobile-ui-crossplatform` — shared-versus-adaptive decisions for one codebase on both platforms.
- `game-ui-design` — HUD priority, occlusion, input modality, controller focus, resolution
  resilience.
- `game-ui-godot` — Godot implementation: Control and Container layout, Theme, CanvasLayer, focus,
  viewport behavior.
- `ui-review` — audit pass over existing UI: evidence, severity, priority, scoped fixes.
- Plugin packaging: `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`. Skills
  only — no commands, agents, hooks, or MCP servers. No build step.

### Routing

Description-only corrections made before packaging, after a static routing review found four
misroutes:

- `mobile-ui-ios` and `mobile-ui-android` excluded Flutter and React Native outright, which also
  suppressed them for the platform-specific tasks inside those codebases they exist to handle. The
  exclusion is now conditional.
- The same two skills gained the non-visual exclusion that the core and generic mobile skills
  already carried.
- `web-ui-design` now names React Native in its exclusion, since `React` alone appears in its
  positive list.
- `game-ui-design` now covers a game rendered in a browser or on a device, so domain wins over
  rendering technology.
- `game-ui-godot` now excludes non-visual Godot changes.

Routing review after these changes: 28 pass, 2 accepted warnings, 0 failures.
