---
name: ui-review
description: Use when asked to review, audit, critique, or find problems in an existing interface - "review this screen", "check this design", "what should be improved", a UX, accessibility, consistency, responsive, or design-fidelity review, or a review-and-fix pass over UI that already exists. Not for building new UI, where the design skills apply directly.
---

# UI Review

Audits interfaces that already exist. Answers one question: given this project, the user's goal, the platform, and the existing design system, is this UI correct, usable, consistent, and resilient?

Review is not redesign. Findings must be real problems with evidence, not preferences. The design rules themselves live in `ui-design-core` and the platform skills — this skill governs how to inspect, judge, prioritize, and report against them.

## 1. Scope and depth

Review only the smallest sufficient scope. Match depth to the request:

- **Quick** — one component or region. Two to five highest-value findings.
- **Screen** — one screen. Highest-priority findings, logically grouped.
- **Flow** — several screens plus the transitions between them.
- **Full audit** — only when the user explicitly asks for breadth.

Do not audit screens the user did not name. If something outside scope genuinely affects the reviewed surface, say so briefly; otherwise leave it.

A small implementation request is not a review request. Do not start an audit when the user asked for a padding change.

## 2. Platform context

Determine the actual domain from project evidence, then judge against the matching skill: web → `web-ui-design`, mobile app → `mobile-ui-design`, game → `game-ui-design`. A framework name alone does not decide the domain — a React web app, a React Native app, and a React-rendered game HUD are three different reviews.

Do not restate those rules here; check for violations of them. If the platform is unclear, review against core principles only and raise no platform-specific finding.

## 3. Evidence before criticism

Every finding needs an observable cause: something in the code, the rendered output, or the project's own conventions.

- State the mechanism, not the mood. "The primary action carries the same visual weight as three secondary actions, so the next step is ambiguous" — not "this looks dated".
- Match language to evidence strength. Confirmed: "this causes". Inferred from code alone: "this may clip under enlarged text — verify at runtime".
- Never assert behavior for a state you have not seen. Separate a confirmed visual defect from a suspected one.
- Where output is masked — clipped content, hidden overflow — report the symptom plus the probable cause from layout evidence, and say the cause is probable unless you verified it.

Never invent a numeric standard. Before citing any value, find it in the project's tokens, a verified platform requirement, or an existing convention.

## 4. What is not a finding

- Personal taste: "looks old", "not modern", "needs more personality", "too boring". Only reviewable against a design goal the project or user actually states.
- A missing feature — search, filters, tabs, onboarding, favorites — unless the user's goal on this screen genuinely fails without it.
- A deviation from convention that is intentional: documented, repeated consistently across the project, required by brand or platform, or a real benefit to the task. Check before writing it up.
- A stated user or project preference — minimal, dense, playful, brand-heavy, native-looking — that does not conflict with usability or accessibility.
- Visual similarity between two components. Duplication needs the same semantic responsibility and interaction contract, not just a similar look.
- The mere presence of a card, gradient, badge, or effect. Ask whether it serves a functional or brand purpose; if it does, it is not a defect.

Project consistency and the user's actual task outrank generic reviewer preference. When reviewing your own earlier work, judge the implementation as fresh evidence — neither defend it nor manufacture problems.

## 5. Severity

- **CRITICAL** — the task cannot be completed, or a serious accessibility or interaction failure blocks a user.
- **HIGH** — causes user error, navigation failure, loss of important information, or serious usability cost.
- **MEDIUM** — meaningfully hurts comprehension, consistency, or efficiency.
- **LOW** — polish, minor inconsistency, visual refinement.

Rank an actual access barrier — an unreachable action, invisible focus, an unlabeled critical control, text clipped under accessibility scaling — above cosmetic issues. Speak from evidence rather than claiming compliance or non-compliance with a standard.

Do not inflate severity, and do not manufacture volume. There is no issue quota. If the screen is sound, say so and stop — a short review with two real findings beats twenty padded ones.

## 6. Priority order

```
broken task → information architecture → primary action clarity → navigation and interaction reliability →
accessibility → platform correctness → responsive/adaptive resilience → project consistency → hierarchy → polish
```

Never lead with a shadow or a color tone while a flow-level problem is unreported.

## 7. What to examine

Use only the categories the surface actually raises. Do not comment on all of them, and do not present a long checklist unless the user asked for one.

