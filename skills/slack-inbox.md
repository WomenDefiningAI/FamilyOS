# Household Inbox — Scheduled Task Prompt

**How to use:** Copy everything below the line and paste it as the prompt when creating the "Household Inbox" scheduled task in Cowork. Set frequency to Hourly.

**Action trust:** see `workspace/TOOLS.md` → Action trust levels for which actions auto-apply (H5) vs. propose (H4) vs. require explicit approval (H4-forever). Class names referenced inline in this prompt (e.g., `slack-inbox/file-shopping-item`) match rows in that ledger.

---

Read SOUL.md and TOOLS.md from the FamilyOS workspace folder.

Check the Slack channel listed in TOOLS.md as the household inbox.

First, read `memory/last-inbox-check.md` to find the timestamp of the last run.
Look for any Slack messages posted **after** that timestamp.
After processing, update `memory/last-inbox-check.md` with the current timestamp.

If there are no new messages: stop here. Do not write anything or send any notification.

For each new message, classify and file:

| If the message is... | File it / act on it by... | Class |
|---|---|---|
| An apply directive — `apply N`, `apply N,M`, `apply all`, or `skip` (case-insensitive) | Act on the most recent weekly review proposals. See **Applying weekly review proposals** below. | (control flow) |
| A revert directive — `revert <YYYY-MM-DD> <class>`, `revert last`, or `revert N` (in response to a digest disambiguation reply) | Reverse a prior `[AUTO-APPLIED]` action. See **Handling reverts and reactions** below. | (control flow) |
| A shopping item | Append to `resources/shopping/current-list.md` | `slack-inbox/file-shopping-item` |
| A school notice, date, or form | Append to `resources/school/notices.md` | `slack-inbox/file-school-notice` |
| A helper note (payment, schedule) | Append to `resources/helpers/log.md` | `slack-inbox/file-helper-log` |
| Home maintenance (repair, vendor, cost) | Append to `resources/home-maintenance/log.md` | `slack-inbox/file-home-maintenance` |
| A general reminder or "remember this" | Append to `LOG.md` with today's date | `slack-inbox/append-general-log` |
| **Triage_no** — an auto-receipt already in Monarch, an accepted calendar invite, a duplicate notification, a newsletter, an automated digest, or any item the classifier is confident requires no human action | Append a `[FILED-NO-ACTION]` line to `LOG.md` per the schema in TOOLS.md → LOG.md tag conventions. **Do NOT post to Slack** — the daily triage-digest covers it. | `slack-inbox/file-no-action` |
| A task you can act on now | Complete it using available tools, then log what was done in `LOG.md` | (varies — see TOOLS.md) |
| Ambiguous or unclear | Append to `LOG.md` tagged exactly as `[NEEDS-REVIEW]` with the original message and today's date | `slack-inbox/tag-needs-review` |

**Filing rules:**
- Always append — never overwrite
- Every entry gets a date: `[YYYY-MM-DD]`
- Ambiguous items must use the exact tag `[NEEDS-REVIEW]` so the weekly review job can find them
- If a message could require sending an email, adding a calendar event, or any outbound action: log the intent in LOG.md tagged `[NEEDS-APPROVAL]` and do not act unilaterally
- **Classifier confidence**: only emit `triage_no` when the message is clearly a no-action item. If confidence is ambiguous (the item could be triage_no OR a real shopping/school/helper/maintenance item), default to `[NEEDS-REVIEW]` — never auto-file as triage_no on uncertainty.

**Acting on H5 actions (auto-apply with audit trail):**

After classifying a message into a row above, look up the row's `Class` column in `workspace/TOOLS.md` → Action trust levels:

