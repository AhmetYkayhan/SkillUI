# Tracked sources

What SkillUI watches, and what it deliberately ignores. This registry exists so an update audit
checks a bounded list instead of sweeping the ecosystem.

**URLs are site roots only.** Deep documentation links churn faster than the guidance behind them,
so navigate from the root rather than trusting a path recorded here. If a root below turns out to
have moved, correct it in this file rather than guessing at a replacement during an audit.

## Tier 1 — authoritative

These can justify a skill change on their own.

### Claude Code — highest priority

`docs.claude.com` · release notes · `claude plugin --help`

Affects: the whole architecture, potentially every skill.

Watch for: skill file format and frontmatter fields; description and discovery behavior; plugin
manifest format; plugin install, enable, and namespacing behavior; how skill bodies are loaded into
context; commands, hooks, agents, and MCP integration; any deprecation of the above.

A change here can invalidate the packaging or the routing model itself, so it is checked first in
every audit even when no platform has moved.

### Apple

`developer.apple.com` — Human Interface Guidelines and developer documentation

Affects: `mobile-ui-design`, `mobile-ui-ios`.

Watch for: navigation and presentation guidance; sheet and modal behavior; Dynamic Type and
accessibility guidance; safe-area and system-chrome expectations; new or retired system UI
conventions; explicitly deprecated interaction guidance.

Ignore: new APIs that express existing guidance differently. A new modifier is not a new design
rule.

### Android

`developer.android.com` · `m3.material.io`

Affects: `mobile-ui-design`, `mobile-ui-android`.

Watch for: system back behavior; window inset and edge-to-edge expectations; IME handling guidance;
large-screen and window-size guidance; Material component semantics where they change behavior, not
styling; permission UX changes tied to platform versions.

Ignore: Material styling refreshes with no behavioral consequence.

### Flutter

`docs.flutter.dev` · release notes

Affects: `mobile-ui-crossplatform`, occasionally `mobile-ui-design`.

Watch for: platform-adaptation best practice; changes to how shared and platform-specific UI are
expected to be separated.

Ignore: widget API additions, tooling, compiler and performance work.

### React Native

`reactnative.dev` · release notes

Affects: `mobile-ui-crossplatform`.

Watch for: platform-adaptation guidance; changes to the recommended shared-versus-native boundary.

Ignore: architecture internals, bridge and renderer work, dependency churn.

### Godot

`docs.godotengine.org` · release notes

Affects: `game-ui-godot`, occasionally `game-ui-design`.

Watch for: Control and Container layout behavior; Theme and StyleBox system changes; focus and
input handling; viewport, stretch, and scaling; UI-related deprecations and renames across major
versions.

Ignore: physics, rendering, scripting, and gameplay systems.

### Web platform

`developer.mozilla.org` · browser release notes where a behavior actually ships

Affects: `web-ui-design`, occasionally `ui-design-core`.

Watch for: browser-native UI behavior; HTML semantics; CSS layout capabilities that change what the
correct structural answer is; responsive and adaptive behavior; accessibility and interaction
platform behavior.

Ignore: a new JavaScript framework release. Framework version numbers are not design guidance.

## Tier 2 — optional, only when relevant

Check these only if SkillUI grows a specialization that needs them, or the user asks:

- Official component-library documentation and registries (for example shadcn) — relevant only if a
  `web-ui-shadcn`-style layer exists.
- Framework-native component library documentation, same condition.
- Official accessibility specifications, when an audit or accessibility specialization needs a
  citation rather than a principle.

## Not sources

These never justify a skill rule on their own:

- Design blogs, social posts, Dribbble, Behance, "UI trends" roundups, SEO design articles.
- Anything phrased as a trend rather than as platform or usability guidance.

They may be useful when the user explicitly asks for trend research. That is a separate request,
not maintenance.

## Source conflict

If two Tier 1 sources point in different directions, do not quietly reconcile them. Record it as a
`SOURCE CONFLICT` in the audit output, state what each says, and leave the decision to the user.