- **Goal and primary action** — what the user is trying to do; whether the primary action is discoverable and distinct; whether secondary actions compete with it. Some screens are purely informational — do not invent a primary action.
- **Information hierarchy** — important reads as important, related content groups, the scan path holds.
- **Interaction** — what looks interactive is, and vice versa; actions produce feedback; states are distinguishable; the interaction cost is justified; nothing important is hidden behind an extra step.
- **States** — the states this surface can actually reach, given the real data and failure model: loading, empty, error, disabled, selected, success, offline, permission. Do not require states the system cannot produce. For errors: what happened, what the user can do, whether entered work survives.
- **Consistency** — components, tokens, spacing, radius, typography, icons, navigation, and state presentation against the rest of the project. With a design system: bypassed components, one-off values, duplicate variants, ignored semantic tokens, recreated primitives. Without one, do not pretend there is one.
- **Visual weight and noise** — competing focal points, decoration overpowering content, unnecessary nesting, weak critical status, containers that spacing and typography already handle.
- **Typography and spacing** — hierarchy legibility, readable sizes, line length for the context, style-count sprawl; spacing that reflects real grouping rather than differing arbitrarily.
- **Color** — clear state meaning, sufficient contrast, one color not overloaded with meanings, status not carried by color alone, no colors bypassing the system.
- **Content resilience** — long labels, empty values, large numbers, multi-line titles, user-generated content, translation. Judge the risk without inventing domain content.
- **Feedback and loading** — result of each meaningful action is legible; layout does not shift; duplicate submits are prevented; blocking is justified; progress is truthful. Do not prescribe a toast, animation, or haptic for everything.
- **Navigation** — current location, predictable back behavior, clear top-level versus nested structure, discoverable destinations, preserved state. Do not propose a new navigation architecture by default.
- **Overlays** — whether the interruption is justified, whether the content fits the surface, whether dismissal is clear, whether context survives, whether they stack.
- **Input** — the input methods the project actually supports, judged by the platform skill's rules.

Accessibility here is a high-value pass — contrast, meaning beyond color, control labeling, focus and traversal, target usability, text resilience, native semantics — not a compliance audit. A formal audit belongs to a dedicated specialization.

## 8. Collecting evidence

Read the review target first, then nearby components, then relevant tokens and design system, then platform context if needed, then wider repository only if necessary.

Where rendered output exists, use it — hierarchy, density, alignment, clipping, occlusion, focus visibility, and visual noise are visible there and often invisible in code. Do not infer interactions or states a still image cannot show.

Where a runtime or inspection tool is genuinely available, prefer it for problems static review cannot settle: responsive behavior, keyboard overlap, focus navigation, hover and focus states, controller flow, transitions. Never assume a browser tool, design-tool connection, engine tooling, or registry exists — verify, and continue without it. Tools collect evidence; they do not conduct the review.

Check only the representative states, viewports, and input modes the finding requires. Do not sweep twenty widths when three answer the question.

When a design source is available, separate the two questions: **design fidelity** (does the implementation match the source: layout, spacing, typography, states, content order, assets) and **UI quality** (does it work on the real platform). A pixel difference can be a correct adaptation to safe areas, real content, text scaling, or accessibility — check before calling it a defect.

## 9. Reporting

Lead with CRITICAL and HIGH findings. Each substantive finding carries: severity, area, the issue, why it matters, and the recommended correction. Omit severity headings that have no findings, and skip the formal structure entirely for a small review — a few precise sentences are better.

A correction must be actionable and grounded in the project: name the existing component, token, pattern, or platform primitive to use. "Improve spacing" is not a finding; "the gap between the heading and the first field exceeds the gap between sections, so they read as separate groups — use the existing section spacing token" is.

Add a short `Keep` note only when a correct decision is at risk of being broken by the fixes — it protects the right structure while the wrong details change. No obligatory "what works well" section, and no general praise.

## 10. Fixing

Reviewing is not authorization to edit. When the user asked only for a review, recommend; do not change code.

When the user asks to review and fix: inspect, identify the highest-impact issues, apply scoped fixes, verify.

Prefer the smallest correction that resolves the finding:
```
smallest local correction → local structural adjustment → component refactor → larger redesign only if necessary
```

Order fixes by dependency — structure before spacing polish, navigation correctness before transition refinement. Do not recommend low-level polish while the problem underneath it is unfixed.

Before applying a fix, check it does not break responsive or adaptive behavior, alter unrelated variants, or change established interaction semantics. Fixing one issue while introducing two inconsistencies is a failed review.

After fixing, re-verify the affected behavior only. Do not rerun the whole review.
