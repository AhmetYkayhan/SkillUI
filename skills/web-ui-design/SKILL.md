---
name: web-ui-design
description: Use when designing or changing UI that runs in a browser - web apps, dashboards, admin panels, landing pages, forms, tables, navigation, responsive layout, or component styling in React, Next.js, Vue, Svelte, Angular, or plain HTML/CSS. Not for native mobile UI (including React Native and other shared-codebase mobile UI), game UI, or non-visual frontend logic.
---

# Web UI Design

Browser specialization of `ui-design-core`. Core rules still apply and are not repeated here. This file answers one question: how does a general UI principle behave in a browser?

## Applies when

The interface is rendered by a browser. A narrow viewport is still web — responsive layout does not make it native mobile UI. If the project is a native app, use the mobile skill; if the surface is a game HUD, menu, or inventory, use the game skill, regardless of which web framework renders it.

Stay framework-neutral. Follow the project's existing framework, styling system, and component library. Do not add a dependency to solve one component.

Insert into the core priority ladder: browser semantics and responsive integrity outrank visual polish, and rank below existing project conventions.

## 1. Semantics

Behavior determines the element, appearance does not.

- Action → `button`. Navigation → link. Never swap them for styling reasons.
- A clickable `div`/`span` is not a default. Use it only when no native element fits, and then restore role, focus, and keyboard behavior yourself.
- Use `label` bound to its control, real headings for document hierarchy, `nav`/`main` for regions, `form` for form submission, `table` for tabular relationships.
- Native element first, ARIA only when no native element expresses the meaning. ARIA does not replace a missing element and does not add behavior.
- Custom appearance must not cost native behavior. Before hand-rolling a select, dialog, or disclosure, use the native element or the project's existing accessible primitive.

## 2. Layout mechanics

Prefer, in order: normal document flow → flex-style layout → grid-style layout. Use the project's existing layout primitives before writing new ones.

- One-dimensional alignment or distribution → flex. Two-dimensional row/column structure → grid. Do not reach for grid because it is available.
- Absolute positioning is for overlays and anchored elements, not page structure. Never build layout from hardcoded coordinates.
- Do not put fixed pixel heights around text or dynamic content.
- Do not compute layout in JavaScript when CSS can express it.

Pick a width strategy from the screen's purpose: constrained reading width, full-width workspace, split layout, or dense data canvas. Do not apply one universal max-width, and do not make every screen full-bleed.

## 3. Responsive behavior

Responsive is re-prioritizing, not shrinking. In order:

```
preserve content priority → reflow → reorder → collapse secondary UI → reduce density → hide (last resort)
```

- Never compress a desktop layout into a narrow viewport. Stack, collapse, or reorder instead.
- Preserve the primary action and essential information at every width.
- Avoid horizontal compression that produces tiny controls or unreadable columns.
- Layout must stay correct while the user resizes, not only at load width.

Breakpoints: use the project's existing scale. Add one only where the layout actually breaks, not because a device is named. Do not accumulate breakpoints.

Keep source order aligned with reading order and keyboard order. Do not use CSS to visually reorder content into a sequence the DOM contradicts.

## 4. Keyboard, focus, pointer

- Every interactive element is reachable and operable by keyboard, with the key behavior its role implies.
- Focus must remain perceivable. Restyle the focus indicator if needed; never remove it.
- Focus order follows the interaction and document order. Opening an overlay moves focus into it; closing returns focus to the trigger.
- Hover is enhancement only. Actions, labels, meaning, and status must be reachable without hover — browsers run on touch devices too.
- Make the interactive target the whole control, not just the icon glyph, and keep adjacent targets from overlapping.

## 5. Forms

- Persistent visible labels. A placeholder is not a label.
- State required or optional explicitly; help text sits with the field.
- Validation messages appear at the field, say what is wrong and what to do, and never discard what the user typed.
- Keep tab order logical; make the submit state (idle, submitting, failed) visible.
- Do not invent domain or backend validation rules.

## 6. Tables and dense data

Use real table semantics or the project's accessible data-table primitive for tabular data.

At narrow widths choose deliberately: horizontal scroll, column priority, hiding low-value columns, or row expansion. Do not shrink every column uniformly, and do not turn a comparison table into a card list — that destroys the relationship the table exists to show.

## 7. Scroll, overflow, sticky

