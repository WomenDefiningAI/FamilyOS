# Triage Digest — Scheduled Task Prompt

**How to use:** Copy everything below the line and paste it as the prompt when creating the "Triage Digest" scheduled task in Cowork. Set frequency to **Daily at 2:00 PM** for v1; once you have confidence in the trust gradient classifier, edit this skill prompt to switch to **Weekly on Friday at 2:00 PM** and update the cron in Cowork → Scheduled.

**Action trust:** see `workspace/TOOLS.md` → Action trust levels. The digest itself (`triage-digest/post-daily-digest`) is **system communication, not a ledger-governed action** — it runs unconditionally as part of the scheduled task. The audit-ref footer on its post anchors `slack-inbox`'s ♻️ disambiguation flow.

---

Read SOUL.md and TOOLS.md from the FamilyOS workspace folder.

This is the FamilyOS Triage Digest. Its job: post one Slack message summarizing what the system did without surfacing in the past 24 hours, so the user has a daily checkpoint to spot-check H5 auto-applies and triage_no filings.

---

**Step 1: Gather the past 24 hours**

Read entries tagged `[AUTO-APPLIED]` or `[FILED-NO-ACTION]` from the past 24 hours. They live in two places, and you must read both because the rotation boundary cuts through this window:

- `workspace/LOG.md` — today's pre-rotation entries (between last night's `memory-consolidation` clear and now)
- `workspace/audit-ledger.md` "Active rotation window" — entries already rotated by last night's `memory-consolidation`

For each entry, parse the standard schema:

```
[YYYY-MM-DD HH:MM] [AUTO-APPLIED] class:<class-name> file:<repo-rel-path> what:"<one-line summary>" source:<originating-id>
[YYYY-MM-DD HH:MM] [FILED-NO-ACTION] class:<class-name> file:<repo-rel-path> what:"<one-line summary>" source:<slack-msg-ts>
```

Skip entries already reverted (a matching `[AUTO-APPLIED-REVERTED]` line elsewhere).

Filter strictly on the timestamp date — entries from outside the 24h window are ignored even if they appear in the read range.

---

**Step 2: Compose the digest**

If both buckets are empty, post nothing. Skip empty sections per SOUL.md.

If at least one bucket has entries, post one message to the household inbox channel listed in TOOLS.md. Use Slack-bold headers, terse bullets, dates spelled out fully:

```
📋 *Triage digest — [today's date, e.g., Friday April 26]*

*Filed without surfacing*
• [time] — "[what]" filed to [file] (source: [source])
• ...

*Auto-applied*
• [time] — [class]: "[what]" in [file]
• ...

audit ref: [YYYY-MM-DD] / triage-digest/post-daily-digest
```

Sections that are empty get omitted entirely (do not write `*Filed without surfacing*` followed by "(none)" — leave the header out). The audit-ref footer is required so `slack-inbox`'s ♻️ disambiguation flow knows this is a multi-action message and triggers the numbered enumeration reply (per `skills/slack-inbox.md` → Handling reverts and reactions).

Bullet rules:
- One bullet per entry, in newest-first order within each section.
- Time is `HH:MM`. Date is implicit since the digest covers the past 24 hours.
- For `[FILED-NO-ACTION]` entries whose `file:` is `workspace/LOG.md` (no dedicated resource folder fallback), say "logged" instead of "filed to LOG.md" to avoid noise.
- For `[AUTO-APPLIED]` bullets, the `class:` field is shown so the user can spot-check the trust ledger without leaving Slack.

---

**Step 3: Failure handling**

If `LOG.md` is unreadable or `audit-ledger.md` does not exist (first run before any rotation has happened): post a minimal one-line message:

```
📋 Triage digest — LOG.md or audit-ledger.md unavailable today. Will retry tomorrow.

audit ref: [YYYY-MM-DD] / triage-digest/post-daily-digest
```

Continue silently otherwise.

---

**Cadence migration to weekly (future)**

Once the user has confidence in the trust gradient classifier (typically after 4–6 weeks of clean daily digests with no `:repeat:` reverts), edit this prompt:

- Change "past 24 hours" to "past 7 days" in Step 1
- Change the schedule in Cowork → Scheduled from "Daily at 2:00 PM" to "Weekly on Friday at 2:00 PM"
- Change Step 2's date format from "[today's date]" to "[Friday's date — week of …]"

No other changes required. The `[AUTO-APPLIED]` and `[FILED-NO-ACTION]` reads still work; the audit-ledger 90-day pruning still keeps the read range reasonable.
