# Weekly OS Review — Scheduled Task Prompt

**How to use:** Copy everything below the line and paste it as the prompt when creating the "Weekly OS Review" scheduled task in Cowork. Set frequency to Weekly on Sunday at 6:00 PM.

---

Read SOUL.md, USER.md, MEMORY.md, and OS-LOG.md from the FamilyOS workspace folder.

This is the weekly FamilyOS review. Your job is to look at how the system performed this week, identify what could be better, and propose specific improvements. You are reviewing the OS itself — not just household tasks.

---

**Step 1: Gather this week's signals**

Read LOG.md and MEMORY.md entries from the past 7 days. Collect:

- All items tagged `[NEEDS-REVIEW]` — things the inbox job couldn't confidently classify
- All items tagged `[NEEDS-APPROVAL]` — actions that were held for human confirmation
- Any items appearing 3 or more times this week (recurring topics or requests)
- Any items in MEMORY.md marked as pending or unresolved for more than 7 days
- Any patterns in timing — things that consistently come up on the same day of week

---

**Step 2: Assess the week**

Score each area briefly (one sentence each):

- **Inbox filing accuracy** — did items land in the right folders, or were there frequent misfires?
- **Morning brief usefulness** — based on what actually mattered this week, was the brief hitting the right things?
- **Memory quality** — is MEMORY.md growing with genuinely useful facts, or filling up with noise?
- **Unresolved items** — anything that fell through the cracks or sat unresolved too long?

---

**Step 3: Generate improvement proposals**

For each signal or pattern found in Step 1, propose a specific, actionable OS change. Use this format:

```
PROPOSAL: [short title]
Signal: [what you observed]
Change: [exactly what to update — which file, which field, what to add/change]
Type: [USER.md update / skill prompt update / new skill / new scheduled task / working habit]
```

Examples of good proposals:
- "Shopping list has 'oat milk' added 4 times this week → add as a standing weekly item in meal-planner.md"
- "ClassDojo messages keep hitting [NEEDS-REVIEW] → add ClassDojo to the school sender list in school-triage.md"
- "Pickup schedule changed for Wednesdays based on 3 separate notes → update USER.md Wednesday entry"
- "You asked for a helper payment reminder twice → add a Friday standing check to morning-brief.md"

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
[List each proposal using the format above]

### Applied this week
[Leave blank — fill in manually after you act on proposals]
```

---

**Step 5: Post a summary to Slack**

Post a brief message to the household inbox Slack channel:

```
📋 *Weekly FamilyOS Review — [DATE]*

[1–2 sentence summary of how the week went]

*Proposals this week:*
• [Proposal 1 — one line]
• [Proposal 2 — one line]
• [Proposal 3 — one line]

Review the full report in OS-LOG.md to apply any changes.
```

Keep it short. The full detail is in OS-LOG.md. This Slack message is just your prompt to go look.

---

**Rules:**
- Propose changes. Do not apply them automatically.
- The user decides what to act on and when.
- If there's nothing worth proposing this week, say so clearly rather than inventing suggestions.
- Honest assessment beats positive spin. If something isn't working, name it.
