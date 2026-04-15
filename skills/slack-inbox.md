# Household Inbox — Scheduled Task Prompt

**How to use:** Copy everything below the line and paste it as the prompt when creating the "Household Inbox" scheduled task in Cowork. Set frequency to Hourly.

---

Read SOUL.md and TOOLS.md from the FamilyOS workspace folder.

Check the Slack channel listed in TOOLS.md as the household inbox.

First, read `memory/last-inbox-check.md` to find the timestamp of the last run.
Look for any Slack messages posted **after** that timestamp.
After processing, update `memory/last-inbox-check.md` with the current timestamp.

If there are no new messages: stop here. Do not write anything or send any notification.

For each new message, classify and file:

| If the message is... | File it / act on it by... |
|---|---|
| An apply directive — `apply N`, `apply N,M`, `apply all`, or `skip` (case-insensitive) | Act on the most recent weekly review proposals. See **Applying weekly review proposals** below. |
| A shopping item | Append to `resources/shopping/current-list.md` |
| A school notice, date, or form | Append to `resources/school/notices.md` |
| A helper note (payment, schedule) | Append to `resources/helpers/log.md` |
| Home maintenance (repair, vendor, cost) | Append to `resources/home-maintenance/log.md` |
| A general reminder or "remember this" | Append to `LOG.md` with today's date |
| A task you can act on now | Complete it using available tools, then log what was done in `LOG.md` |
| Ambiguous or unclear | Append to `LOG.md` tagged exactly as `[NEEDS-REVIEW]` with the original message and today's date |

**Filing rules:**
- Always append — never overwrite
- Every entry gets a date: `[YYYY-MM-DD]`
- Ambiguous items must use the exact tag `[NEEDS-REVIEW]` so the weekly review job can find them
- If a message could require sending an email, adding a calendar event, or any outbound action: log the intent in LOG.md tagged `[NEEDS-APPROVAL]` and do not act unilaterally

---

## Applying weekly review proposals

When a Slack message matches `apply N`, `apply N,M`, `apply all`, or `skip` (case-insensitive), it's an instruction to act on the most recent week's proposals in `OS-LOG.md`.

1. **Find the target.** Read the most recent `## Week of [date]` section in `OS-LOG.md`. If that section is older than 7 days, post `No recent weekly review to apply against — skipping.` and stop.
2. **For each proposal named (or all of them if `apply all`):**
   - If the proposal is marked `Auto-apply: yes`, perform the change exactly as written — edit the named file, add the named line, or update the named field. Do not improvise beyond what the proposal says.
   - If marked `Auto-apply: no`, do NOT apply. Note in the Applied section that it requires manual setup (and why).
   - If the proposal already has an entry in that week's **Applied** section, skip it (already applied).
3. **Update OS-LOG.md.** Append one line per proposal to the **Applied** section of that week's block:
   - Applied: `- **[YYYY-MM-DD HH:MM]** — Proposal N applied via Slack reply. [What changed and where.]`
   - Not applied: `- **[YYYY-MM-DD HH:MM]** — Proposal N not applied: requires manual setup ([reason]).`
   - Skip directive: `- **[YYYY-MM-DD HH:MM]** — Week skipped via Slack reply. No proposals applied.`
4. **Post a confirmation** back to the same Slack channel. Keep it tight:
   ```
   ✅ Applied: 1 (school senders), 3 (USER.md Wed pickup)
   ⚠️ Manual setup needed: 2 (new scheduled task — Cowork → Scheduled)
   ```
   For `skip`: `Skipped — no proposals applied this week.`
5. Do not act on parts of the message that aren't apply directives. If the user mixed an apply directive with other content, only handle the apply portion and treat the rest as ambiguous (`[NEEDS-REVIEW]`).