- If the class is **H5**: perform the file append/edit AS WRITTEN, then append an `[AUTO-APPLIED]` line to `LOG.md` per the schema in TOOLS.md → LOG.md tag conventions. The originating Slack message ID becomes the `source:` field.
- If the class is **H4**: perform the file append/edit AS WRITTEN (no proposal step — these are still log appends today), but write a regular dated entry in LOG.md instead of `[AUTO-APPLIED]`. The H4 baseline is preserved for classes whose ledger row signals "eligible for H5 promotion later."
- If the class is **H4-forever** or the class is missing from the ledger: do NOT auto-file. Tag the LOG entry `[NEEDS-APPROVAL]` and surface for confirmation.

**TOOLS.md unreadable fallback** (preserve pre-feature behavior):

If `workspace/TOOLS.md` is missing, malformed, or its Action trust levels table cannot be parsed, use this embedded defaults table — these are the pre-feature defaults, NOT a forced H4 fallback:

| Class | Embedded default |
|---|---|
| `slack-inbox/file-shopping-item` | H5 (auto-file; matches pre-feature behavior) |
| `slack-inbox/file-school-notice` | H5 |
| `slack-inbox/file-helper-log` | H5 |
| `slack-inbox/file-home-maintenance` | H5 |
| `slack-inbox/append-general-log` | H5 |
| `slack-inbox/tag-needs-review` | H5 |
| `slack-inbox/append-pending-asana` | H5 |
| `slack-inbox/append-needs-approval` | H5 |
| `slack-inbox/update-inbox-timestamp` | H5 |
| `slack-inbox/file-no-action` | H4 (do NOT auto-emit triage_no without ledger; surface as [NEEDS-REVIEW] until ledger restored) |
| any other class | H4 (propose) |

Post a single Slack message to `#familyos`: `⚠️ TOOLS.md unreadable — running on embedded defaults until restored.` Continue the hourly run with embedded defaults; the daily morning brief will surface the issue separately if it persists.

---

## Stalled PENDING-ASANA re-prompt

After processing any new messages (even if there were none), scan LOG.md for entries tagged `[PENDING-ASANA]` whose date is more than 24 hours before today.

If 1 or more stalled entries exist:

1. Bundle them into a single Slack message posted to the household inbox channel.

   **Single-item format** (exactly 1 stalled entry — emoji-confirm path):

   ```
   📝 Pending: **[Task title]** — [due date or "no due date"] — logged [YYYY-MM-DD]
   React 👍 to confirm or ✕ to skip.

   audit ref: [YYYY-MM-DD] / slack-inbox/post-pending-asana-reminder
   ```

   On the next hourly run, read reactions on this message via the Slack MCP (see **Handling reverts and reactions** below):
   - 👍 reaction by Danielle → process as `confirm 1` (create the Asana task, remove the PENDING-ASANA line, post a brief confirmation)
   - ✕ reaction by Danielle → process as `skip` (leave PENDING-ASANA in place; do not re-prompt today)

   **Multi-item bundle format** (≥2 stalled entries — typed-directive path):

   ```
   📝 *PENDING-ASANA backlog — still waiting on confirmation*

   These items have been sitting for more than a day. Reply `confirm 1,3` (or `confirm all`, or `skip`) and I'll create the selected ones in Lovell Warner Personal.

   1. **[Task title]** — [due date or "no due date"] — logged [YYYY-MM-DD]
   2. ...

   audit ref: [YYYY-MM-DD] / slack-inbox/post-pending-asana-reminder
   ```

   Multi-item bundles do NOT use emoji-confirm — typed `confirm 1,3` is more expressive when there are multiple items. The `audit ref:` footer is required on every slack-inbox post.

2. Post once per day at most — check LOG.md for a line matching `[YYYY-MM-DD] [PENDING-ASANA-REMINDER-SENT]` with today's date. If present, skip. Otherwise, after posting, append `[YYYY-MM-DD] [PENDING-ASANA-REMINDER-SENT]` to LOG.md so the same batch isn't re-sent within the same day.

3. When a later `confirm N` or `confirm all` reply lands, process it like an apply directive — create the named tasks in Asana's "Lovell Warner Personal" project (class: `asana/create-task` — H4-forever, requires explicit user confirmation per TOOLS.md) and remove those entries from the PENDING-ASANA list in LOG.md (class: `slack-inbox/asana-remove-pending` — H5; log cleanup after user-confirmed Asana create).

