# SkillUI

A layered set of Agent Skills for UI/UX work, packaged as a Claude Code plugin. The core layer
holds platform-independent design rules, platform layers specialize them, and engine or OS layers
specialize those further. Each layer loads only when the task calls for it.

## Included skills

| Skill | Responsibility |
|---|---|
| `ui-design-core` | Universal UI/UX rules: hierarchy, layout, spacing, typography, states, accessibility principles. |
| `web-ui-design` | Browser specialization: semantics, layout mechanics, responsive behavior, forms, tables, overlays. |
| `mobile-ui-design` | Mobile application specialization: touch, safe areas, keyboard, navigation, adaptive layout. |
| `mobile-ui-ios` | iOS and iPadOS implementation: SwiftUI or UIKit, presentation, Dynamic Type, iPad adaptation. |
| `mobile-ui-android` | Android implementation: Compose or Views, insets, back behavior, IME, Material theming. |
| `mobile-ui-crossplatform` | Shared-codebase decisions: what stays shared, what adapts, when a platform branch is justified. |
| `game-ui-design` | Game specialization: HUD priority, occlusion, input modality, controller focus, resolution resilience. |
| `game-ui-godot` | Godot implementation: Control and Container layout, Theme, CanvasLayer, focus, viewport behavior. |
| `ui-review` | Audit pass over UI that already exists: evidence, severity, priority, scoped fixes. |

## Routing model

```
ui-design-core
├── web-ui-design
├── mobile-ui-design
│   ├── mobile-ui-ios
│   ├── mobile-ui-android
│   └── mobile-ui-crossplatform
├── game-ui-design
│   └── game-ui-godot
└── ui-review
```

**This is a conceptual routing model, not an inheritance mechanism.** Claude Code has no skill
inheritance or dependency system, and this plugin does not add one. Each skill is discovered
independently through its own `description`; the tree describes how the rules are divided, not how
they load. What keeps the layers from overlapping is that the core owns what is universally true
and every other layer states what it is *not* for.

Typical combinations:

| Task | Layers that apply |
|---|---|
| SwiftUI or UIKit screen | core + mobile + ios |
| Jetpack Compose or Views screen | core + mobile + android |
| Flutter or React Native screen | core + mobile + crossplatform |
| Flutter screen, iOS-specific bug | core + mobile + crossplatform + ios |
| React or Next.js dashboard | core + web |
| Godot HUD or menu | core + game + godot |
| Unity or Unreal UI | core + game |
| "Review this screen" | the layers above + review |
| Non-visual logic, any stack | none |

## Install

The plugin is a directory of Markdown skills. There is no build step.

Load it from a local clone for the current session:

```bash
claude --plugin-dir /path/to/SkillUI
```

Inspect what it contributes, including its token cost:

```bash
claude --plugin-dir /path/to/SkillUI plugin details skillui
```

For a persistent install, this repository also ships `.claude-plugin/marketplace.json`, so it can
be added as a plugin marketplace source and installed through Claude Code's `/plugin` interface in
an interactive session. Run `claude plugin --help` for the management commands your version
supports — `enable`, `disable`, and `details` operate on installed plugins.

## Token cost

Only the nine `description` lines stay in context; a skill's body loads when that skill actually
fires. Measured with `claude plugin details skillui`:

- Always-on: roughly 880 tokens for the whole plugin
- Per skill when it fires: roughly 1.7k–4.7k tokens

A typical task loads two or three layers, not nine.

## Design principles

- **Core owns what is universally true.** A rule lives in `ui-design-core` only if it holds for
  web, mobile and game UI alike. Everything else belongs to a platform layer.
- **No duplicated rules.** A platform layer specializes, extends, or overrides the core. It never
  restates it.
- **Small context footprint.** Each skill is an agent behavior specification, not a design
  handbook. Detailed audit rules are deliberately kept out of the core and left to `ui-review`.
- **No invented project facts.** Frameworks, design systems, tokens, device targets, platform
  versions and brand values are read from the project or asked about — never assumed.
- **Tooling is optional.** No skill requires an MCP server, a design-tool connection, or a runtime
  inspector. Where one exists it is used as evidence; where it does not, the work continues.

## Compatibility

Each skill is a directory containing a `SKILL.md` with YAML frontmatter limited to `name` and
`description`. No non-standard metadata, no custom activation mechanism, no loader, and no
cross-skill dependency system is used. The skills work with any agent runtime that follows the
Agent Skills format; the plugin manifest is what makes them installable in Claude Code.

The plugin contributes skills only — no commands, agents, hooks, or MCP servers.

## Maintenance

Update tracking lives in `maintenance/` and never loads during normal use — it is documentation in
the repository, not a skill.

- `UPDATE_SOURCES.md` — the bounded list of authoritative sources SkillUI watches.
- `UPDATE_WORKFLOW.md` — how an external change becomes a patch.
- `UPDATE_AUDIT_PROMPT.md` — hand this to Claude in this repository to run an audit.
- `AUDIT_HISTORY.md` — one line per run.

**An audit never edits a skill.** It researches, classifies, compares against the current rules,
and proposes. Applying anything is a separate, explicit decision. That is what keeps the library
deterministic instead of drifting with whatever documentation changed this month.

## Status

Version 0.1.0, initial packaged release. The nine skills have been through a static routing review
(28 pass, 2 accepted warnings, 0 failures); skill composition may still evolve.

If you already installed these skills by copying them into a skills directory, remove those copies
after installing the plugin — otherwise each skill is present twice.

No `LICENSE` file is present in this repository yet.
