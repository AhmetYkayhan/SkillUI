# SkillUI

A layered set of Agent Skills for UI/UX work. The core layer holds platform-independent
design rules; platform layers specialize them.

## Layout

```
skills/
├── ui-design-core/      # platform-independent UI/UX decision rules
├── web-ui-design/       # browser specialization
└── mobile-ui-design/    # mobile application specialization
```

Layers:

| Skill | Responsibility |
|---|---|
| `ui-design-core` | Universal UI/UX rules. Available now. |
| `web-ui-design` | Web/browser specialization. Available now. |
| `mobile-ui-design` | Native and cross-platform mobile specialization. Available now. |
| `game-ui-design` | Game engine HUD and menu specialization. |
| `ui-review` | Detailed UI audit and review pass. |

## Design principles

- **Core owns what is universally true.** A rule lives in `ui-design-core` only if it holds
  for web, mobile and game UI alike. Everything else belongs to a platform layer.
- **No duplicated rules.** A platform layer specializes, extends, or overrides the core.
  It never restates it.
- **Small context footprint.** Each skill is an agent behavior specification, not a design
  handbook. Detailed audit rules are deliberately kept out of the core and left to `ui-review`.
- **No invented project facts.** Frameworks, design systems, tokens, device targets and brand
  values are read from the project or asked about — never assumed.

## Install

Copy or symlink a skill directory into your skills directory.

Project scope:

```bash
mkdir -p .claude/skills
cp -R skills/ui-design-core skills/web-ui-design skills/mobile-ui-design .claude/skills/
```

User scope, available in every project:

```bash
mkdir -p ~/.claude/skills
cp -R skills/ui-design-core skills/web-ui-design skills/mobile-ui-design ~/.claude/skills/
```

Install `ui-design-core` alongside any platform layer; the platform layers assume it is present.

## Compatibility

Each skill is a directory containing a `SKILL.md` with YAML frontmatter limited to `name` and
`description`. No non-standard metadata, no custom activation mechanism, and no cross-skill
dependency system is used, so the skills work with any agent runtime that follows the Agent
Skills format.
