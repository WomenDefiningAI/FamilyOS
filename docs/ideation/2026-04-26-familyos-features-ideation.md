---
date: 2026-04-26
topic: familyos-features
focus: new skills/features extending FamilyOS as household operations layer
mode: repo-grounded
---

# Ideation: FamilyOS Plugin Features

## Build Queue (decided 2026-04-26)

**Order:** #4 → #7 → #2 → #3

| Order | Survivor | Status | Notes from review |
|-------|----------|--------|---------|
| **1st** | #4 Trust Gradient | **In progress (ce-brainstorm)** | Single triage area + auto-apply reversibles per Helpfulness principles + per-skill graduated trust by sensitivity. |
| 2nd | #7 Decision Log + Solutions Archive | Queued | Wanted but not first. |
| 3rd | #2 Relationship Schema → **Danielle's relationship management** | Queued (reframed) | Reframe: less about Soma's friends, more about Danielle staying in touch with friends/colleagues, birthday cards, family gifts (Mother's Day, Father's Day for John + Danielle's father, thank-you cards, annual Christmas chatty-newsletter card). Leverage Clay/Mesh MCP + Postable (Minted lacks text breadth). Tie to seasonal planning rhythm (Dec → Soma Feb 11 birthday party, Oct → Christmas planning). Potentially the highest-value skill. |
| 4th | #3 Health-as-First-Class | Queued (broadened) | Integrate Danielle's existing Claude Projects health context (Danielle = most complex; lipids, muscle building). Accommodate new projects (plastic-associated chemical reduction, John's RCPD treatment consideration). Soma's asthma + appointment triage stays in scope. |

**Deferred / shelved:**
- #1 Financial Layer — defer the alarms-shape; real near-term ask is monthly-spend visibility from Monarch + budget recommendation as travel year tapers.
- #5 Schedule Choreography — keep Monday-pickup-protocol + ad-hoc dropoff switching (Danielle's sales calls); drop the full partner-handoff brief (neither parent travels reliably).
- #6 Permission Slip Bridge — shelved; low volume currently.

## Grounding Context

### Codebase Context

**Plugin today (7 skills):** meal-planner, housekeeper-weekly, school-triage, morning-brief, memory-consolidation, weekly-review, slack-inbox.

**Architecture:** Slack `#familyos` is the single inbox/router. State flows through `LOG.md` (daily scratch, cleared nightly) → `MEMORY.md` (persistent + "Waiting on human" section, capped at 150 lines) → `OS-LOG.md` (proposals + applied changes). Skills follow a propose → confirm → apply cycle, using tags `[PENDING-ASANA]`, `[NEEDS-APPROVAL]`, `[NEEDS-REVIEW]` that the evening-memory job rescues into MEMORY.md so they survive the nightly LOG clear.

**Family:** Danielle (mom, WFH, primary user), John (dad, WFH, graphic arts/games), Soma (7.5, Grade 1 at Claren Academy, asthmatic + dust-mite allergic, after-school club Mon–Wed, karate Thursdays). Helper: Cassidy (housekeeper, Mondays 10:30am–1:30pm, monthly e-transfer against invoice).

