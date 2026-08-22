# SkillUI update audit

Paste the section below to Claude, in this repository, when you want to check whether anything
outside SkillUI has moved. It is a research and reporting task — it does not change the library.

---

Run a SkillUI update audit.

**You must not modify any skill.** No `SKILL.md`, no description, no plugin manifest, no routing.
This task produces a report and nothing else. If you conclude a patch is warranted, propose it and
stop — the decision is mine.

**Research, do not recall.** This task depends on current information. Use web access if it is
available in this session. If it is not, say so explicitly and report what you could not verify
rather than answering from memory. Never state that a platform changed something unless you found
it in a source during this run.

**Scope.** Read `maintenance/UPDATE_SOURCES.md` and check only what it lists. Start with Claude
Code, then the domains SkillUI actually implements. Go deeper only where a source shows a real
signal — this is a bounded check, not an ecosystem sweep.

**Window.** Read `maintenance/AUDIT_HISTORY.md` and check the period since the last successful
audit. If there is none, choose a reasonable recent window and state which one you chose.

**Method.** Follow `maintenance/UPDATE_WORKFLOW.md` steps 1–5: check, filter, classify, compare,
propose. Stop before step 6.

Apply its two filters honestly. Newer is not automatically better — an item matters only if it is
authoritative, relevant, stable, and behavior-changing. And a new API is not a new design rule; ask
whether the UI decision changed, not whether the implementation surface moved.

Before proposing a rule, check whether the behavior is already covered in `ui-design-core`, the
relevant platform layer, or a specialization. Do not propose a duplicate. If new guidance
supersedes an existing rule, say so — the proposal replaces, it does not stack.

Weigh every candidate against the token budget: is this worth carrying in context on every task
that touches this layer?

**Trends are not sources.** A design trend is not a reason to change a skill. Platform guidance and
usability evidence are.

**Conflicts.** If two authoritative sources disagree, report `SOURCE CONFLICT` with what each says.
Do not reconcile them silently.

**No forced findings.** `NO CHANGE` is a correct and common result. Do not manufacture work to make
the audit look productive.

Report in this shape:

```
Checked period
Sources checked

CRITICAL updates
HIGH updates
MEDIUM updates
No-action updates

Affected SkillUI skills

Recommended patches
  Affected skill
  Current behavior
  New evidence
  Why the change is needed
  Minimal proposed patch
  Expected routing / context impact
  Tests required

Routing regression needed?  Yes / No

Recommendation:
  NO CHANGE
  PATCH RECOMMENDED
  ARCHITECTURE REVIEW REQUIRED
```

Then append a one-line entry to `maintenance/AUDIT_HISTORY.md` recording the date, the period
checked, the result, and that no changes were applied. That file is the only thing you may write.
