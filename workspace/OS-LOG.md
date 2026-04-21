# OS-LOG.md — FamilyOS Changelog

Every week the Weekly OS Review job appends a report here.
When you apply a proposed change, fill in the "Applied this week" section.
This file is your record of how FamilyOS has evolved over time.

---

## How to read this file

Each entry covers one week. The weekly review job writes the top sections automatically.
You fill in "Applied this week" manually after acting on proposals.

If you want to roll back a change, this log tells you what was there before.

---

<!-- Weekly review entries appear below, newest at top -->

## Week of 2026-04-13

### Week summary
First full week since FamilyOS went live. Memory is capturing the right kind of facts (dentist dates, birthday-party logistics, helper arrangements), but the Slack-inbox → Asana task-creation handoff is the clear weak spot — seven items are sitting in LOG.md as `[PENDING-ASANA]` today, echoing a process concern MEMORY.md already flagged on 2026-04-16. Two smaller items (duplicate dentist calendar event, "message to Meghan" Asana close) also didn't get resolved this week.

### Signals found
- **[NEEDS-REVIEW] items:** 1 — duplicate "Dentist" calendar event 8:00–9:00 AM on 2026-05-27 inviting Danielle and John (logged 2026-04-18).
- **[NEEDS-APPROVAL] items:** 1 — close Asana task "confirmation message to Meghan" after Danielle reported the message was sent (logged 2026-04-18).
- **[PENDING-ASANA] items:** 7, all logged today (2026-04-19), all awaiting Slack confirmation to create in Asana:
  1. Send Ms. Jan a photo of Soma's concert silver costume (due 2026-05-19)
  2. Reimburse John's personal ChatGPT charges
  3. Credit Disneyland trip and related charges to OTMG — log only, don't reimburse
  4. Switch credit card for YouTube Premium to OTMG, reimburse
  5. Remove transactions in Family Savings that shouldn't be there (Monarch)
  6. Write a MOST (medical instructions) for John and Danielle
  7. Pay life insurance premium
- **Recurring topic:** Slack-inbox → Asana handoff. MEMORY.md [2026-04-16] flagged "the hourly Slack-inbox skill initially skipped the Asana create-task flow for new items from LOG entries" as worth watching. Today's 7 stalled items confirm the pattern — the create-task confirmation request is not reaching the user reliably.
- **Unresolved from prior weeks:** None (first review).
- **Timing pattern:** Today's 7 `[PENDING-ASANA]` items all landed together, suggesting Sunday brain-dump behavior. Worth accommodating rather than fighting.

### Proposals

**Proposal 1: Post a consolidated confirmation prompt for today's 7 PENDING-ASANA items**
- Signal: 7 items stuck in LOG.md with no confirmation flow reaching Danielle.
- Change: Post a single Slack message to `#familyos` listing all 7 items with titles, notes, and due dates, inviting `confirm 1,2,5` / `confirm all` / `skip N` responses. Covered in the weekly review Slack post below so it gets bundled with the review.
- Type: Working habit (one-off unblocking action for this week).
- Auto-apply: No — requires posting to Slack, which the weekly-review skill already authorizes via Step 6.

**Proposal 2: Update the Slack-inbox skill to re-prompt stalled PENDING-ASANA items**
- Signal: Today's stalled batch of 7 reproduces the 2026-04-16 process failure exactly.
- Change: Edit `skills/slack-inbox/SKILL.md` (or the inbox prompt) so that on every hourly run it checks LOG.md for `[PENDING-ASANA]` entries older than 24 hours and re-posts them as a single batch confirmation prompt to `#familyos`. Prevents drift to "sitting for a week."
- Type: Skill prompt update.
- Auto-apply: Yes (text edit to an existing skill file).

**Proposal 3: Set the urgency threshold in USER.md**
- Signal: USER.md line 87 still reads "(not yet set — default to flagging items that need action today or tomorrow)". The default has been running for the full week without being pinned down.
- Change: Replace line 87 with an explicit value, e.g.: "Flag any item needing action within 48 hours. Elevate immediately anything touching Soma's health, Claren school, or money moving out of a household account, regardless of horizon."
- Type: USER.md update.
- Auto-apply: Yes.

**Proposal 4: Add a standing "housekeeping" section to the Monday morning brief**
- Signal: Two items ([NEEDS-REVIEW] duplicate dentist event, [NEEDS-APPROVAL] Meghan Asana close) have been sitting since 2026-04-18 with no nudge surface.
- Change: Edit `skills/morning-brief/SKILL.md` so the Monday brief includes a "Housekeeping" block listing open `[NEEDS-REVIEW]` and `[NEEDS-APPROVAL]` items from MEMORY.md. Other weekdays stay unchanged.
- Type: Skill prompt update.
- Auto-apply: Yes.

