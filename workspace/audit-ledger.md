# Audit Ledger — Durable record of auto-applied / filed-no-action / reverted entries

Append-only. Rotated each night by the Evening Memory job (`memory-consolidation`) **before** LOG.md is cleared, so audit trail survives the daily clear.

Used by:
- **`weekly-review`** — counts confirmations-without-edits per H4 action class to surface H4→H5 promotion proposals (see `workspace/TOOLS.md` → Action trust levels)
- **`triage-digest`** — daily 2:00 PM Slack post that summarizes recent `[FILED-NO-ACTION]` and `[AUTO-APPLIED]` entries
- **`slack-inbox`** — looks up `[AUTO-APPLIED]` entries by class + date when processing ♻️ rollback or typed `revert <date> <class>`, and reads `[REACTION-PROCESSED]` markers for cross-day idempotency

Pruning: when this file exceeds ~500 lines, the Evening Memory job summarizes entries older than 90 days into `## Archive` (one-line-per-class with cumulative counts) and removes the originals from "Active rotation window."

Tag formats are documented in `workspace/TOOLS.md` → LOG.md tag conventions.

---

## Active rotation window

<!-- Newest first; rotated nightly from LOG.md by memory-consolidation. Older than 90 days are summarized into ## Archive below. -->

---

## Archive

<!-- Summary entries older than 90 days. Format: "[YYYY-MM ranges] class:<class-name> count:N reverts:M" — one line per class per month-range. -->
