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

| If the message is about... | File it to... |
|---|---|
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