---

## Applying weekly review proposals

When a Slack message matches `apply N`, `apply N,M`, `apply all`, or `skip` (case-insensitive), it's an instruction to act on the most recent week's proposals in `OS-LOG.md`.

1. **Find the target.** Read the most recent `## Week of [date]` section in `OS-LOG.md`. If that section is older than 7 days, post `No recent weekly review to apply against — skipping.` and stop.
2. **For each proposal named (or all of them if `apply all`):**
   - If the proposal is marked `Auto-apply: yes`, perform the change exactly as written — edit the named file, add the named line, or update the named field. Do not improvise beyond what the proposal says.
   - If marked `Auto-apply: no`, do NOT apply. Note in the Applied section that it requires manual setup (and why).
   - If the proposal already has an entry in that week's **Applied** section, skip it (already applied).
3. **Update OS-LOG.md** (class: `slack-inbox/os-log-update-applied` — H5; text append to OS-LOG.md "Applied" section after user `apply N`). Append one line per proposal to the **Applied** section of that week's block:
   - Applied: `- **[YYYY-MM-DD HH:MM]** — Proposal N applied via Slack reply. [What changed and where.]`
   - Not applied: `- **[YYYY-MM-DD HH:MM]** — Proposal N not applied: requires manual setup ([reason]).`
   - Skip directive: `- **[YYYY-MM-DD HH:MM]** — Week skipped via Slack reply. No proposals applied.`
4. **Post a confirmation** back to the same Slack channel. Keep it tight:
   ```
   ✅ Applied: 1 (school senders), 3 (USER.md Wed pickup)
   ⚠️ Manual setup needed: 2 (new scheduled task — Cowork → Scheduled)

   audit ref: [YYYY-MM-DD] / slack-inbox/post-confirmation
   ```
   For `skip`: `Skipped — no proposals applied this week.` (followed by the same audit-ref footer.)
5. Do not act on parts of the message that aren't apply directives. If the user mixed an apply directive with other content, only handle the apply portion and treat the rest as ambiguous (`[NEEDS-REVIEW]`).

---

## Handling reverts and reactions

The trust gradient lets H5 actions auto-apply with rollback as the safety net. This section covers (a) ♻️ reactions on slack-inbox's own past messages, (b) 👍/✕ reactions on single-item PENDING-ASANA reminders, and (c) typed `revert` directives.

**On every hourly run**, after processing new messages:

1. **Slack MCP capability check** (one-time, first run after this feature ships):
   - Read the most recent FamilyOS bot message in `#familyos` via `slack_read_channel`. Confirm the response includes a non-empty `reactions` array (even if no reactions were added — empty array is fine; missing field is the failure case).
   - If the field is missing entirely, log to LOG.md: `[YYYY-MM-DD] [REACTION-CAPABILITY] unavailable — Slack MCP does not expose reactions. Falling back to typed revert / confirm directives only.` Skip the rest of this section on every subsequent run until a curator updates this skill prompt.
   - Otherwise: proceed with reaction reading.

