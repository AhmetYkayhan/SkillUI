# Update workflow

How SkillUI absorbs an external change. The whole point of this file is that steps 1–5 happen
without touching a skill, and step 6 is a person.

```
CHECK → FILTER → CLASSIFY → COMPARE → PROPOSE → USER APPROVAL → PATCH → TEST → CHANGELOG → VERSION
```

## 1. CHECK

Read the sources in `UPDATE_SOURCES.md`. Nothing else. Start with Claude Code, then only the
domains SkillUI actually implements today.

The default window is *since the last successful audit*, taken from `AUDIT_HISTORY.md`. On a first
run, pick a reasonable recent window and say which one you picked.

Do not sweep the ecosystem. Go deep only where a source shows a real signal.

## 2. FILTER

Drop anything that cannot change how a skill behaves. Most release notes are noise for this
purpose: bug fixes, compiler and tooling work, performance internals, dependency bumps, backend
changes.

## 3. CLASSIFY

Each surviving item gets a domain and an impact.

Domain: `ROUTING`, `CORE-DESIGN`, `WEB`, `MOBILE-GENERIC`, `IOS`, `ANDROID`, `CROSSPLATFORM`,
`GAME`, `GODOT`, `REVIEW`, `FRAMEWORK-ONLY`, `TOOLING`, `DOCUMENTATION-ONLY`, `NO-ACTION`.

Impact:

| Level | Meaning |
|---|---|
| `CRITICAL` | Makes current SkillUI behavior wrong or incompatible — e.g. Claude Code changes skill discovery or the plugin format. |
| `HIGH` | Major platform behavior change that makes an existing rule wrong. |
| `MEDIUM` | New guidance or pattern that would meaningfully improve a skill. |
| `LOW` | Polish, wording, a clearer phrasing of something already true. |
| `NONE` | Real information, no skill consequence. |

Two filters that catch most false positives:

**Newer is not better.** An item only becomes a patch candidate if it is authoritative, relevant,
stable, and actually behavior-changing. Recency alone is not an argument.

**A new API is not a new design rule.** Ask whether the UI *decision* changed. If only the
implementation surface moved and the skill can stay framework-neutral, it is `FRAMEWORK-ONLY` or
`NO-ACTION`.

## 4. COMPARE

Put the new guidance next to the current rule and pick one:

- current rule still correct
- current rule partially outdated
- current rule now conflicts with the source
- rule missing that would be genuinely useful
- implementation-only change, no rule involved

Before proposing anything new, check whether the behavior is already covered in the core, the
platform layer, or a specialization. Do not add a duplicate rule. If a new rule supersedes an old
one, they do not both survive — the patch replaces.

## 5. PROPOSE

For each candidate:

```
Affected skill
Current behavior
New evidence (source + what it says)
Why the change is needed
Minimal proposed patch
Expected routing / context impact
Tests required
```

Nothing is written to a skill at this stage.

## 6. USER APPROVAL — hard gate

**An audit never edits a `SKILL.md`.** Findings are presented; the user decides. There is no path
where an external document changes SkillUI on its own. This is what keeps the library deterministic.

## 7. PATCH

Once approved, apply the **smallest sufficient patch**. Do not rewrite a skill because one rule
moved. Keep unrelated accepted updates in separate commits so each one stays a reversible diff.

Extra care for `ui-design-core` — it is in context for every UI task. Before a rule lands there:

- Could it live in a specialization instead?
- Is it true for web *and* mobile *and* game?
- Does it raise the cost of every UI task?

If any answer is uncomfortable, it belongs in a layer, not the core.

Resist reflexive splitting too. A skill growing is not by itself a reason to spawn a specialization
— ask whether the need is recurring, whether it is framework-specific, and whether keeping it here
actually wastes tokens. Behavioral density matters more than line count.

Before a new rule lands anywhere, one more question: is this worth carrying in context on every
task that touches this layer? If not, it is a reference, a future specialization, or a no-action.

## 8. TEST

**If `description`, activation scope, a domain boundary, or platform escalation changed** — run the
full routing suite: 30 primary cases plus the 6 regression cases. Routing is the part that breaks
silently.

**If only a body rule changed** — a targeted behavioral review plus the relevant platform cases is
enough, but confirm the description is genuinely untouched before skipping the full suite.

## 9. CHANGELOG

Record the change in `CHANGELOG.md`. Routing-affecting entries are marked as such, because they
change runtime behavior rather than guidance.

## 10. VERSION

Bump the version in `.claude-plugin/plugin.json` when a change ships, then append the audit result
to `AUDIT_HISTORY.md`.

## Rollback

Every accepted update is one identifiable, reversible diff. Do not merge unrelated guideline
updates into a single large patch — that trades away the ability to revert one of them.

## Running an audit

Hand `UPDATE_AUDIT_PROMPT.md` to Claude in this repository. There is no slash command and no
scheduled job on purpose: maintenance is manual, on demand, and carries no runtime context cost
during ordinary UI work.
