---
date: 2026-04-26
topic: trust-gradient
---

# Trust Gradient — Helpfulness-graded action approval for FamilyOS

## Problem Frame

The Slack-inbox → Asana confirmation stall has been the only OS friction surfaced in two consecutive weekly reviews (Week of 2026-04-13, Week of 2026-04-20). Every FamilyOS skill currently operates at one approval level: propose-then-confirm, regardless of whether the action is "edit a markdown file" or "send an email." That uniform gate is the right shape for high-blast-radius actions but turns low-blast-radius actions into a nag-shaped backlog. The system needs a way for low-stakes actions to act with confidence (and FYI Danielle in a digest) while high-stakes actions stay strictly gated — without inventing new persistence, new Slack channels, or new propose/apply mechanics.

The framing principle is the [Helpfulness ladder](https://medium.com/helpful-com/how-to-be-an-effective-early-stage-employee-hint-be-helpful-e681b456a01f) (H1 = ask without pre-work; H4 = pre-work + recommendation + meta-conversation; H5 = take action confidently and FYI), balanced by the 30% safe-failure principle: in low-risk areas, accept that ~30% of decisions may need rollback as the cost of speed.

---

## Actors

- A1. **Danielle (curator):** primary FamilyOS user. Reviews triage_no digests, applies promotion proposals, reverts auto-applied actions when needed, edits TOOLS.md when the trust ledger needs adjusting.
- A2. **Slack-inbox skill:** hourly job that classifies inbound messages, files them, surfaces apply/confirm directives, processes Slack reactions on its own messages.
- A3. **Weekly-review skill:** Sunday job that proposes OS improvements; gains a new responsibility — counting confirmations-without-edits per action class and proposing H4→H5 promotions.
- A4. **All other FamilyOS skills (meal-planner, school-triage, housekeeper-weekly, morning-brief, memory-consolidation):** consume the trust ledger to determine which of their action classes auto-apply vs. propose.
- A5. **Triage-digest skill (new):** daily (then weekly) Slack post summarizing what was filed without surfacing.

---

## Key Flows

- F1. **Auto-apply on H5 action**
  - **Trigger:** A skill produces an action whose class is marked H5 in `workspace/TOOLS.md` Action trust levels.
  - **Actors:** A2/A4, A1
  - **Steps:** Skill performs the action (e.g., appends a shopping item, retags a LOG entry). Appends to `LOG.md` a line tagged `[AUTO-APPLIED]` with date, class, what changed, and source. No Slack post.
  - **Outcome:** Change is in the file. Visible to Danielle in the next triage_no digest and via the LOG.md `[AUTO-APPLIED]` lines.
  - **Covered by:** R1, R2, R5, R8

- F2. **Triage_no FYI (filed without surfacing)**
  - **Trigger:** Slack-inbox classifies a message as `triage_no` (auto-receipts already in Monarch, accepted-cal-invites, duplicate notifications, newsletters, etc.) at high confidence.
  - **Actors:** A2, A1
  - **Steps:** Slack-inbox files the message into the appropriate resource folder (or `LOG.md` if no folder fits) tagged `[FILED-NO-ACTION]`. No Slack reply, no proposal. Triage-digest skill (A5) runs daily at 2:00 PM and posts a single Slack message summarizing what was filed without surfacing in the past 24h.
  - **Outcome:** Mental load preserved (no decision asked of Danielle), with a daily checkpoint to spot-check.
  - **Covered by:** R3, R4, R9

- F3. **H4 propose-confirm with reduced friction (single-item Asana batch)**
  - **Trigger:** Slack-inbox PENDING-ASANA stall reminder runs and there is exactly one stalled item.
  - **Actors:** A2, A1
  - **Steps:** Instead of the standard "reply `confirm 1` (or `confirm all`, or `skip`)" format, post: "React 👍 to confirm or ✕ to skip." Hourly inbox job reads reactions on its own past message; on 👍, processes as `confirm 1`; on ✕, as `skip`. Multi-item bundles still use typed directives.
  - **Outcome:** Fewer keystrokes for the most common stall case (single-item).
  - **Covered by:** R6

- F4. **Auto-apply rollback via Slack reaction**
  - **Trigger:** Danielle reacts ♻️ on a Slack message that referenced an `[AUTO-APPLIED]` LOG entry, OR posts `revert <date> <class>` in the inbox channel.
  - **Actors:** A1, A2
  - **Steps:** Slack-inbox finds the `[AUTO-APPLIED]` LOG entry, reverses the file edit (delete the appended line, restore prior tag, etc.), appends `[AUTO-APPLIED-REVERTED]` to LOG.md with reason "user revert via Slack." Posts a brief confirmation ("Reverted: shopping list addition of 'oat milk' on 2026-04-26.").
  - **Outcome:** 30% safe-failure becomes operational — Danielle can roll back without leaving Slack.
  - **Covered by:** R10, R11

- F5. **Curator-promoted H4→H5 graduation**
  - **Trigger:** Weekly-review (Sunday) detects an action class with ≥6 H4 confirmations-without-edits since its last promotion (or since the class was introduced).
  - **Actors:** A3, A1
  - **Steps:** Weekly-review adds a numbered proposal: "Promote class X from H4 to H5 — observed N successful confirmations without edits since [date]. Auto-apply: yes (text edit to TOOLS.md)." Danielle replies `apply N` in Slack. Slack-inbox edits the TOOLS.md Action trust levels table to mark X as H5.
  - **Outcome:** Class graduates; future actions of class X auto-apply via F1.
  - **Covered by:** R12, R13

---

## Requirements

**Trust ledger (the canonical artifact)**

- R1. Extend `workspace/TOOLS.md` with a new section **"Action trust levels"** containing a single table: `Action class | H-level | Rationale`. The table is the canonical source of truth for whether any FamilyOS skill action auto-applies, proposes, or stays absolutely gated.
- R2. The table's rows enumerate action classes (not micro-actions). Every existing FamilyOS skill action falls into one or another class. Initial seed values must include at minimum: slack-inbox file appends to shopping/school/helpers/maintenance (H5 from day one — already reversible text appends), slack-inbox `[NEEDS-REVIEW]` tagging (H5), meal-planner appends to current-list.md (H4 baseline, eligible for H5), school-triage appends to school/notices.md (H4 baseline, eligible for H5), Asana task creation (H4 forever), email/Slack/SMS sending (H4 forever), calendar writes (H4 forever), Maple pushes (H4 forever).
- R3. Each row's H-level is one of three values: `H5`, `H4`, or `H4-forever`. `H4-forever` means the class is never eligible for promotion regardless of track record (anything in TOOLS.md "Actions that require explicit approval" maps here).
- R4. Each existing skill prompt that produces actions gains a one-line reference to the trust ledger ("For action H-levels see workspace/TOOLS.md → Action trust levels"). Skills do not duplicate the H-level information — they read from TOOLS.md.

**Triage_no behavior**

- R5. Slack-inbox gains a `triage_no` classification alongside its existing categories. Items in this bucket file to the appropriate resource folder (or LOG.md as fallback) tagged `[FILED-NO-ACTION]` and produce no Slack post.
- R6. A new skill, **triage-digest**, runs daily at 2:00 PM and posts a single Slack message to `#familyos` summarizing `[FILED-NO-ACTION]` entries from the past 24 hours. Format: bulleted list with date, source, what was filed where. If zero entries, the skill posts nothing (per SOUL.md "skip empty sections").
- R7. The triage-digest cadence is intended to move from daily to weekly (Friday 2:00 PM) once Danielle has confidence in the classifier. The cadence is governed by the skill's prompt and is changeable as a text edit (no scheduled-task reconfiguration required beyond updating the cron).

**H5 auto-apply mechanics**

- R8. When a skill performs an H5 action, it appends to `LOG.md` a line tagged `[AUTO-APPLIED]` containing: date, action class, what changed (file + summary), and source (originating Slack message ID, email, or trigger).
- R9. `[AUTO-APPLIED]` LOG entries are first-class for the triage-digest skill: each daily digest includes a section listing them, separate from `[FILED-NO-ACTION]`. Danielle can scan one digest to see everything the system did without asking.
- R10. Slack-inbox handles a ♻️ reaction on any of its own past messages that referenced an `[AUTO-APPLIED]` action. On reaction: find the LOG entry, reverse the change, append `[AUTO-APPLIED-REVERTED]` with reason "user revert via Slack." Reply with a one-line confirmation.
- R11. Slack-inbox also handles a typed `revert <date> <class>` directive in `#familyos` as an equivalent rollback path for cases where no Slack message is conveniently available to react on.

**H4 friction reduction**

- R12. When the slack-inbox PENDING-ASANA stall reminder bundles exactly one item, the post format invites a 👍 / ✕ reaction instead of a typed `confirm 1` / `skip`. Multi-item bundles retain typed directives.
- R13. The same emoji-confirm pattern is **not** extended to weekly-review apply directives in this v1 — weekly review proposals can be many in number, and `apply 1,3` is more expressive than emoji.

**H4→H5 promotion**

- R14. Weekly-review's existing signals step gains a sub-step: count, per action class with status H4 in TOOLS.md (excluding H4-forever), the number of confirmations-without-edits since the class was last promoted (or since it was introduced). Track this by reading LOG.md `[AUTO-APPLIED]` and `[NEEDS-APPROVAL]` lines plus OS-LOG.md applied-proposal records.
- R15. When a class accumulates ≥6 confirmations-without-edits, weekly-review emits a numbered proposal of type `OS change` with `Auto-apply: yes` whose action is "edit TOOLS.md Action trust levels: change row X from H4 to H5." Danielle promotes via `apply N` in Slack.
- R16. The threshold of 6 is a tunable in the weekly-review skill prompt, not hard-coded across files.
- R17. A class that was promoted to H5 can be demoted back to H4 (or H4-forever) by Danielle editing TOOLS.md directly. Weekly-review respects whatever TOOLS.md currently says — promotion is one-directional via proposals, but manual demotion is always available as an escape hatch.

---

## Acceptance Examples

- AE1. **Covers R1, R2, R5, R8.** Given TOOLS.md marks `slack-inbox: file shopping item` as H5, when Danielle drops "buy oat milk" into `#familyos`, slack-inbox appends "oat milk" to `resources/shopping/current-list.md` with no Slack reply, and appends a `[2026-04-27] [AUTO-APPLIED] slack-inbox / file shopping: "oat milk" → resources/shopping/current-list.md (source: Slack msg abc123)` line to LOG.md.

- AE2. **Covers R6, R9.** Given three H5 actions occurred between 2:00 PM 2026-04-26 and 2:00 PM 2026-04-27, when triage-digest runs at 2:00 PM 2026-04-27, it posts one Slack message with two sections — `*Filed without surfacing*` (the FILED-NO-ACTION entries) and `*Auto-applied*` (the three AUTO-APPLIED entries).

- AE3. **Covers R3.** Given the inbox PENDING-ASANA bundle has exactly one stalled item, when slack-inbox posts the stall reminder, the message reads: "Pending: 'Schedule HVAC tune-up'. React 👍 to confirm or ✕ to skip." On 👍, slack-inbox creates the Asana task and removes the PENDING-ASANA line.

- AE4. **Covers R10.** Given an `[AUTO-APPLIED]` LOG entry from this morning (e.g., "added 'oat milk' to shopping list"), when Danielle reacts ♻️ on the corresponding triage-digest line in Slack, slack-inbox removes the "oat milk" line from `resources/shopping/current-list.md`, appends `[AUTO-APPLIED-REVERTED]` to LOG.md, and replies "Reverted: shopping list addition of 'oat milk' on 2026-04-26."

- AE5. **Covers R14, R15.** Given the action class `meal-planner: append to current-list.md` is currently H4 in TOOLS.md and has accumulated 7 confirmations-without-edits since it was introduced 9 weeks ago, when weekly-review runs Sunday, it emits proposal: "Promote `meal-planner: append to current-list.md` from H4 to H5 — 7 confirmations without edits since 2026-02-22. Auto-apply: yes (TOOLS.md edit)." On `apply N`, the TOOLS.md row changes to H5 and future meal-planner appends auto-apply.

- AE6. **Covers R3.** Given the action class `slack-inbox: send confirmation email to school` exists, it is marked `H4-forever` in TOOLS.md regardless of track record, so weekly-review never proposes promoting it.

---

## Success Criteria

- **Confirmation stall pattern stops appearing in OS-LOG weekly review signals.** The only friction flagged in two consecutive cycles disappears within 3 review cycles after the gradient ships.
- **Danielle's reported friction with FamilyOS interaction drops.** Subjective check: she's not avoiding `#familyos` because of the response burden.
- **No silent surprises.** Every H5 action is visible to Danielle in the daily/weekly digest; every revert is one Slack reaction away.
- **Downstream agent handoff:** ce-plan can implement v1 from this document without inventing scope, success criteria, or behavior. Trust ledger format, triage_no flow, H5 auto-apply mechanics, emoji-confirm path, rollback flow, and promotion criteria are all specified to behavior-level.

---

## Scope Boundaries

- Track-record-based **automatic** H4→H5 promotion is explicitly out — promotion is curator-driven via weekly review proposals only, even when the threshold is met.
- Multi-user / multi-curator support is out. The trust ledger has one curator (Danielle); John is not a participant in the system in v1.
- Confidence-scored at-the-edge (per-instance score instead of per-class taxonomy) is rejected — predictability of category-based gradient is a feature here.
- Emoji-confirm is **not** extended to weekly-review `apply N` directives in v1 (multi-item bundles retain typed directives).
- The 30% safe-failure principle is the philosophical floor, not an enforced metric. The system does not measure rollback rate or alert when a class crosses 30% — that is a future enhancement if needed.
- Rollback of multiple stacked AUTO-APPLIED actions in one reaction is out (each ♻️ reverts one entry; multi-revert needs typed `revert` directives).
- Auto-promotion notification ("X just graduated to H5") is out — graduation is visible only via the weekly review apply confirmation.
- A standalone "trust dashboard" surface is out. TOOLS.md table is the dashboard.

---

## Key Decisions

- **Approach A (extend TOOLS.md as the trust ledger)** chosen over B (new TRUST.md), C (per-skill inline annotations), and the confidence-scoring challenger. Rationale: TOOLS.md already governs approval; one canonical artifact for trust + tools; auditable in 30 seconds.
- **Curator-promoted graduation** chosen over track-record auto-promotion, pre-declared static taxonomy, or hybrid. Rationale: plugs directly into existing propose→apply rails; no silent behavior drift; user stays in the loop for one click per promotion.
- **Triage_no digest cadence: daily (2:00 PM) initially, intended to move to weekly (Friday 2:00 PM) when Danielle has confidence.** Rationale: high-frequency observation builds calibration before low-frequency observation can be trusted.
- **H5 rollback via Slack reaction (♻️)** with typed `revert` as fallback. Slack-native, no leaving the inbox channel.
- **Promotion threshold: 6 confirmations-without-edits.** ≈6 weeks for weekly skills (meal-planner), ≈1 week for daily-frequency skills (slack-inbox classifications). Tunable in weekly-review prompt.
- **No new file** for the trust ledger. TOOLS.md grows; the existing approval list becomes part of the H4-forever rationale column.
- **Single-item PENDING-ASANA bundle uses emoji-confirm; multi-item retains typed directives.** Reduces friction for the most common stall case without losing expressivity.

---

## Dependencies / Assumptions

- Slack reactions on the FamilyOS bot's own past messages are readable by the slack-inbox skill via the Slack MCP. (**Assumption — verify in planning.**) If reactions are not readable, the rollback flow falls back to typed `revert <date> <class>` and the single-item Asana confirm falls back to typed `confirm 1` / `skip`.
- LOG.md tag-counting at sufficient granularity to support per-class promotion proposals. The existing weekly-review already reads LOG.md; this extension reads more carefully.
- No partner (John) involvement in v1. If the partner-handoff feature (ideation #5) ships later, the trust ledger may need a "who can curate this class" column.
- TOOLS.md remains the canonical governance file. If a future refactor splits TOOLS.md into TOOLS + TRUST, the requirements here translate cleanly to a TRUST.md.
- The triage-digest skill's "skip empty sections" rule respects SOUL.md voice — when zero entries, post nothing rather than "nothing today!" enthusiasm.

---

## Outstanding Questions

### Resolve Before Planning

(None — all product decisions resolved during this brainstorm.)

### Deferred to Planning

- [Affects R10] [Technical] [Needs research] How exactly does the slack-inbox skill detect reactions on its own past messages — is it reading the Slack MCP's `conversations.history` with reactions included, polling specific message timestamps, or using events? Confirm via slack MCP capability check before finalizing rollback flow.
- [Affects R8, R14] [Technical] What is the exact LOG.md line schema for `[AUTO-APPLIED]` so weekly-review's per-class counter can parse it reliably? Define a single canonical format in planning.
- [Affects R6] [Technical] Should the triage-digest skill be a new scheduled task, or a section appended to the existing morning brief at a different time? Default in this doc is a separate skill — confirm in planning whether that adds value or just bureaucracy.
- [Affects R14] [Technical] The "since the class was introduced" timestamp — where does it live? Initial proposal: a `since:` annotation in the TOOLS.md table row. Confirm in planning.
- [Affects R2] [Needs research] Initial seed values for the table — does every existing FamilyOS skill action map cleanly into one of the named classes, or are there micro-actions that need their own row? Audit in planning.

---

## Next Steps

→ `/ce-plan` for structured implementation planning
