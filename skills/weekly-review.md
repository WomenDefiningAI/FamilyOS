# Weekly OS Review — Scheduled Task Prompt

**How to use:** Copy everything below the line and paste it as the prompt when creating the "Weekly OS Review" scheduled task in Cowork. Set frequency to Weekly on Sunday at 6:00 PM.

**Action trust:** see `workspace/TOOLS.md` → Action trust levels. The proposal-counting step below reads `workspace/audit-ledger.md` to determine which H4 action classes have accumulated enough confirmations-without-edits to surface as H4→H5 promotion proposals.

---

Read SOUL.md, USER.md, MEMORY.md, OS-LOG.md, TOOLS.md, and audit-ledger.md from the FamilyOS workspace folder.

This is the weekly FamilyOS review. Your job is to look at how the system performed this week, identify what could be better, and propose specific improvements. You are reviewing the OS itself — not just household tasks.

---

**Step 1: Gather this week's signals**

Read LOG.md and MEMORY.md entries from the past 7 days. Collect:

- All items tagged `[NEEDS-REVIEW]` — things the inbox job couldn't confidently classify
- All items tagged `[NEEDS-APPROVAL]` — actions that were held for human confirmation
- Any items appearing 3 or more times this week (recurring topics or requests)
- Any items in MEMORY.md marked as pending or unresolved for more than 7 days
- Any patterns in timing — things that consistently come up on the same day of week

**Step 1b: Per-class confirmation counter (for H4→H5 promotion proposals)**

Read `workspace/TOOLS.md` → Action trust levels. For every row whose H-level is exactly `H4` (not `H5`, not `H4-forever`), capture the `Action class` and the `Since` date.

For each such class, count **confirmations-without-edits** since `Since`:

1. Read `workspace/audit-ledger.md` "Active rotation window" — durable record of `[AUTO-APPLIED]` entries rotated by memory-consolidation. Also read `workspace/LOG.md` for entries that haven't been rotated yet (today's entries). Both are valid sources.
2. Filter: lines tagged `[AUTO-APPLIED]` whose `class:` matches the H4 class AND whose timestamp is after the row's `Since` date.
3. Subtract any matching `[AUTO-APPLIED-REVERTED]` entries — a reverted action does not count as a confirmation.
4. Edit signal: if the user directly edited the `file:` named in an AUTO-APPLIED entry within 24 hours of that entry's timestamp (use `git log --follow` if the worktree is git-tracked, or file mtime as fallback), reset the count for that class to zero — Danielle's edit signals the auto-action would have been wrong.

The result is `confirmations` per class.

**Promotion threshold (tunable):** when `confirmations >= 6`, the class is eligible for promotion. The threshold is conservative: ≈6 weeks for weekly-cadence skills, ≈1 week for daily-cadence skills. If first quarter post-launch shows >2 promotion proposals per cycle, raise this threshold by editing it here.

Exclude classes whose H-level is `H4-forever` — they are never eligible for promotion regardless of track record. Demotion (H5 → H4 or H5 → H4-forever) is **not** automated; if a class is producing wrong actions, Danielle edits TOOLS.md directly.

---

**Step 2: Assess the week**

Score each area briefly (one sentence each):

- **Inbox filing accuracy** — did items land in the right folders, or were there frequent misfires?
- **Morning brief usefulness** — based on what actually mattered this week, was the brief hitting the right things?
- **Memory quality** — is MEMORY.md growing with genuinely useful facts, or filling up with noise?
- **Unresolved items** — anything that fell through the cracks or sat unresolved too long?

---

**Step 3: Generate improvement proposals**

For each signal or pattern found in Step 1 or Step 1b, propose a specific, actionable OS change. Number proposals sequentially starting at 1. Use this format:

```
**Proposal N: [short title]**
- Signal: [what you observed]
- Change: [exactly what to update — which file, which field, what to add/change]
- Type: [USER.md update / skill prompt update / new skill / new scheduled task / working habit / trust-gradient promotion]
- Auto-apply: [yes if it's a text edit to existing files in workspace/ or skills/; no if it requires creating scheduled tasks, sending external messages, or deleting files]
```

Examples of good proposals from regular signals:
- "Shopping list has 'oat milk' added 4 times this week → add as a standing weekly item in meal-planner.md"
- "ClassDojo messages keep hitting [NEEDS-REVIEW] → add ClassDojo to the school sender list in school-triage.md"
- "Pickup schedule changed for Wednesdays based on 3 separate notes → update USER.md Wednesday entry"
- "You asked for a helper payment reminder twice → add a Friday standing check to morning-brief.md"

**For Step 1b promotion candidates** (H4 classes with `confirmations >= 6`), use this exact form:

```
**Proposal N: Promote `<class>` to H5**
- Signal: <N> confirmations of `<class>` without edits since <Since-date>
- Change: edit workspace/TOOLS.md Action trust levels — change the `<class>` row's H-level from H4 to H5, and update its Since column to today's date.
- Type: trust-gradient promotion
- Auto-apply: yes
```

Apply this verbatim — it's a text edit to TOOLS.md, so the existing `apply N` directive in slack-inbox handles it. After promotion, future actions of `<class>` auto-apply via the trust ledger and emit `[AUTO-APPLIED]` LOG entries.

If a class is missing from TOOLS.md but appears in `[AUTO-APPLIED]` entries (orphan annotation), surface a separate proposal: "Add missing class `<class>` to TOOLS.md trust ledger" with `Auto-apply: no` (manual setup — needs a deliberate H-level decision).

Keep proposals concrete. Vague suggestions ("improve memory") are not useful.

---

**Step 4: Write to OS-LOG.md**

Append a dated entry to OS-LOG.md in this format:

```
## Week of [YYYY-MM-DD]

### Week summary
[2–3 sentences on how the system performed]

### Signals found
- [NEEDS-REVIEW items]: [count and brief description]
- [Recurring topics]: [list]
- [Unresolved from prior weeks]: [list]

### Proposals
[Number each proposal sequentially using the Step 3 format above]

### Applied
[Leave blank — populated automatically when the user replies to the Slack post with `apply N`, or manually if they apply something themselves]
```

---

**Step 5: Post a summary to Slack**

Post a brief message to the household inbox channel listed in TOOLS.md:

```
📋 *Weekly FamilyOS Review — [DATE]*

[1–2 sentence summary of how the week went]

*Proposals:*
1. [Proposal 1 — one line]
2. [Proposal 2 — one line]
3. [Proposal 3 — one line]

Reply `apply 1,3` to act on specific items, `apply all`, or `skip`. Full detail (and any items needing manual setup) in OS-LOG.md.

audit ref: [YYYY-MM-DD] / weekly-review/post-summary
```

Keep it short. The full detail is in OS-LOG.md. This Slack message is the user's prompt to act.

---

**Rules:**
- Propose changes. Do not apply them automatically.
- The user decides what to act on and when.
- If there's nothing worth proposing this week, say so clearly rather than inventing suggestions.
- Honest assessment beats positive spin. If something isn't working, name it.
