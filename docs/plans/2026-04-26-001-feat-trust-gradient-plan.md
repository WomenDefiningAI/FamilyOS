---
title: "feat: Trust Gradient — Helpfulness-graded action approval for FamilyOS"
type: feat
status: active
date: 2026-04-26
deepened: 2026-04-26
origin: docs/brainstorms/2026-04-26-trust-gradient-requirements.md
---

# feat: Trust Gradient — Helpfulness-graded action approval for FamilyOS

## Overview

FamilyOS skills currently propose-then-confirm every action regardless of blast radius. Low-stakes file appends ("buy oat milk") wait on the same Slack confirmation as outbound emails. The result is a confirmation backlog flagged in two consecutive weekly reviews.

This plan ships a Helpfulness-ladder-graded action approval system: each action class is assigned an H-level (H5 auto-apply / H4 propose-confirm / H4-forever absolute gate) in `workspace/TOOLS.md`. H5 actions act and FYI; H4 actions propose; H4-forever actions stay strictly gated. Rollback for H5 actions happens via Slack reaction (♻️) or typed `revert` directive. Promotion from H4 to H5 is curator-driven via a new weekly-review proposal type (≥6 confirmations-without-edits triggers it).

The change touches two canonical workspace files (`TOOLS.md` and a new `audit-ledger.md`), all seven existing skill prompts, and adds one new skill (triage-digest). No code changes — every skill is a markdown prompt — so verification is manual scenario-walkthrough rather than automated tests.

---

## Problem Frame

The Slack-inbox → Asana confirmation stall has been the only OS friction surfaced in two consecutive weekly reviews (Week of 2026-04-13, Week of 2026-04-20). Uniform approval is uniform friction: a tax-balance Asana task went overdue in plain sight, and weekly review proposals routinely sit at H4 awaiting `apply N` for changes that are pure text edits.