- The page scrolls. Do not block or hijack it to feel "app-like".
- Minimize nested scroll regions; scroll ownership must be obvious. Page → panel → table → modal scrolling stacked together is a defect.
- Fix horizontal overflow at its cause — a fixed width, a non-wrapping string, a missing `min-width: 0`, a bad track, a positioned child. Do not mask it with `overflow: hidden`.
- Sticky and fixed elements must earn the viewport they consume. Check obstruction, remaining viewport at short heights, keyboard interaction, and stacking. Do not layer several of them.
- Treat viewport-height units carefully: browser chrome and mobile viewport changes move them. Follow the project's existing approach and verify content can still overflow normally.

## 8. Real content

Design against realistic content, not placeholder text: longest realistic labels, translated strings, empty values, large numbers, multi-line titles, URLs, dynamic status text. Decide what the layout does when text grows.

Do not fabricate product content — menu items, metrics, plans, roles, filters, or features. Use existing project or domain content; ask if it is required and absent.

## 9. Loading and stability

Loading is a layout decision, not just a spinner. Depending on the existing data flow: keep the layout stable, show a skeleton matching the real shape, keep prior content while refreshing, show progress for long work, and prevent duplicate submits.

Reserve space for async content and media so nothing jumps. Give images and embeds a known aspect ratio and responsive sizing; use the project's existing media primitive. Do not add imagery to fill space.

Do not invent networking, caching, or retry behavior.

## 10. Overlays

- Use a modal only when the task genuinely needs an isolated context. Editing and creation do not require one by default.
- A modal needs a clear title, one clear primary action, an obvious dismissal, correct focus handling, and its own scroll behavior. Do not compress a long workflow into one.
- Popovers, dropdowns, and tooltips are for short-lived, anchored, secondary interactions. Do not host a primary workflow in one, and never put critical information behind a hover-only surface.

## 11. Components and design systems

Create a component when there is a real boundary: reused behavior, a reused visual contract, a meaningful semantic unit, or independent state. Do not split markup into components only to shorten a file, and do not build a monolith either.

Extend an existing component when it stays coherent. If extending would cause a props or boolean explosion, break its semantics, or bolt on an unrelated responsibility, a separate component is the better answer. Visual similarity alone is not a reason to merge.

If the project uses a component system, compose its existing primitives instead of recreating them — and adapt them to the project's tokens and visual language rather than shipping library defaults unexamined.

Component sourcing priority:
```
existing project component → project design system → configured registry or MCP → custom implementation
```

Never assume a registry, component MCP, or design-tool MCP exists. Verify it is actually available in the session before using it, and continue normally without it. Anything pulled from an external source must be checked against project conventions, tokens, semantics, accessibility, and existing dependencies. An external source supplies components; it does not make design decisions.

## 12. Product screens vs marketing pages

Keep them distinct. In an application or dashboard, prioritize task completion, scanability, state clarity, actions, and data. Marketing sections, hero blocks, and decorative feature cards are not application defaults.

On a marketing page, information architecture and the actual product message still come before decoration. Do not emit the generic AI landing page by default: giant gradient headline, glowing CTA, three-card feature row, floating glass panels, decorative blobs, invented logos, invented metrics, invented testimonials, or filler badges. Each requires a real reason and real content.

## 13. Motion

Motion needs a function: show a relationship, confirm an action, or orient the user during a transition. Continuous decorative movement is not a default. Respect a reduced-motion preference where the project supports it.

## 14. Web anti-patterns — do not

- Squeeze a desktop layout into a narrow viewport.
- Build fake buttons from `div`/`span` with a click handler.
- Put a critical control behind hover only.
- Set fixed heights around dynamic text.
- Use absolute positioning as the page's structural system.
- Add breakpoints per device name until the scale is noise.
- Hide horizontal overflow instead of fixing it.
- Nest scroll regions without need.
- Solve layout in JavaScript that CSS already solves.
- Add a UI library for a single component.
- Recreate a primitive the project's design system already ships.
- Add a hamburger menu reflexively without deciding what the navigation should become.
- Use ARIA to patch a missing native element.

## 15. Effort scaling in web work

Scale investigation to the task — spacing, radius, or a single label needs the target component and its immediate responsive effect, nothing more.

- New screen or dashboard: layout, component reuse, responsive behavior, semantics.
- New navigation: hierarchy, wide and narrow behavior, keyboard, focus.
- Structural change: sanity-check narrow, medium, and wide viewports against the project's existing breakpoints. Do not produce a full responsive audit for every task.

Detailed UI and accessibility audits belong to a review skill, not here.