**Tools wired:** Asana ("Lovell Warner Personal" project), Maple (via Chrome MCP — no API), Monarch (read-only references), Gmail, Google Calendar (multi-cal), Google Drive (Cassidy's task doc, meal plans).

**SOUL.md voice:** *not a scold or nag, not a lifestyle optimization system, not performatively enthusiastic.* Terse. Dates spelled out fully. Skip empty sections. Approval-required actions: send email/msg, add/remove cal events, create Asana, push to Maple, delete files, share family info externally.

**Connector limits:** Slack can't read photos, no DMs, can't parse Gmail attachments; multi-cal spotty; scheduled tasks fire only when desktop is unlocked; no Maple/Shop.app APIs; FamilyOS must not live in iCloud-synced folders.

### Pain Points and Documented Gaps

* **Slack-inbox → Asana confirmation stalls** — flagged in two consecutive review cycles (Week of 2026-04-13, Week of 2026-04-20). The only pattern surfaced twice.

* **Finance / reimbursements** — MEMORY.md 2026-04-19 captures 5+ stalled OTMG/Monarch items (John's ChatGPT, life insurance, YouTube Premium, Family Savings cleanup); brief 2026-04-25 surfaced an *overdue* tax-balance Asana task.

* **Helper payment loop** — `resources/helpers/log.md` Payment-log table is structurally empty despite Cassidy working every Monday.

* **Recurring/standing dates** — OS-LOG Proposal 5 had to manually create 5 annual tasks that were "tracked only in prose."

* **Schedule deviations** — MEMORY.md 2026-04-22 codifies the Monday-pickup protocol (message Anne Luyten + email Wize); 2026-04-24 confirms it ran manually again.

* **Health-appointment triage** — Soma asthma + Versa Health Apr 29 + dentist May 27 + Danielle bloodwork Apr 24, scattered across briefs without dedicated triage.

* **Permission-slip deadlines** — Gmail attachments unreadable, `school/notices.md` empty.

* **Maple feedback loop** — meal-planner pushes one-way, never reads back what's been bought.

### External Patterns Cataloged

* Obie Fernandez's Personal CTO OS — slash-command rituals (`/morning`, `/decide`, `/prep`), versioned markdown across `team/`, `decisions/`, `priorities/`. 11.5k lines compounded in 3 weeks.

* LangChain EAIA — three triage buckets (`triage_no`, `triage_notify`, `triage_email`); draft-only pattern as deliberate design choice.

* Reclaim.ai — P1–P4 priority weighting with defense levels; auto-reschedule for displaced habits.

* Ohai.ai — scan-to-calendar, Circles for multi-member households with role tiers, human-fallback pattern.

* Claire Vo's "9 AI employees" — calendar-wipe incident → progressive-trust pattern as canonical recovery.

* Tody — decay-curve task scheduling (time-since-last-done × per-item urgency).

* Clay — relationship-decay keep-in-touch cadences.

* Cozi — pre-built seasonal home maintenance checklists.

* Family Office CoS — three-beat weekly rhythm (Mon plan, Wed escalate, Fri pre-load).

* Hotel concierge guest profile, ICU SBAR brief format, ATC jurisdiction routing — adjacent structural moves.

## Ranked Ideas

### 1. Financial Layer — money-watch + helper-payment loop

I don't want to start on the financial layer just yet. Soon, I do need help working through Monarch and setting a reasonable family budget (because we've been doing some high spending on travel, which won't continue past this year. What I need is a sane way to know what our monthly spending is

**Description:** A daily watcher across Monarch, Gmail receipts, and Asana for money-out events (overdue bills, recurring charges needing OTMG re-routing, reimbursement-eligible items), bundled with the Cassidy invoice → e-transfer cycle that closes the structurally-empty `resources/helpers/log.md` Payment-log table.

**Warrant:** `direct:` Brief 2026-04-25: "(overdue) Pay my tax balance — Asana task, was due Friday April 24." MEMORY.md 2026-04-19 captures 5+ stalled OTMG/Monarch items: John's ChatGPT, YouTube Premium credit-card switch, life insurance premium, Disneyland reimbursement, Family Savings cleanup. USER.md line 87: "Elevate immediately anything touching… money moving out of a household account, regardless of horizon." `resources/helpers/log.md` Payment-log table has only headers — no rows — despite weekly Cassidy visits.

**Rationale:** A tax balance went overdue *in plain sight of the morning brief.* Half the documented `[PENDING-ASANA]` backlog is finance. No skill currently owns money — and the Cassidy monthly-invoice-then-pay cycle is pure-human ritual despite USER.md spelling out the exact rule.

**Downsides:** Monarch has no MCP — reads are scrape-shaped. E-transfer can't actually be pushed (no API), so the pipeline ends at "draft + log + bank URL," human still clicks. Risk of false alarms on legitimate but unfamiliar transactions.

**Confidence:** 90% · **Complexity:** Medium · **Status:** Unexplored

***

### 2. Relationship Schema — `resources/people/` + `resources/vendors/` + decay primitive

We could add the Clay MCP and add a skill for Danielle's relationship management. It's less important to organize Soma's friends this way, but Danielle does really want to stay in touch with friends and colleagues, send birthday cards and family gifts (Mother's day coming up, Father's day for John and Danielle's father, thank you cards for gift givers, big task of sending Christmas cards each year). This is potentially the top skill here, and it should leverage Danielle's existing tools (i.e. the Clay/Mesh MCP and using Postable to send cards, plus making recommendations on how to send a large, chatty newsletter with a family photo card each year. Some families use Minted, but this does not appear to have the breadth of text that Danielle wants to provide in an update. This is also related to family planning that goes by seasons (ex. December kicks off Soma birthday party planning for February 11th, October kicks off christmas planning, etc)

**Description:** A one-time schema investment: per-person markdown for Soma, John, Cassidy, Soma's friends (Rohan, Stirling, Meghan), and providers (Anne Luyten, Wize, Versa Health, Ninja X Academy, pediatrician, dentist), plus a parallel vendor directory with billing/account/satisfaction fields, plus a reusable `last_touch:` / `decay_days:` tag pattern. Every future skill reads from one source instead of re-deriving context from chat.

**Warrant:** `external:` Obie's Personal CTO OS uses `team/<person>.md` and accumulated 11,579 lines of versioned context in 3 weeks; hotel concierge pre-arrival guest profile; Clay's relationship-decay model; Tody's per-item decay curve. `direct:` MEMORY.md "Soma's social network" already collects friend names but has no schema; ce-learnings-researcher explicitly flagged the missing vendor directory; recurring vendor mentions (Wize, Versa, Ninja X) re-derive context every time.

**Rationale:** Single biggest leverage move. Subsumes the recurring-roster problem, social-cadence tracking, vendor-rescheduling, birthday-prep — they all become 50-line skills instead of 500. The decay primitive powers chore urgency, friend-follow-up nudges, vendor-relationship maintenance, and decision-revisit triggers from one mechanism.

**Downsides:** Schema design is hard to undo; risk of over-engineering. Backfill from MEMORY.md takes a focused pass. Some entities (Soma's friends, providers) have privacy weight that dictates how visible the files become.

**Confidence:** 85% · **Complexity:** Medium · **Status:** Explored (handed off to /ce-brainstorm 2026-04-26)

***

### 3. Health-as-First-Class — Soma health profile + appointment triage + consumables par-levels

I do have Claude Projects holding information about all of our health backgrounds, with mine as the most complex. I would find it helpful to have some kind of tie in with reminders of what I'm managing (lipids, muscle building) and what types of health related projects I can add (plastic associate chemical reduction, john considering treatment for his RCPD)

**Description:** Three pieces working together: a canonical `soma-health.md` (asthma triggers, allergens, providers, current meds + dosing, growth curve, immunizations, sick-day patterns), a triage skill that owns Versa/dentist/bloodwork/refill threading across Gmail+calendar, and restaurant-style par-levels for inhalers and allergy meds with reorder triggers fired by usage signals.

**Warrant:** `direct:` USER.md: "Allergic to dust mites; has asthma." USER.md elevation rule: "Elevate immediately anything touching… \[Soma's health], regardless of horizon." Brief 2026-04-25 caught the Apr 29 Versa appointment vs. Craft/Story Club conflict only because morning-brief happened to notice; brief 2026-04-22: "Friday April 24: Danielle LifeLabs bloodwork 7:45 AM… fasting." MEMORY.md 2026-04-21: Soma dentist May 27. `external:` restaurant mise-en-place par-levels + supply-chain reorder-point math.

**Rationale:** Health is the single category USER.md elevates regardless of horizon, yet today it's scattered across briefs without a triage layer or a consumable-stockout safeguard. The Apr 29 Versa appointment already created a confirmed school-pickup conflict caught by chance. Inhaler running out mid-attack is the worst-case failure mode.

**Downsides:** Health data is sensitive — local-first matters here; needs care around what surfaces vs. stays in-file. Par-levels need usage signals the system doesn't capture today (would require a quick-log mechanism).

**Confidence:** 90% · **Complexity:** Medium-High · **Status:** Unexplored

***

### 4. Trust Gradient — triage\_no + auto-apply-reversibles + per-skill earned trust ✅ IN PROGRESS

I would appreciate this as well. Have a single triage area, auto-apply reversibles that apply Helpfulness principles, and yes per-skill graduated trust based on the sensitivity of area.

**Description:** Coherent system, not three patches. EAIA-style `triage_no` bucket where confident-no-action items (auto-receipts already in Monarch, accepted-cal-invites, duplicate notifications, newsletters) never enter the proposal queue. Auto-apply on a hold-window timer for trivially reversible proposals (text edits, Asana retags, low-cost Maple staples). Per-skill graduated trust where meal-planner can earn auto-apply for grocery list edits after 30 successful confirmations, while school-triage stays strict.

**Warrant:** `direct:` OS-LOG flagged the slack-inbox-to-Asana confirmation stall in Week of 2026-04-13 *and* Week of 2026-04-20 — the only pattern surfaced in two consecutive review cycles. `external:` LangChain EAIA's three-bucket triage (`triage_no` is the missing primitive); Reclaim's defense-level graduation; Claire Vo's progressive-trust pattern (recovered from a calendar-wipe incident).

**Rationale:** Uniform approval is uniform friction. After the 50th meal plan confirmation, the gate is theater; after the 1st school-permission auto-send, the cost is real. Trust gradient matches friction to actual blast radius — the only sustainable path past the "I should just do it myself" cliff.

**Downsides:** Reversibility taxonomy needs careful design; "apply unless stopped" reads nag-adjacent if SOUL.md tone slips. Track-record persistence needs schema work. The graduation criteria are the hardest part.

**Confidence:** 85% · **Complexity:** Medium · **Status:** Unexplored

***

### 5. Schedule Choreography Layer — night-before call-sheet + monday-pickup-protocol + partner-handoff

**Description:** Three transition surfaces. A 9pm one-page call sheet for tomorrow (each person's first commit, gear required, weather + asthma index, special notes — preparatory, not reactive). A deviation watcher that detects Soma calendar anomalies and pre-drafts the documented Anne Luyten + Wize Academy messages. A partner-handoff brief when one parent is OOO (current state, in-flight tasks, pre-authorized vs. needs-check-in, anomalies to monitor).

I think this is an interesting skill to explore, but at the moment neither parent travels. But this is helpful when there's change from standard and then things have to change (like the example mentioned here.). There's no Partner OOO reliably, but sometimes I do have sales calls that make for a partner school drop off switch

**Warrant:** `direct:` MEMORY.md 2026-04-22 codifies the Monday-pickup protocol verbatim: "when grandparents pick Soma up from school on a Monday… Danielle must (1) message Anne Luyten to confirm, AND (2) email Wize Academy to let them know Soma won't be there." 2026-04-24 confirms it ran manually again. `external:` cinematographer call-sheet (a one-page night-before artifact is structurally different from a morning brief — enables prep, not just awareness); submarine watch-handoff for partner-OOO.

**Rationale:** Mornings fall apart on things that could've been pre-staged the night before. Pickup deviations are documented but currently entirely human-driven. Partner-OOO is high-failure-mode territory currently done as a verbal scramble at the door.

**Downsides:** Adds a 9pm ritual surface — risks SOUL.md "not a scold/nag" if pushed too hard. Partner-handoff requires John's engagement to be useful (he's not currently a direct user).

**Confidence:** 80% · **Complexity:** Medium · **Status:** Unexplored

***

### 6. Permission Slip & Attachment Bridge

**Description:** Narrowly scoped. When school-triage encounters a Gmail PDF attachment (filename heuristic + sender domain + first-page text), download via Chrome MCP into a workspace tempdir, run the pdf skill, extract deadline / signature requirement / payment, append to `school/notices.md`, create `[PENDING-ASANA]` tagged with the source thread. Sets a reusable pattern for bridging other named connector gaps (Shop.app, Maple) via pdf/Chrome MCP.

We don't have any high volume of permission slips right now, so we can shelve this.

**Warrant:** `direct:` README explicitly names "can't parse Gmail attachments" as a hard connector limit; `resources/school/notices.md` is empty despite school-triage running for weeks; USER.md elevation rule covers anything touching Claren. `reasoned:` permission-slip deadlines are the canonical hidden-deadline failure mode — they hide inside PDFs while school-triage processes only the email body.

**Rationale:** Three-way alignment: named connector gap + structurally empty resource file + highest-elevation category. The unguarded path by which a Claren deadline silently misses today.

**Downsides:** OCR on heterogeneous school PDFs is messy; needs a confidence threshold and a fail-loud fallback when sub-threshold. Adds Chrome MCP dependency to school-triage.

**Confidence:** 90% · **Complexity:** Low-Medium · **Status:** Unexplored

***

### 7. Decision Log + Solutions Archive

**Description:** Two append-only knowledge layers. `resources/decisions/`: date, decision, options-considered, owner, expiry/revisit-trigger — prevents household debates from re-running every 8 weeks (Ninja X re-enrollment, after-school options, vendor switches). `docs/solutions/`: skill-design rationale, what failed, reusable primitives — compounds plugin development itself.

I appreciate this idea and think it's be good to put in queue, but not first.

**Warrant:** `external:` Obie Fernandez's `/decide` log captures every non-trivial decision with alternatives considered; software-engineering ADR (Architectural Decision Record) tradition. `direct:` ce-learnings-researcher explicitly flagged the missing `docs/solutions/` archive; MEMORY.md shows recurring program-enrollment debates with no captured rationale, so each cycle re-litigates from scratch.

**Rationale:** Meta-leverage. Decision log = institutional memory beyond MEMORY.md (which is operational, not deliberative). Solutions archive = leverage on building leverage; future skill 8, 9, 10 can grep prior decisions ("how did we solve photo input?") instead of rediscovering.

**Downsides:** Risk of becoming yet-another-file-no-one-reads if not wired into weekly-review and ce-brainstorm/ce-plan. Decision-log structure must be terse enough to actually get filled.

**Confidence:** 75% · **Complexity:** Low · **Status:** Unexplored

***

## Cross-Cutting Pattern

* **Survivors 1 + 2 + 3** form a **schema-first phase** — invest in financial / people / health structure before adding more reactive skills.

* **Survivors 4 + 5** form a **temporal-control phase** — when actions happen, who confirms, how transitions land.

* **Survivors 6 + 7** form a **bridging + compounding phase** — fix the named connector gap and capture institutional memory.

## Rejection Summary

| #  | Idea                                            | Reason Rejected                                                                          |
| :- | :---------------------------------------------- | :--------------------------------------------------------------------------------------- |
| 1  | recurring-roster (annual prose-only dates)      | Subsumed by Survivor 2 (people/vendor schema) + Survivor 7 (decision log)                |
| 2  | sunday-orchestrator (collapse 3 Sunday skills)  | Real friction but a refactor of existing skills, not a new feature                       |
| 3  | brief-as-diff (morning brief delta mode)        | Implementation detail of morning-brief, below meeting-test floor                         |
| 4  | pantry-photo-auto-ingest                        | Tactical fix to a single connector workaround; below floor                               |
| 5  | maple-readback as standalone                    | Implementation detail of meal-planner, not a new skill                                   |
| 6  | conversational-morning / continuous-brief       | No concrete warrant the current brief is failing differently than diff-mode would solve  |
| 7  | skills-from-utterance (runtime skill authoring) | Too speculative given Claude Code's skill-loading model                                  |
| 8  | plugin-writes-out (calendar/Asana write-back)   | Requires partner buy-in; high coordination cost without clear upside                     |
| 9  | soma-OS (kid-facing brief)                      | 7.5yo is on the edge; high failure cost for uncertain reading-level fit; revisit at 9–10 |
| 10 | cassidy-bot (housekeeper Slack-bot)             | Requires Cassidy adopting a new tool; current Google Doc + Slack drop pattern is working |
| 11 | read-only-mode / observe                        | Mode flag, below floor                                                                   |
| 12 | forgetful-memory (aggressive aging)             | Contradicts the working 150-line cap pattern; SOUL.md doesn't signal need                |
| 13 | premium-mode ($1000/mo always-on)               | Hypothetical without warrant                                                             |
| 14 | per-member-memory standalone                    | Merged into Survivor 2                                                                   |
| 15 | john-domain-approval (co-principal model)       | Requires John as user; merge potential into trust-gradient if he opts in                 |
| 16 | household-okrs / standing-priorities            | SOUL.md "not a lifestyle-optimization system" makes forced ritual risky                  |
| 17 | sqlite-event-log underneath markdown            | High implementation cost; markdown-only pattern is working; defer                        |
| 18 | signal-density-review trigger                   | Refinement to weekly-review, not a new skill                                             |
| 19 | pit-wall 30-sec cue                             | Overlaps brief-as-diff; JIT timing hard to get right                                     |
| 20 | household-incident / snow-day-irops             | Speculative — no logged crises pattern yet to ground playbook against                    |
| 21 | voice-note-inbox (iPhone shortcut dropzone)     | Strongest borderline-cut; surface in ce-brainstorm if pursuing capture-side ideas        |
| 22 | auto-apply-low-stakes standalone                | Merged into Survivor 4                                                                   |
| 23 | triage-no standalone                            | Merged into Survivor 4                                                                   |
| 24 | per-skill-trust standalone                      | Merged into Survivor 4                                                                   |
| 25 | stage-manager-cue-sheet standalone              | Merged into Survivor 5                                                                   |
| 26 | partner-handoff standalone                      | Merged into Survivor 5                                                                   |
| 27 | family-cab decision log standalone              | Merged into Survivor 7                                                                   |
| 28 | par-levels for consumables standalone           | Merged into Survivor 3                                                                   |
