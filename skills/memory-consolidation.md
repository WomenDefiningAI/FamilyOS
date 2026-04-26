# Evening Memory — Scheduled Task Prompt

**How to use:** Copy everything below the line and paste it as the prompt when creating the "Evening Memory" scheduled task in Cowork. Set frequency to Daily at your preferred evening time (9:00 PM recommended).

**Action trust:** see `workspace/TOOLS.md` → Action trust levels. This skill's actions all auto-apply (H5) by design — they run as a nightly scheduled task. Class names: `memory-consolidation/append-dated-entry`, `memory-consolidation/append-waiting-on-human`, `memory-consolidation/remove-resolved-item`, `memory-consolidation/rotate-audit-ledger`, `memory-consolidation/consolidate-archive`, `memory-consolidation/clear-log`.

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

**Step 2: Update MEMORY.md** (class: `memory-consolidation/append-dated-entry`)

Add a dated entry at the top of MEMORY.md (below the "Waiting on human" section):

```
## [YYYY-MM-DD]
- [Learning 1]
- [Learning 2]
```

Be terse. One line per fact. Skip anything already captured in MEMORY.md.
If today's log had nothing worth persisting, skip this step entirely — don't add an empty entry.

**Step 2a: Rescue waiting-on-human items into MEMORY.md** (classes: `memory-consolidation/append-waiting-on-human`, `memory-consolidation/remove-resolved-item`)

Before clearing LOG.md, scan it for any line tagged `[PENDING-ASANA]`, `[NEEDS-APPROVAL]`, or `[NEEDS-REVIEW]`.

For each such line that is not already present in the "Waiting on human" section of MEMORY.md, append it verbatim to that section (preserve the date prefix and tag).

For each entry already listed in "Waiting on human" that LOG.md marks as resolved today (e.g., an explicit "resolved" note referencing the same item), remove the corresponding line from the "Waiting on human" section.

This prevents waiting items from disappearing when LOG.md is cleared in Step 4.

**Step 2b: Rotate audit tags into audit-ledger.md** (class: `memory-consolidation/rotate-audit-ledger`)

Before clearing LOG.md, scan it for any line tagged `[AUTO-APPLIED]`, `[FILED-NO-ACTION]`, `[AUTO-APPLIED-REVERTED]`, or `[REACTION-PROCESSED]`.

For each such line, append it verbatim to the **"Active rotation window"** section of `workspace/audit-ledger.md` (newest at the top of that section). Preserve the timestamp and tag exactly as written.

If `workspace/audit-ledger.md` does not exist (first run after this skill ships), create it from the template documented at the top of the file:

```
# Audit Ledger — Durable record of auto-applied / filed-no-action / reverted entries
[…template body…]
## Active rotation window
## Archive
```

These tags do **not** rescue to MEMORY.md "Waiting on human" — they're audit trail, not waiting items. They rotate to the ledger so weekly-review can count confirmations-without-edits per H4 action class even after the LOG clear (see `workspace/TOOLS.md` → Action trust levels).

If a line is malformed (e.g., missing the required `class:` field), skip it silently and append a one-line warning to LOG.md before the clear: `[YYYY-MM-DD HH:MM] [ROTATION-SKIPPED] reason:"<short text>" line:<verbatim line>`. The warning persists into the ledger via this same step on the next run, so no audit data is lost.

**Step 2c: Prune the audit ledger if needed** (class: `memory-consolidation/consolidate-archive`)

After Step 2b's append, check `workspace/audit-ledger.md` total line count. If "Active rotation window" exceeds ~500 lines, summarize entries older than 90 days into the `## Archive` section as one line per class per month-range:

```
[YYYY-MM..YYYY-MM] class:<class-name> count:<N> reverts:<M>
```

Then remove those original entries from "Active rotation window." Newer entries stay verbatim.

**Step 3: Trim old memory if needed**

If MEMORY.md exceeds 150 lines, consolidate entries older than 90 days into a brief summary paragraph at the bottom under `## Archive`. Keep the last 90 days as individual dated entries.

**Step 4: Clear LOG.md** (class: `memory-consolidation/clear-log`)

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
