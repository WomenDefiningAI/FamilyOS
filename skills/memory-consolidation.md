# Evening Memory — Scheduled Task Prompt

**How to use:** Copy everything below the line and paste it as the prompt when creating the "Evening Memory" scheduled task in Cowork. Set frequency to Daily at your preferred evening time (9:00 PM recommended).

---

Read SOUL.md, USER.md, MEMORY.md, and TOOLS.md from the FamilyOS workspace folder.
Then read LOG.md.

**Step 1: Extract today's learnings**

From LOG.md, identify anything worth persisting as household knowledge:
- New facts about family members, schedules, or preferences
- Resolved issues and how they were resolved
- Vendor or helper notes (who did what, cost, outcome)
- Upcoming dates or deadlines not already in MEMORY.md
- Patterns worth remembering for future reference
- Anything explicitly flagged as "remember this"

**Step 2: Update MEMORY.md**

Add a dated entry at the top of MEMORY.md (below the "Waiting on human" section):

```
## [YYYY-MM-DD]
- [Learning 1]
- [Learning 2]
```

Be terse. One line per fact. Skip anything already captured in MEMORY.md.
If today's log had nothing worth persisting, skip this step entirely — don't add an empty entry.

**Step 2a: Rescue waiting-on-human items into MEMORY.md**

Before clearing LOG.md, scan it for any line tagged `[PENDING-ASANA]`, `[NEEDS-APPROVAL]`, or `[NEEDS-REVIEW]`.

For each such line that is not already present in the "Waiting on human" section of MEMORY.md, append it verbatim to that section (preserve the date prefix and tag).

For each entry already listed in "Waiting on human" that LOG.md marks as resolved today (e.g., an explicit "resolved" note referencing the same item), remove the corresponding line from the "Waiting on human" section.

This prevents waiting items from disappearing when LOG.md is cleared in Step 4.

**Step 3: Trim old memory if needed**

If MEMORY.md exceeds 150 lines, consolidate entries older than 90 days into a brief summary paragraph at the bottom under `## Archive`. Keep the last 90 days as individual dated entries.

**Step 4: Clear LOG.md**

Replace the contents of LOG.md with exactly this:

```
# LOG.md — Daily Scratchpad

Append-only during the day. The Evening Memory job reads this each night, extracts learnings into MEMORY.md, then clears it for tomorrow.

You can write here directly, or drop things in your Slack inbox channel — the hourly job will file them here automatically.

---

```

**Step 5: Check for anything urgent**

If today's log contained anything flagged as unresolved that the user needs to know about before tomorrow morning — post a brief note to the household inbox channel listed in TOOLS.md.

Otherwise: finish silently. No confirmation message needed.