**Proposal 5: Create annual recurring Asana tasks for known standing household dates**
- Signal: USER.md lists recurring dates (BCAA renewal 2026-06-04, power washing every March, tire swap October and March, annual dentist hygienist recall) that are currently tracked only in prose — not in Asana, not in calendar, not in any reminder system.
- Change: Create 4 annual-recurring tasks in the "Lovell Warner Personal" Asana project: (a) BCAA tenants insurance renewal (June 4), (b) power washing booking (March), (c) tire swap — winter to summer (March) and summer to winter (October), (d) Soma dentist annual hygienist recall booking.
- Type: New Asana tasks (batch of 4 recurring).
- Auto-apply: No — Asana task creation requires explicit confirmation per USER.md line 79.

**Proposal 6: Start a lightweight "stuck items" section in MEMORY.md**
- Signal: MEMORY.md is well-formed but has no dedicated place for items explicitly waiting on a human. Today's 7 `[PENDING-ASANA]` entries are in LOG.md (which gets cleared nightly), which means memory-consolidation has to rescue them every evening or they disappear.
- Change: Add a "Waiting on human" section to the top of MEMORY.md after the header. Memory-consolidation moves any `[PENDING-ASANA]`, `[NEEDS-APPROVAL]`, or `[NEEDS-REVIEW]` items from LOG.md into that section each night and clears them when resolved.
- Type: New file section + skill prompt update to `skills/memory-consolidation/SKILL.md`.
- Auto-apply: Yes (text edits only).

### Trackable tasks to propose (require confirmation before Asana creation)

These are one-off actionable items surfaced by this week's signals. Not auto-created — awaiting a `confirm` reply:

- **Resolve duplicate May 27 dentist calendar event** — decide delete vs. keep the Redtree entry.
- **Close Asana task "confirmation message to Meghan"** — Danielle confirmed on 2026-04-18 that the message was sent.
- The 7 PENDING-ASANA items listed above under Signals.

### Applied
- **[2026-04-19 17:26]** — `apply all` received in Cowork session. All six proposals acted on in this pass.
- **[2026-04-19 17:26]** — Proposal 1 applied: consolidated PENDING-ASANA confirmation prompt posted to `#familyos` listing all 7 items with titles, notes, and due dates. Awaiting `confirm N` reply.
- **[2026-04-19 17:26]** — Proposal 2 applied: `skills/slack-inbox.md` updated with a "Stalled PENDING-ASANA re-prompt" section that scans LOG.md each hour for entries older than 24 hours and posts a once-per-day batch confirmation prompt.
- **[2026-04-19 17:26]** — Proposal 3 applied: `workspace/USER.md` urgency threshold replaced. New rule: flag items needing action within 48 hours; elevate Soma-health / Claren / money-out-of-household-account immediately regardless of horizon.
- **[2026-04-19 17:26]** — Proposal 4 applied: `skills/morning-brief.md` Step 5a added. On Mondays only, the brief includes a "Housekeeping" section surfacing `[NEEDS-REVIEW]`, `[NEEDS-APPROVAL]`, and `[PENDING-ASANA]` items plus the MEMORY.md "Waiting on human" section.
- **[2026-04-19 17:26]** — Proposal 5 applied: 5 annual recurring tasks created in Asana "Lovell Warner Personal":
  - BCAA tenants insurance renewal — due 2026-06-04 ([link](https://app.asana.com/1/1213619404321217/project/1213862432550383/task/1214140743462417))
  - Book spring power washing — due 2027-03-01 ([link](https://app.asana.com/1/1213619404321217/project/1213862432550383/task/1214140954456896))
  - Tire swap — winter to summer — due 2027-03-15 ([link](https://app.asana.com/1/1213619404321217/project/1213862432550383/task/1214140954477780))
  - Tire swap — summer to winter — due 2026-10-15 ([link](https://app.asana.com/1/1213619404321217/project/1213862432550383/task/1214140954452006))
  - Book Soma's next annual dentist hygienist recall — due 2027-05-01 ([link](https://app.asana.com/1/1213619404321217/project/1213862432550383/task/1214140743525477))
  - Note: Asana recurrence must be set in the Asana UI (MCP API can't set recurrence directly). Each task's notes section flags this.
- **[2026-04-19 17:26]** — Proposal 6 applied: `workspace/MEMORY.md` gained a "Waiting on human" section at the top, and `skills/memory-consolidation.md` gained a Step 2a that rescues `[PENDING-ASANA]`, `[NEEDS-APPROVAL]`, and `[NEEDS-REVIEW]` entries from LOG.md into that section each night before the clear.

