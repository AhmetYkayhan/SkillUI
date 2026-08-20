---
name: ui-design-core
description: Use when designing, implementing, restyling or restructuring any user interface - screens, layouts, components, navigation, spacing, typography, hierarchy, interaction states, empty/loading/error states - on any platform. Load before writing or changing UI code. Not for backend, data, or non-visual logic.
---

# UI Design Core

Platform-independent UI/UX decision rules. Governs *what* the interface should be, not *how* a framework expresses it.

## Scope

Owns: information hierarchy, layout, spacing, typography, color usage, contrast, density, affordance, feedback, states, progressive disclosure, consistency, accessibility principles.

Does not own: framework syntax, component library APIs, platform APIs, animation libraries, build config, backend, business logic, game mechanics.

## 1. Context before design

Never assume framework, design system, tokens, brand colors, fonts, component library, device target, screen size, breakpoints, or navigation model.

For each unknown that actually changes the decision:
1. Look for evidence in the project (task-related files first).
2. Use the evidence if found.
3. Ask the user if not found.

If the decision can be made from a platform-independent principle, decide and continue. Do not interrupt for information you do not need.

Read order — stop as soon as you have enough:
`task files` → `nearby components` → `design system / tokens` → `wider repository`

Never scan the whole repository by default.

## 2. Platform layer

Platform-specific rules live in separate skills (`web-ui-design`, `mobile-ui-design`, `game-ui-design`). Core rules stay valid on every platform; a platform skill specializes or overrides them, and its rule wins on direct conflict.

Determine platform from concrete project evidence — config files, project files, source language, dependency manifests, or an explicit statement from the user. A single weak signal is not evidence.

If no platform skill is loaded, or platform is unclear: apply core rules only, make no platform-specific assumptions, and do not invent rules on behalf of an absent skill.

## 3. Effort scaling

Use the smallest sufficient design process.

- Local change (spacing, label, one prop, one variant): apply the relevant rule, edit, stop.
- New screen, new flow, or structural redesign: run the full workflow below.

Do not produce design analysis that the task does not require.

## 4. Workflow (structural work only)

**UNDERSTAND** — screen purpose, primary user goal, primary action, secondary actions, information that must be visible, existing project patterns.

**REUSE** — search existing components, tokens, layouts, variants, and interaction patterns. Reuse or extend before creating. A near-duplicate component is a defect.

**STRUCTURE** — resolve information architecture, content priority, grouping, layout, and visual hierarchy. No styling yet.

**DESIGN** — spacing, typography, color, affordance, feedback, states.

**IMPLEMENT** — follow the project's existing conventions; defer platform mechanics to the platform skill.

## 5. Decision priority

```
project context → existing consistency → user goal → hierarchy → accessibility → visual polish
```

Later items never override earlier ones. Visual novelty is last, always.

Design layer order:
```
information architecture → layout → hierarchy → spacing → typography → interaction → styling → decoration
```

A weak layout is fixed by restructuring, never by gradients, shadows, animation or decoration.

## 6. Consistency and tokens

- Reuse existing tokens. Do not invent a value that already exists semantically.
- Prefer semantic tokens over raw values.
- Keep spacing and radius systematic. A one-off value needs a concrete reason.
- Do not introduce a new scale, palette, or component variant when one exists.
- Do not redesign an established project convention without a concrete reason.

## 7. Intentional UI

Every significant element must serve at least one of: hierarchy, navigation, grouping, feedback, interaction, status, readability, brand. Elements serving none are removed, not styled.

Express grouping with spacing, alignment, typography, dividers, or background before adding a container. Add a card only when those are insufficient.

## 8. Anti-patterns — do not

- Wrap every section in a card.
- Use gradients, glow, blobs, or glassmorphism without a stated purpose.
- Stack excessive rounded containers or shadows.
- Use arbitrary one-off border-radius values.
- Use emojis as interface icons unless the product explicitly calls for it.
- Center-align large blocks of functional content.
- Put oversized marketing headings inside product screens.
- Invent design tokens, colors, or spacing scales that the project already defines.
- Ship inconsistent variants of the same component.
- Hide a primary action behind an extra interaction.
- Trade usability for visual novelty.
- Add visual complexity to make the UI look "designed".

## 9. Interaction states

When building an interactive element, design its real states, not only the ideal one. Consider: default, hover, focus, pressed, selected, disabled, loading, success, error, empty.

Include only states the platform and the component actually have. Do not force a state that does not exist on the target platform.

Every list, table, or data surface needs a defined empty state and error state before it ships.

## 10. Accessibility (principles)

- Sufficient text and UI contrast.
- Never communicate state or meaning by color alone.
- Visible focus and clear interactive affordance.
- Adequate target size for the input method in use.
- Meaningful labels for every control; icon-only controls need an accessible name.
- Errors state what happened and what to do.

Platform accessibility APIs belong to the platform skill.

## 11. Boundaries

Detailed UI audits belong to a separate review skill. Here, apply only the quality rules needed while producing the UI.

Do not modify backend, data, or business logic unless the UI task genuinely requires it.