2. **Read reactions** on slack-inbox's own past messages (those bearing an `audit ref:` footer) from the last 7 days via `slack_read_channel`. For each new reaction (one not yet logged in `[REACTION-PROCESSED]`):

   **♻️ rollback flow:**
   - Parse the message's `audit ref:` footer to extract `<date>` and `<class>`.
   - **Single-action message** — the audit-ref class points to one specific class (e.g., `slack-inbox/post-pending-asana-reminder`, `slack-inbox/post-confirmation`, or any future per-action confirmation post): find the corresponding `[AUTO-APPLIED]` line in `LOG.md` (today's entries) or `workspace/audit-ledger.md` "Active rotation window" (older entries). Reverse the change by reading the `file:` and `what:` fields of the AUTO-APPLIED entry and performing the inverse edit (delete the appended line; restore the prior tag; etc.). Append `[AUTO-APPLIED-REVERTED]` per schema. Reply: `Reverted: <what:>. (auto-applied <date>; reversed today)`. Append `[REACTION-PROCESSED] msg:<msg-ts> action:revert` to LOG.md.
   - **Multi-action message** — the audit-ref class is `triage-digest/post-daily-digest` (the digest aggregates many bullets in one message): a single ♻️ has no per-bullet granularity, so reply with a numbered enumeration of the digest's reversible bullets (auto-applied entries only — `[FILED-NO-ACTION]` entries are not "reversible" since no resource state was changed beyond a log line) and prompt: `Which to revert? Reply revert N (or revert N,M / revert all-auto / cancel).` Append `[REACTION-PROCESSED] msg:<msg-ts> action:revert-prompted` to LOG.md so the same ♻️ doesn't repeat the prompt.

   **👍 / ✕ on single-item PENDING-ASANA reminder:**
   - Verify the audit-ref footer's class is `slack-inbox/post-pending-asana-reminder` AND the message body has the single-item format (one task only, not a numbered list).
   - 👍 → process as `confirm 1` against that one item. Create the Asana task in Lovell Warner Personal, remove the matching PENDING-ASANA line from LOG.md, post brief confirmation, append `[REACTION-PROCESSED] msg:<msg-ts> action:confirm`.
   - ✕ → process as `skip`. Leave the PENDING-ASANA line in place. Append `[REACTION-PROCESSED] msg:<msg-ts> action:skip`.
   - Multi-item PENDING-ASANA bundles ignore reaction reactions entirely — only typed `confirm 1,3` / `confirm all` / `skip` directives count.

   **Pre-feature messages** (no audit-ref footer) — older slack-inbox posts that pre-date this feature: any reaction is silently ignored. Append `[REACTION-PROCESSED] msg:<msg-ts> action:skip-pre-feature` so the same reaction isn't re-evaluated on the next hourly run.

   **Lookup miss** — the audit-ref points to a class+date but no `[AUTO-APPLIED]` line is found in LOG.md or audit-ledger.md (write race / very old / pruned past 90 days / never existed): reply `Cannot revert — no audit record found for <class> on <date>. The action may not have been applied, or the audit entry was pruned.` Append `[REACTION-PROCESSED] msg:<msg-ts> action:revert-miss`.

3. **Typed `revert` directives** — when a `#familyos` message matches `revert <YYYY-MM-DD> <class>`, `revert last`, or `revert N` (in response to a digest disambiguation reply that listed numbered bullets):

   - `revert <date> <class>`: same logic as the single-action ♻️ flow above, addressing the AUTO-APPLIED entry by typed date+class.
   - `revert last`: locate the most recent `[AUTO-APPLIED]` line not already reverted; revert it.
   - `revert N` or `revert N,M` or `revert all-auto`: process in the context of the most recent disambiguation prompt (the most recent slack-inbox post tagged `[REACTION-PROCESSED] msg:<msg-ts> action:revert-prompted` within the past hour). Resolve N/M to the bullets from that disambiguation list, apply each like the single-action flow.
   - `cancel` (only meaningful in the disambiguation context): no action, append `[REACTION-PROCESSED] msg:<original-digest-msg-ts> action:revert-cancelled`.

4. **Cross-day idempotency**: `[REACTION-PROCESSED]` entries are rotated to `workspace/audit-ledger.md` nightly by memory-consolidation (per Step 2b in that skill). On every hourly run, check BOTH LOG.md AND audit-ledger.md "Active rotation window" before processing a reaction — the same `msg:<msg-ts>` should not be re-processed on consecutive days.

5. Reverts of multiple stacked AUTO-APPLIED actions in one ♻️ are out — each ♻️ reverts at most one action (or triggers disambiguation in the digest case). Use typed `revert N,M` for batch revert via the disambiguation path.