The framing principle is the [Helpfulness ladder](https://medium.com/helpful-com/how-to-be-an-effective-early-stage-employee-hint-be-helpful-e681b456a01f) (H1 = ask without pre-work; H4 = pre-work + recommendation + meta-conversation; H5 = take action confidently and FYI), balanced by 30% safe-failure: in low-risk areas, accept some rollback as the cost of speed.

See origin: `docs/brainstorms/2026-04-26-trust-gradient-requirements.md`.

---

## Requirements Trace

All 17 origin R-IDs are covered. Mapping:

- R1, R2, R3, R4 (trust ledger) → U1, U7
- R5 (triage_no classification) → U3
- R6 (triage-digest skill, daily 2pm) → U5
- R7 (cadence change to weekly Friday 2pm later) → U5 (governance documented)
- R8 (AUTO-APPLIED LOG schema) → U2, U3
- R9 (AUTO-APPLIED in digest) → U5
- R10 (♻️ reaction rollback) → U4
- R11 (typed `revert` fallback) → U4
- R12 (single-item PENDING-ASANA emoji confirm) → U4
- R13 (no emoji-confirm for weekly-review apply) → U4 (explicit non-implementation)
- R14, R15, R16 (H4→H5 promotion proposals) → U6
- R17 (manual demotion via TOOLS.md edit) → U1 (documented behavior)

**Origin actors:** A1 Danielle (curator), A2 slack-inbox skill, A3 weekly-review skill, A4 all other skills, A5 triage-digest skill (new).
**Origin flows:** F1 auto-apply, F2 triage_no FYI, F3 single-item emoji-confirm, F4 ♻️ rollback, F5 H4→H5 graduation.
**Origin acceptance examples:** AE1 (covers R1, R2, R5, R8), AE2 (covers R6, R9), AE3 (covers R12 / F3), AE4 (covers R10), AE5 (covers R14, R15), AE6 (covers R3).

---

## Scope Boundaries

- Track-record-based **automatic** H4→H5 promotion is out — promotion is curator-driven only.
- Multi-curator support is out (Danielle is the sole curator in v1).
- Confidence-scored at-the-edge (per-instance score) is rejected — predictability of category-based gradient is a feature.
- Emoji-confirm is **not** extended to weekly-review `apply N` directives in v1.
- 30% safe-failure is a philosophical floor, not an enforced metric — no rollback-rate alerting in v1.
- Multi-revert in one ♻️ reaction is out — ♻️ on a multi-bullet triage-digest message triggers a disambiguation reply (`revert N`); single-action messages revert immediately. Bulk revert via typed `revert N,M` is supported via the disambiguation path.
- A standalone "trust dashboard" surface is out — TOOLS.md is the dashboard.
- **The original documented friction (multi-item PENDING-ASANA confirmation stall) remains H4 in v1.** Single-item Asana stalls graduate to emoji-confirm (R12); multi-item stalls keep the existing typed `confirm 1,3` / `confirm all` / `skip` directives. Asana task creation never moves to H5 (TOOLS.md gates it as an external write). The plan's value for the documented friction comes indirectly: as classes that were stuck behind generic propose-confirm graduate to H5 via the H4→H5 promotion flow, the volume of confirmation requests drops, and the multi-item Asana batches become more focused. If the Asana multi-item stall persists after 2-3 weekly review cycles post-launch, surface it as a follow-up brainstorm — likely shape: graduate Asana create from H4 to H4-with-default-yes (28-day track record threshold; auto-create unless Danielle reacts ✕ within 1h).

### Deferred to Follow-Up Work

- Audit of Slack MCP reaction-reading capability lands in U4's first sub-step rather than as a separate up-front spike.
- Migration of triage-digest cadence from daily to weekly (Friday 2:00 PM) is a future text edit governed by Danielle's confidence — captured in U5 as a documented switch, not a separate plan.

---

## Context & Research

### Relevant Code and Patterns

- `workspace/TOOLS.md` — current "Actions that require explicit approval" list maps directly onto H4-forever rows. New "Action trust levels" section sits beneath it.
- `workspace/LOG.md` — append-only daily scratchpad, format `[YYYY-MM-DD] [TAG] freeform text`. New tags `[AUTO-APPLIED]`, `[FILED-NO-ACTION]`, `[AUTO-APPLIED-REVERTED]` follow that shape with `key:value` suffix.
- `workspace/MEMORY.md` — "Waiting on human" rescue section is the existing precedent for tag-driven persistence; new tags do NOT need rescuing (they're not waiting on a person) but their counts feed weekly-review.
- `skills/slack-inbox.md` — already has the classifier table (R5 extends it), the PENDING-ASANA bundle/reminder pattern (R12 modifies single-item case), and the `apply N` / `confirm N` / `skip` directive parser (R10/R11 extend with `revert`).
- `skills/weekly-review.md` — already emits numbered proposals with `Auto-apply: yes/no` and writes to OS-LOG.md; promotion proposals (R14/R15) are a new proposal type within this existing engine.
- `skills/memory-consolidation.md` — rescues `[PENDING-ASANA]`, `[NEEDS-APPROVAL]`, `[NEEDS-REVIEW]` from LOG.md to MEMORY.md "Waiting on human." Must be updated to ignore `[AUTO-APPLIED]` / `[FILED-NO-ACTION]` (not waiting on anyone) and not treat them as noise to clear either.

### Institutional Learnings

- No `docs/solutions/` archive exists yet (called out in the upstream brainstorm). The plan creates `docs/solutions/2026-04-26-trust-gradient.md` as the first entry once shipped (deferred — see ideation #7 in queue).
- MEMORY.md 2026-04-21 already records: "a `:+1:` emoji reaction on a Slack task proposal counts as valid confirmation to create the Asana task." R12's emoji-confirm flow generalizes this existing convention.
- MEMORY.md 2026-04-19 records: "yes create all" Slack message is a valid batch confirmation. Consistent with the typed-directive path retained for multi-item bundles.

### External References

- LangChain EAIA's three-bucket triage (`triage_no`, `triage_notify`, `triage_email`) is the inspiration for R5's `triage_no` classification — already cataloged in origin.
- Slack web API `conversations.history` returns `reactions` array per message; the Slack MCP's `slack_read_channel` / `slack_read_thread` wrap that endpoint. Verify the wrapper exposes the reactions field in U4 step 1.

---

## Key Technical Decisions

- **Trust ledger lives in TOOLS.md** (Approach A from brainstorm), not a new TRUST.md. Rationale: TOOLS.md already governs approval; one canonical artifact for trust + tools; auditable in 30 seconds.
- **Action class granularity:** ~25–30 classes, named `<skill>: <verb-phrase>` (kebab-case). Classes group similar actions but stay distinct enough that promotion is meaningful (e.g., `slack-inbox/file-shopping-item` and `slack-inbox/file-school-notice` are separate rows because their failure costs differ).
- **`since:YYYY-MM-DD` annotation** lives inline in each ledger row, in a 4th column: `Class | H-level | Rationale | Since`. Promotion proposals reset this date when a class graduates.
- **`[AUTO-APPLIED]` LOG schema:** `[YYYY-MM-DD] [AUTO-APPLIED] class:<class-name> file:<repo-rel-path> what:"<one-line-summary>" source:<originating-id>`. Pipe-free, grep-friendly, parser-friendly.
- **Triage-digest is a new skill, not a section of morning-brief.** Different cadence (2pm vs 7am), different posture (FYI checkpoint vs day-start brief), different audit purpose. Bundling would dilute both.
- **Reaction reading approach:** triage-digest and PENDING-ASANA reminder Slack posts include the LOG entry's date+class as a footer line ("audit ref: 2026-04-26 / slack-inbox/file-shopping-item"), so the slack-inbox revert handler can locate the LOG entry from the reacted message without a brittle ID linkage.
- **Default-to-H4 on classifier uncertainty.** When an action's class is ambiguous or its trust ledger row is missing, the skill defaults to H4 propose-confirm. Auto-apply is opt-in by virtue of being declared in the ledger.

---

## Open Questions

### Resolved During Planning

- **Slack MCP exposes message reactions?** Yes — `slack_read_channel` wraps `conversations.history` which returns the `reactions` array per message. Verified in U4 step 1 by reading recent FamilyOS messages and confirming the reactions field is populated.
- **Should `[AUTO-APPLIED]` entries get rescued by memory-consolidation into "Waiting on human"?** No — they're not waiting on anyone. Memory-consolidation should leave them in LOG.md until the nightly clear (already the default for non-tagged entries) and let weekly-review count them before that. See U2.
- **Triage-digest as new skill or section of morning-brief?** New skill. Decided in brainstorm; reaffirmed by cadence/posture difference.
- **Where does "since the class was introduced" timestamp live?** Inline 4th column in TOOLS.md trust ledger row.

### Deferred to Implementation

- The exact regex/grep used by weekly-review to parse `[AUTO-APPLIED]` lines and count per-class confirmations. Settle in U6 by walking through 2 weeks of synthetic LOG entries.
- Whether `[FILED-NO-ACTION]` entries appended by slack-inbox land in `LOG.md` (current default) or in a per-day file like `resources/swept/YYYY-MM-DD.md`. v1 keeps them in LOG.md to minimize file proliferation; revisit if LOG.md noise grows.
- The exact Slack message format for triage-digest's two-section layout — whether to use Slack-native bold blocks, code blocks, or plain text with inline markers. Resolve by trying one and tuning during U5 verification.

---

## Implementation Units

- U1. **Trust ledger in TOOLS.md (foundation)**

**Goal:** Add the canonical "Action trust levels" section to `workspace/TOOLS.md` with seeded rows covering every existing FamilyOS action class. This is the artifact every other unit reads.

**Requirements:** R1, R2, R3, R4, R17

**Dependencies:** None.

**Files:**
- Modify: `workspace/TOOLS.md`
- Test: manual verification — read each existing skill prompt and confirm every action it can take maps to a row in the ledger.

**Approach:**
- Add a new H2 section **"Action trust levels"** beneath the existing "Actions that require explicit approval" list. The new section subsumes that list (every old line becomes an H4-forever row in the ledger) but the prose stays as introductory context.
- Table columns: `Action class | H-level | Rationale | Since`.
- Seed values from the audit (~28 rows). Highlights:
  - H5 from day one: `slack-inbox/file-shopping-item`, `slack-inbox/file-school-notice`, `slack-inbox/file-helper-log`, `slack-inbox/file-home-maintenance`, `slack-inbox/append-general-log`, `slack-inbox/tag-needs-review`, `slack-inbox/update-inbox-timestamp`, `memory-consolidation/append-dated-entry`, `memory-consolidation/clear-log`, `weekly-review/append-os-log`. (Pure file appends/edits to workspace files; safely reversible by file edit.)
  - H4 baseline (eligible for H5): `meal-planner/append-shopping-ingredients`, `meal-planner/save-week-plan`, `school-triage/append-notices`. These are state-changing file edits originating from non-classifier skills — start H4 until track record justifies promotion.
  - H4-forever: `asana/create-task`, `external/send-email`, `external/calendar-modify`, `external/maple-push`, `workspace/delete-overwrite`, `external/share-family-info`. Plus any outbound message to a non-household recipient (e.g., the Anne Luyten / Wize protocol messages).
  - **Not in the ledger (communication, not state-change):** Slack posts from FamilyOS skills to `#familyos` (`morning-brief/post-daily-brief`, `weekly-review/post-summary`, `slack-inbox/post-pending-asana-reminder`, `triage-digest/post-daily-digest`, `slack-inbox/post-confirmation`). These run unconditionally as part of their parent skill's normal flow.
- Rationale column is one short clause per row (e.g., "text append, easily reversed" / "Slack post — edit-reversible" / "external write — TOOLS.md gates"). For H5-from-day-one rows in the initial seed, append `(seed; revisit week 1)` so the first weekly review post-launch deliberately re-examines them — phased-rollout safeguard against migration regressions.
- Since column is `2026-04-26` (today) for all H5 rows; blank for H4 / H4-forever rows.
- Ledger scope: the trust gradient governs **state-changing actions** — file edits in `workspace/`, Asana writes, calendar writes, email/SMS sends, Maple pushes, file deletes/overwrites. **Slack posts from FamilyOS skills to `#familyos` (own inbox) are communication, not ledger-governed actions.** They run as part of their parent skill's normal flow (morning-brief posts the daily brief; triage-digest posts the digest; slack-inbox posts the PENDING-ASANA reminder); the gradient does not gate them. This matches today's behavior (those posts already fire without confirmation). Audit-ref footers ride on those posts to enable U4's reaction-rollback flow, but the post itself is not the action being approved.
- Conservative seed scope (H5-from-day-one): intentionally limited to text appends to workspace files — `slack-inbox/file-shopping-item`, `slack-inbox/file-school-notice`, `slack-inbox/file-helper-log`, `slack-inbox/file-home-maintenance`, `slack-inbox/append-general-log`, `slack-inbox/tag-needs-review`, `memory-consolidation/append-dated-entry`, `memory-consolidation/clear-log`, `weekly-review/append-os-log`. Other state-changing actions (`meal-planner/append-shopping-ingredients`, `school-triage/append-notices`, etc.) start H4 baseline and graduate via U6's promotion mechanism when track record warrants.
- Add a 3-paragraph prose preamble before the table:
  - One paragraph defining the H-ladder + 30% safe-failure framing (with a link to the Helpfulness blog post).
  - One paragraph explaining that the ledger is the single source of truth — skills read this, not their own opinions.
  - One paragraph explaining manual demotion (R17): Danielle can edit any H5 row down to H4 or H4-forever directly; this is the always-available escape hatch.

**Patterns to follow:**
- Table style and tone matches the existing TOOLS.md "Connected tools" and "Tool use priority" sections — terse, no scolding.

**Test scenarios:**
- Happy path: every action enumerated in the audit has exactly one row in the ledger.
- Edge case: H4-forever rows in the ledger 1:1 mirror the existing "Actions that require explicit approval" bullet list (no surprise additions or omissions).
- Edge case: every H5 row's class name appears verbatim in at least one skill's action emit path (after U3, U4, U5, U7 land).

**Verification:**
- Reading TOOLS.md, Danielle can answer "why did X auto-happen, or fail to happen?" by finding the row and its rationale.

---

- U2. **LOG schema + audit-ledger durable substrate + memory-consolidation rotation**

**Goal:** Define the three new LOG tags AND introduce `workspace/audit-ledger.md` as the durable record that survives the nightly LOG clear. Without a durable substrate, weekly-review's promotion counter (U6) has no data to count from after night 1 — feasibility review F1/F2.

**Requirements:** R5, R8 (and unblocks R14, R15)

**Dependencies:** None (parallel to U1).

**Files:**
- Create: `workspace/audit-ledger.md` (the durable rotated record)
- Modify: `skills/memory-consolidation.md` (add a rotation step before clearing LOG.md)
- Modify: `workspace/TOOLS.md` (add "LOG.md tag conventions" subsection beneath Action trust levels)
- Test: manual verification with a synthetic LOG.md and a 7-day rotation walkthrough.

**Approach:**
- Document four new tags in a new "LOG.md tag conventions" subsection of TOOLS.md:
  - `[AUTO-APPLIED]` — system performed an H5 action. Format: `[YYYY-MM-DD HH:MM] [AUTO-APPLIED] class:<class-name> file:<repo-rel-path> what:"<one-line-summary>" source:<originating-id-or-trigger>`.
  - `[FILED-NO-ACTION]` — slack-inbox classified an item as triage_no. Format: `[YYYY-MM-DD HH:MM] [FILED-NO-ACTION] class:<class-name> file:<repo-rel-path> what:"<one-line-summary>" source:<slack-msg-ts>`. When the item has no dedicated resource folder (fallback case), `file:workspace/LOG.md` and the `what:` field carries the original message text in quotes — distinguishable from AUTO-APPLIED by the tag itself.
  - `[AUTO-APPLIED-REVERTED]` — slack-inbox reversed an H5 action. Format: `[YYYY-MM-DD HH:MM] [AUTO-APPLIED-REVERTED] reverts:<original-AUTO-APPLIED-date-time> class:<class-name> reason:"<short-text>"`.
  - `[REACTION-PROCESSED]` — slack-inbox processed a Slack reaction (♻️ / 👍 / ✕) on one of its own past messages. Format: `[YYYY-MM-DD HH:MM] [REACTION-PROCESSED] msg:<msg-ts> action:<revert|confirm|skip>`. Idempotency marker — prevents re-processing the same reaction.
- Create `workspace/audit-ledger.md` with this shape:
  ```
  # Audit Ledger — durable record of auto-applied / filed-no-action / reverted entries
  
  Append-only. Rotated each night by memory-consolidation BEFORE LOG.md is cleared.
  Used by weekly-review for H4→H5 promotion proposal counting (R14, R15).
  Pruned monthly: entries older than 90 days are summarized into a `## Archive` block, one-line-per-class with counts.
  
  ## Active rotation window
  
  <!-- Newest first; oldest pruned monthly -->
  ```
- Update `skills/memory-consolidation.md` Step 4 (the LOG.md clear). Insert a rotation step ahead of the clear:
  1. Read LOG.md.
  2. For every line matching `[AUTO-APPLIED]`, `[FILED-NO-ACTION]`, `[AUTO-APPLIED-REVERTED]`, `[REACTION-PROCESSED]` — append to `workspace/audit-ledger.md` "Active rotation window" section.
  3. THEN clear LOG.md to its template.
- The four new tags do NOT route to MEMORY.md "Waiting on human" — they're audit trail, not waiting items. Update memory-consolidation rescue rules to explicitly skip them.
- Add a monthly pruning step to memory-consolidation: when audit-ledger.md exceeds ~500 lines, summarize entries older than 90 days into the `## Archive` block (one line per class with cumulative counts) and remove the originals from "Active rotation window."
- Document in memory-consolidation.md why these tags rotate to audit-ledger instead of MEMORY.md (one-line comment) so a future curator doesn't conflate the two.

**Patterns to follow:**
- Tag format mirrors existing `[YYYY-MM-DD] [TAG] text` shape from MEMORY.md "Waiting on human" entries.
- Rotation pattern follows the existing MEMORY.md 150-line cap + Archive block pattern.

**Test scenarios:**
- Happy path: synthetic LOG.md with three `[AUTO-APPLIED]` lines, one `[FILED-NO-ACTION]`, one `[NEEDS-APPROVAL]` — after memory-consolidation runs: NEEDS-APPROVAL goes to MEMORY.md "Waiting on human"; the four new-tag lines land in audit-ledger.md "Active rotation window"; LOG.md is cleared.
- Happy path (pruning): synthetic audit-ledger.md with 600+ entries spanning 4 months — after the monthly pruning step, entries older than 90 days collapse to summary lines in `## Archive`, "Active rotation window" stays under ~500 lines.
- Edge case: a malformed `[AUTO-APPLIED]` line (missing `class:` field) — memory-consolidation skips rotation for that line, logs a one-line warning to LOG.md before clearing.
- Edge case: audit-ledger.md does not exist (first run) — memory-consolidation creates it with the template above and proceeds.
- Integration: after U3 emits a real `[AUTO-APPLIED]` line at 11am, memory-consolidation runs at 9pm, audit-ledger.md gains the line, weekly-review on Sunday counts it.

**Verification:**
- After a full day plus nightly run, audit-ledger.md "Active rotation window" reflects the day's auto-applies and filed-no-actions; LOG.md is empty (template only); MEMORY.md "Waiting on human" only contains genuine waiting items.

---

- U3. **slack-inbox: triage_no classification + AUTO-APPLIED emit path**

**Goal:** Teach slack-inbox to classify a `triage_no` bucket and to emit `[AUTO-APPLIED]` lines for H5 actions.

**Requirements:** R5, R8

**Dependencies:** U1 (ledger to read), U2 (schema to emit).

**Files:**
- Modify: `skills/slack-inbox.md`
- Test: manual verification with synthetic Slack message fixtures.

**Approach:**
- Add a `triage_no` row to the slack-inbox classification table: "An auto-receipt already in Monarch, an accepted calendar invite, a duplicate notification, a newsletter, an automated digest" → "Append `[FILED-NO-ACTION]` line to LOG.md per schema; do NOT post to Slack."
- Update existing classification rows to consult the trust ledger: when a message classifies into shopping/school/helper/maintenance/general-log/needs-review, the resulting action's class name is determined by the row (e.g., shopping → `slack-inbox/file-shopping-item`). The skill then looks up that class's H-level in TOOLS.md trust ledger:
  - If H5: perform the file append AND emit `[AUTO-APPLIED]` LOG line.
  - If H4: perform the file append AND log the source as a regular entry (existing behavior).
  - If H4-forever or class missing: default to H4 propose path.
- Add the audit ref footer to any Slack post: when slack-inbox posts to `#familyos` (e.g., the PENDING-ASANA reminder, an apply confirmation), include a one-line footer `audit ref: <YYYY-MM-DD> / <class-name>` to enable the U4 reaction-based revert to find the LOG entry without a brittle ID linkage.
- Add a new directive parser branch for `triage_no` confidence: if the slack-inbox classifier is uncertain (e.g., the message has multiple plausible classifications), default to NOT emitting triage_no — fall through to existing `[NEEDS-REVIEW]` path. Auto-apply is opt-in by classifier confidence.
- Document SOUL.md voice considerations: triage_no never posts to Slack (zero noise); AUTO-APPLIED emit produces no Slack reply (the daily digest is the FYI surface).

**Patterns to follow:**
- Existing slack-inbox classification table and append-rules block.
- LOG.md tag format from U2.

**Test scenarios:**
- Covers AE1. Happy path: Danielle drops "buy oat milk" → slack-inbox classifies as shopping → looks up `slack-inbox/file-shopping-item` (H5) → appends "oat milk" to `resources/shopping/current-list.md` AND appends `[2026-04-27] [AUTO-APPLIED] class:slack-inbox/file-shopping-item file:resources/shopping/current-list.md what:"oat milk" source:slack-msg-abc123` to LOG.md → no Slack reply.
- Happy path: Danielle drops a Stripe receipt → classifies as triage_no → appends `[FILED-NO-ACTION]` line to LOG.md → no Slack reply.
- Edge case: ambiguous message ("dentist") could be school, helper, or maintenance → classifier confidence is below threshold → falls through to `[NEEDS-REVIEW]` (does NOT auto-classify or auto-apply).
- Edge case: the trust ledger row for a class is missing → skill defaults to H4 propose path AND posts a one-line warning to LOG.md flagging the missing row for the next weekly review.
- Error path: TOOLS.md is unreadable (file missing or malformed) → skill preserves pre-feature behavior: classes that were auto-filing today (shopping/school-notice/helper/maintenance/general-log/needs-review filing into resource files) continue to file without confirmation; new H5 classes that didn't previously auto-apply default to H4 propose. Embedded fallback table in slack-inbox.md captures pre-feature defaults explicitly. Logs a one-line warning + posts a single Slack message ("⚠️ TOOLS.md unreadable — running on embedded defaults until restored") so the curator knows to fix it.
- Integration: after a triage_no classification, the next U5 triage-digest run includes the entry in its "Filed without surfacing" section.

**Verification:**
- Synthetic walkthrough of 5 sample inbox messages produces the expected LOG.md mutations and zero spurious Slack posts.

---

- U4. **slack-inbox: reaction-reading for ♻️ rollback and single-item emoji-confirm**

**Goal:** Add Slack reaction reading to the slack-inbox skill so (a) ♻️ on its own past messages rolls back the referenced AUTO-APPLIED action, and (b) 👍 / ✕ on a single-item PENDING-ASANA reminder serves as confirm/skip.

**Requirements:** R10, R11, R12, R13

**Dependencies:** U2 (schema), U3 (audit-ref footer pattern).

**Files:**
- Modify: `skills/slack-inbox.md`
- Test: manual verification — post a Slack message with the audit-ref footer, react ♻️, observe LOG mutation.

**Approach:**
- Step 1 (verification): on the first hourly run after this unit ships, the skill reads the most recent FamilyOS message in `#familyos` via `slack_read_channel` and confirms the response includes a `reactions` array. If the field is absent, fall back to typed `revert <date> <class>` directives only and log the issue. (One-time check — once verified, the path is stable.)
- Step 2 (♻️ rollback): in each hourly run, fetch reactions on slack-inbox's own past messages from the last 7 days. For any message bearing a new ♻️ reaction:
  - Parse the message's `audit ref:` footer to extract `<date>` and `<class>`.
  - **Single-action message** (the audit-ref footer points to one specific class — e.g., a PENDING-ASANA reminder, a future per-action confirmation post): find the matching `[AUTO-APPLIED]` line in LOG.md or `workspace/audit-ledger.md` (after rotation). Reverse the change: read the `file:` and `what:` fields, perform the inverse edit (delete the appended line; restore the prior tag; etc.). Append `[AUTO-APPLIED-REVERTED]` to LOG.md per schema. Reply in `#familyos` with one line: `Reverted: <what:>. (auto-applied <date>; reversed <today>)`.
  - **Multi-action message** (the triage-digest is one Slack message with N bullets across two sections — single ♻️ has no per-bullet granularity): instead of attempting a guess, reply in `#familyos` with a numbered enumeration of the digest's reversible bullets (auto-applied entries only; FILED-NO-ACTION entries are not "reversible" as a concept since no resource state was changed beyond a log line) and prompt: `Which to revert? Reply revert N (or revert N,M / revert all-auto / cancel).` This is the disambiguation flow. Process the user's typed reply via Step 4's typed-revert path.
- Pre-feature messages: if a ♻️ lands on a slack-inbox post that has no `audit ref:` footer (older than this feature's deploy), skip silently — log `[REACTION-PROCESSED] msg:<msg-ts> action:skip-pre-feature`.
- Lookup miss: if the audit-ref footer points to an `[AUTO-APPLIED]` entry that's nowhere in LOG.md or audit-ledger.md (write race / very old / never-existed), reply: `Cannot revert — no audit record found for <class> on <date>. The action may not have been applied, or the audit entry was pruned (only the last 90 days are retained in audit-ledger).`
- Step 3 (single-item emoji-confirm): modify the PENDING-ASANA stall reminder. When the bundle has exactly one item, post format becomes:
  ```
  📝 Pending: **<task title>** — <due date or "no due date"> — logged <YYYY-MM-DD>
  React 👍 to confirm or ✕ to skip.
  audit ref: <YYYY-MM-DD> / slack-inbox/post-pending-asana-reminder
  ```
  In the next hourly run, read reactions on past stall reminder messages: 👍 → process as `confirm 1`; ✕ → process as `skip`. Multi-item bundles retain the existing typed `confirm 1,3` / `confirm all` / `skip` format unchanged.
- Step 4 (typed revert fallback): parse `revert <YYYY-MM-DD> <class>` (or `revert last`) directives in `#familyos` messages — same logic as ♻️ but addresses by typed date+class instead of reaction.
- Once a message has been processed (revert applied OR confirm applied), append `[YYYY-MM-DD] [REACTION-PROCESSED] msg:<msg-ts> action:<revert|confirm|skip>` to LOG.md so the same reaction isn't re-processed in subsequent hourly runs. Memory-consolidation rescue rules in U2 are extended (one more skip case).
- Document in slack-inbox.md why R13 (no emoji-confirm for weekly-review apply directives) is intentionally out: weekly-review proposals can be many; `apply 1,3` is more expressive than emoji.

**Patterns to follow:**
- Existing PENDING-ASANA bundle posting in slack-inbox.md (preserved format for multi-item).
- Existing `confirm N` / `apply N` / `skip` directive parser logic.

**Test scenarios:**
- Covers AE3. Happy path (single-item Asana): one PENDING-ASANA item stalled → reminder posts in 1-item format → Danielle reacts 👍 → next hourly run creates the Asana task and removes the PENDING-ASANA line from LOG.md → posts confirmation in #familyos.
- Covers AE4. Happy path (rollback): yesterday's LOG had `[AUTO-APPLIED] class:slack-inbox/file-shopping-item file:resources/shopping/current-list.md what:"oat milk"` → today's triage-digest Slack post mentions the entry → Danielle reacts ♻️ on that digest line's source message → next hourly run removes "oat milk" line from current-list.md, appends `[AUTO-APPLIED-REVERTED]` to LOG.md, replies "Reverted: oat milk."
- Happy path (typed revert): Danielle posts `revert 2026-04-26 slack-inbox/file-shopping-item` → next hourly run executes the same reversal, confirms in Slack.
- Edge case (multi-item PENDING-ASANA): two items stalled → existing typed-directive bundle format used unchanged → 👍 reaction NOT honored (only `confirm 1,2` / `confirm all` / `skip` typed).
- Edge case (reaction on non-FamilyOS message): a 👍 on a message Danielle posted (not slack-inbox bot output) → ignored, no action.
- Edge case (idempotency): the same ♻️ reaction is read in two consecutive hourly runs → second run sees `[REACTION-PROCESSED]` LOG line for that msg-ts → no double-revert.
- Edge case (revert target already gone): `revert 2026-04-25 ...` but the LOG entry was swept by the nightly clear → reply "Cannot revert — original AUTO-APPLIED entry is no longer in LOG.md (cleared 2026-04-26 evening)."
- Error path (Slack MCP doesn't expose reactions): step 1 verification fails → ♻️ path disabled, only typed `revert` directive available → log warning in OS-LOG.md for next weekly review.

**Verification:**
- Single-item Asana stall happens once; emoji-react resolves it; LOG/Asana are consistent.
- An H5 auto-applied change can be rolled back via ♻️ or typed `revert` and the file state matches pre-application.

---

- U5. **triage-digest skill (new)**

**Goal:** Daily 2:00 PM Slack post summarizing what the system did without surfacing.

**Requirements:** R6, R7, R9

**Dependencies:** U2 (LOG schema), U3 (slack-inbox emits the entries).

**Files:**
- Create: `skills/triage-digest.md`
- Modify: `FamilyOS/familyos.plugin` (register the new skill in the plugin manifest)
- Test: manual verification by running the skill against a synthetic LOG.md.

**Approach:**
- Cadence: daily at 2:00 PM. Document in the skill prompt that this will move to weekly (Friday 2:00 PM) once Danielle has confidence — switching is a one-line edit + Cowork scheduled-task cron change.
- Inputs: entries from the past 24 hours tagged `[AUTO-APPLIED]` or `[FILED-NO-ACTION]`, read from BOTH `LOG.md` (today's pre-rotation entries) AND `workspace/audit-ledger.md` "Active rotation window" (entries already rotated by last night's memory-consolidation). Together they cover the full 24h window across the rotation boundary. (Past 7 days when migrated to weekly cadence — same union.)
- Outputs: one Slack post to `#familyos` with two sections, each preceded by a Slack-bold header, each with bulleted entries:

  ```
  📋 *Triage digest — <today's date>*

  *Filed without surfacing*
  • <YYYY-MM-DD> — <what:> filed to <file:> (source: <source>)
  • ...

  *Auto-applied*
  • <YYYY-MM-DD> — <class>: <what:> in <file:>
  • ...
  ```
- SOUL.md compliance: skip empty sections (no "nothing today!"); if BOTH sections are empty, the skill posts nothing at all.
- Audit-ref footer on the digest message itself: `audit ref: <YYYY-MM-DD> / triage-digest/post-daily-digest`. This anchors U4's ♻️ disambiguation flow — a ♻️ on the digest is recognized as a multi-action message and triggers the numbered enumeration reply, not a single-action revert.
- The digest post is **system communication, not a ledger-governed action** (per U1's ledger scope). Triage-digest runs as a scheduled task and posts unconditionally — there's no propose-confirm gate. The audit-ref footer is purely a navigation aid for U4's rollback flow.
- Failure mode: if LOG.md is unreadable, post a one-line "Triage digest unavailable today — LOG.md unreadable. Will retry tomorrow." message and continue.

**Patterns to follow:**
- Existing morning-brief skill's Slack post format (Slack-bold headers, terse bullets, dates spelled out).
- Existing "skip empty sections" rule from SOUL.md (mirror the morning-brief approach).

**Test scenarios:**
- Covers AE2. Happy path: 3 H5 actions and 2 triage_no entries in past 24h → digest posts both sections with 5 total bullets.
- Edge case: zero entries in either category → skill posts nothing (does not post "Triage digest — nothing today").
- Edge case: only auto-applied entries (no triage_no) → digest posts only the *Auto-applied* section.
- Edge case: an entry in LOG.md older than 24h sneaks in via timezone weirdness → digest filters strictly on the date field, ignores the older entry.
- Integration: after the digest posts, ♻️ on any line's source message resolves to the correct LOG entry via U4's audit-ref parser.

**Verification:**
- Synthetic 24h LOG.md window produces the expected post; empty window produces no post.
- After 6 weeks of daily digests with no observed misclassifications, Danielle can switch the cadence to Friday 2:00 PM with a one-line edit.

---

- U6. **weekly-review: H4→H5 promotion proposals**

**Goal:** Extend weekly-review to count confirmations-without-edits per H4 action class and emit promotion proposals when threshold is met.

**Requirements:** R14, R15, R16, R17

**Dependencies:** U1 (ledger to read), U2 (LOG schema), U3 (auto-applied entries to count).

**Files:**
- Modify: `skills/weekly-review.md`
- Test: manual verification with synthetic 7+ weeks of LOG entries.

**Approach:**
- Add a new sub-step to weekly-review's "Step 1: Gather this week's signals" — per-class confirmation counter:
  - Read TOOLS.md trust ledger; for every row with H-level == `H4` (excluding H4-forever), capture `class` and `since:` date.
  - Read `workspace/audit-ledger.md` "Active rotation window" — the durable record of `[AUTO-APPLIED]` and `[AUTO-APPLIED-REVERTED]` entries rotated nightly by memory-consolidation (per U2). Also read the current-week LOG.md for entries that haven't been rotated yet (today's entries). For each H4 class with a `since:` date, count entries since `since:` per class. **Confirmation = an `[AUTO-APPLIED]` entry whose class matches AND no corresponding `[AUTO-APPLIED-REVERTED]` line exists referencing it.** Heuristic refinement: if Danielle directly edited the `file:` named in an entry within 24h of the entry's timestamp, count it as an edit (resets the count) — detect via `git log --follow <file>` if the repo is git-tracked, or via mtime comparison as a fallback.
  - Settle the exact grep/parser in this unit by walking through a synthetic 2-week audit-ledger.md fixture before the first real promotion proposal fires.
- Add a new proposal type to "Step 3: Generate improvement proposals":
  - When a class has `confirmations >= 6` since `since:`, emit numbered proposal:
    ```
    **Proposal N: Promote <class> to H5**
    - Signal: <N> confirmations of <class> without edits since <since:>
    - Change: edit workspace/TOOLS.md Action trust levels — change <class> row from H4 to H5; update Since to today's date.
    - Type: skill prompt update
    - Auto-apply: yes
    ```
- The threshold (6) is a single tunable in the weekly-review skill prompt, declared explicitly at the top of the new sub-step so a future Danielle can change it with one edit.
- Document R17 (manual demotion) in the weekly-review prompt: the skill never proposes demotion automatically. Demotion is Danielle's call — direct edit of TOOLS.md.
- The promotion proposal's `apply N` path (handled by existing slack-inbox apply-directive logic) edits the TOOLS.md row. No new applier code needed — the existing weekly-review apply mechanism handles text edits to workspace files (the `Auto-apply: yes` rule).

**Patterns to follow:**
- Existing weekly-review proposal format from `skills/weekly-review.md` step 3.
- Existing `Auto-apply: yes` semantics already supported by slack-inbox apply handler.

**Test scenarios:**
- Covers AE5. Happy path: 7 weeks of synthetic LOG entries showing 7 confirmations of `meal-planner/append-shopping-ingredients` without intervening edits → weekly-review emits the promotion proposal verbatim per the format above.
- Happy path: Danielle replies `apply N` in Slack → existing apply handler edits TOOLS.md row → `meal-planner/append-shopping-ingredients` is now H5 → next meal-planner run auto-applies.
- Covers AE6. Edge case: `slack-inbox: send confirmation email to school` is `H4-forever` → weekly-review's class iterator skips it → no promotion proposal, ever.
- Edge case: a class hits 6 confirmations but Danielle edited the source file once in the past week → counter resets → no proposal.
- Edge case: a class was promoted to H5 last week, then auto-applied 2 things this week → not eligible for re-promotion (already H5); not surfaced.
- Edge case: a class is missing from TOOLS.md ledger → weekly-review notes the gap as a separate proposal ("Add missing class to ledger"), does not attempt promotion math on it.
- Integration: 6-week walkthrough simulating real cadence shows 1–2 promotions surfaced per quarter at most, not a flood.

**Verification:**
- Synthetic LOG fixture across 7 weeks produces exactly the expected proposal; non-eligible classes are correctly skipped.

---

- U7. **Per-skill prompt updates: ledger reference + class declarations**

**Goal:** Update every existing skill prompt to reference the trust ledger and declare its action classes inline. Ensures every action a skill emits has a discoverable name that matches a ledger row.

**Requirements:** R4 (each skill references the ledger; no duplication of H-level info)

**Dependencies:** U1 (ledger exists).

**Files:**
- Modify: `skills/meal-planner.md`
- Modify: `skills/school-triage.md`
- Modify: `skills/morning-brief.md`
- Modify: `skills/memory-consolidation.md` (already touched in U2; consolidate edits)
- Modify: `skills/housekeeper-weekly.md` (if present in repo; otherwise note absence)
- Modify: `skills/weekly-review.md` (already touched in U6; consolidate edits)
- Modify: `skills/slack-inbox.md` (already touched in U3, U4; consolidate edits)
- Test: manual review — each skill's action emit paths name a class that exists in TOOLS.md.

**Approach:**
- Add a one-line reference at the top of each skill prompt: `**Action trust:** see workspace/TOOLS.md → Action trust levels for which actions auto-apply (H5) vs. propose (H4) vs. require explicit approval (H4-forever).`
- For each skill, locate every state-mutating action and annotate it inline in prose with `(class: <class-name>)`. Example for meal-planner.md: "After meal plan is finalized, append new ingredients to `resources/shopping/current-list.md` (class: `meal-planner/append-shopping-ingredients`)."
- The annotation is informational — the skill consults TOOLS.md at decision time; the inline class name is a readability aid for the curator and a checksum for plan completeness.
- Skills do NOT duplicate the H-level itself — only the class name. H-level lives only in TOOLS.md.
- Verify: after this unit, every class declared in TOOLS.md ledger appears verbatim in at least one skill's annotation, and vice versa (no orphans on either side).

**Patterns to follow:**
- Existing skill prompts' top-matter format (e.g., the "How to use" preamble in slack-inbox.md).

**Test scenarios:**
- Happy path: every skill reads and applies the ledger correctly during a manual walkthrough of one synthetic invocation per skill.
- Edge case: a skill action whose class is annotated but missing from the ledger → the skill defaults to H4 (per U3's fallback rule) AND the next weekly-review surfaces a "missing ledger class" proposal (per U6's edge case).
- Coverage check: grep for `(class: ` annotations across `skills/*.md` and confirm every unique class name maps 1:1 to a TOOLS.md row.

**Verification:**
- `comm -23 <(grep -ohE 'class: [a-z][a-z0-9-]+/[a-z0-9-]+' skills/*.md | sort -u) <(awk '/Action trust levels/,0' workspace/TOOLS.md | grep -ohE '`[a-z][a-z0-9-]+/[a-z0-9-]+`' | tr -d '`' | sort -u)` returns empty (no orphan annotations).
- Same comm in reverse returns empty (no orphan ledger rows).
- Regex anchors on the slash-separator class-name format (e.g., `slack-inbox/file-shopping-item`) — not the legacy space-colon format that earlier drafts of this plan used.

---

## System-Wide Impact

- **Interaction graph:** All 7 skills consult `workspace/TOOLS.md` at action-emission time. The hourly slack-inbox skill gains reaction-reading paths; the daily memory-consolidation skill gains tag-skip rules; the weekly-review skill gains a new proposal type. The new triage-digest skill is purely consumptive (reads LOG.md, posts to Slack).
- **Error propagation:** TOOLS.md unreadable → all skills default to H4 (no auto-apply) until ledger is restored. LOG.md unreadable → triage-digest posts a one-line failure note; weekly-review's promotion-counter falls back to "no proposals this cycle"; slack-inbox continues classifying but logs to a stderr-equivalent (its own classification audit).
- **State lifecycle risks:** The audit-ref footer is the load-bearing connection between Slack messages and LOG entries for rollback. If a Slack post is edited in a way that strips the footer, ♻️ rollback won't find the LOG entry (typed `revert` is the fallback). Document this in U4.
- **API surface parity:** None — TOOLS.md is the only governance surface. Per-skill annotations are documentation, not contracts.
- **Integration coverage:** The end-to-end paths most likely to fail in subtle ways: (a) classifier confidence → fall through to H4 → user expects auto-apply but sees a propose; (b) LOG entry swept by nightly clear before user reacts ♻️ → revert returns a "cannot find original" reply. Both are documented behavior, not bugs.
- **Unchanged invariants:** TOOLS.md "Actions that require explicit approval" list semantics are preserved (those bullets become H4-forever rows). The existing `apply N` / `confirm N` / `skip` directive grammar is preserved (additions are `revert`, ♻️, 👍, ✕ — all additive, none replace existing syntax). MEMORY.md "Waiting on human" rescue mechanism is unchanged (the new tags are explicitly excluded; nothing existing is removed).

---

## Risks & Dependencies

| Risk | Mitigation |
|------|------------|
| Slack MCP doesn't expose `reactions` field on messages | U4 step 1 verification gate; ♻️ path disabled gracefully if absent; typed `revert <date> <class>` is the always-available fallback per R11 |
| Classifier emits triage_no incorrectly (filing something that needed action without surfacing) | Daily digest at 2:00 PM is the safety net — Danielle sees the FILED-NO-ACTION list within hours and can react ♻️ on the digest (triggers disambiguation flow) OR post typed `revert` |
| Auto-apply produces a wrong file edit (the 30% safe-failure case) | Same as above — ♻️ on single-action messages reverts in one click; ♻️ on multi-bullet digest triggers numbered disambiguation reply (`revert N`); audit-ledger.md preserves the inverse for up to 90 days |
| Nightly LOG clear sweeps `[AUTO-APPLIED]` entries before weekly-review can count them (feasibility F1/F2) | U2 introduces `workspace/audit-ledger.md` rotated by memory-consolidation BEFORE the LOG clear; weekly-review reads audit-ledger as durable substrate |
| ♻️ on multi-bullet triage-digest message has no per-bullet granularity (feasibility F4) | Disambiguation flow in U4: ♻️ triggers a numbered enumeration reply; user picks via typed `revert N`. Single-action messages keep the one-click behavior |
| TOOLS.md unreadable on a transient infra blip silently regresses currently-auto-filing skills (feasibility F7) | U3 fallback is "preserve pre-feature behavior" via embedded defaults table, NOT "force H4." Posts a one-line warning to Slack so curator sees the degradation |
| TOOLS.md ledger and per-skill annotations drift out of sync | U7's coverage check (orphan-detection grep, slash-aware) is run as part of weekly-review signals; mismatches produce a "ledger drift" proposal automatically |
| Promotion proposals fire too often, creating new noise | Threshold of 6 is conservative (≈6 weeks for weekly skills); R16 makes it tunable; if first quarter shows >2 promotion proposals/cycle, raise threshold |
| All H5-from-day-one classes flip behavior on day one (feasibility F10) | U1 phased-rollout convention: ledger ships with H5-from-day-one rows annotated `(seed; revisit week 1)` in the Rationale column. First weekly review post-launch surfaces any wrong calls as a routine signal. The H5-from-day-one set is intentionally narrow — text appends to workspace files only, not Slack posts (those start at H4). |
| `[REACTION-PROCESSED]` idempotency markers cleared nightly cause re-processing of cross-day reactions | U2 explicitly rotates `[REACTION-PROCESSED]` to audit-ledger.md alongside the other tags; U4 reads both LOG.md AND audit-ledger.md when checking idempotency |
| Memory-consolidation accidentally rotates a malformed AUTO-APPLIED line and corrupts audit-ledger | U2 test scenario covers malformed-line case: skip + warn instead of corrupt + propagate |

---

## Documentation / Operational Notes

- TOOLS.md becomes the single most-changed canonical file in this plan. Once shipped, the "Action trust levels" section is the answer to "why did this thing auto-happen, or fail to happen?" — link it from the FamilyOS README.
- Cowork scheduled task: triage-digest needs a new daily 2:00 PM slot. When Danielle migrates to weekly cadence, the change is one cron field edit (no skill-prompt change beyond updating the documented cadence note).
- The first week after this lands is a calibration window — Danielle should expect to ♻️ a small number of incorrect auto-applies. That's the system working as designed (30% safe-failure principle); it's not a bug.

---

## Sources & References

- **Origin document:** [docs/brainstorms/2026-04-26-trust-gradient-requirements.md](docs/brainstorms/2026-04-26-trust-gradient-requirements.md)
- **Ideation document:** [docs/ideation/2026-04-26-familyos-features-ideation.md](docs/ideation/2026-04-26-familyos-features-ideation.md)
- **Helpfulness ladder source:** https://medium.com/helpful-com/how-to-be-an-effective-early-stage-employee-hint-be-helpful-e681b456a01f
- **Existing FamilyOS rails referenced:** `workspace/TOOLS.md`, `workspace/LOG.md`, `workspace/MEMORY.md`, `workspace/OS-LOG.md`, `workspace/SOUL.md`, all files under `skills/`.
- **Trust ledger external prior art (cataloged in origin):** LangChain EAIA's three-bucket triage; Reclaim.ai defense-level graduation; Claire Vo's progressive-trust pattern.
