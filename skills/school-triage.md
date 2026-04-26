# School Triage — On-Demand Skill

**How to use:** Open a Cowork session and paste everything below the line. Run this whenever you want a sweep of school communications.

**Action trust:** see `workspace/TOOLS.md` → Action trust levels. This skill's state-changing action is `school-triage/append-notices` (H4 baseline; eligible for H5 promotion via weekly-review).

---

Read SOUL.md, USER.md, and MEMORY.md from the FamilyOS workspace folder.

Scan Gmail for all emails from school-related senders in the last 7 days.
School senders include: the school domain, teacher names from USER.md, school office addresses, and any parent communication platforms (ClassDojo, ParentSquare, Seesaw, Remind, etc.).

For each email, classify as:
- **Action needed today** — form due, money needed, response required immediately
- **Upcoming deadline** — something due in the next 2 weeks
- **FYI only** — newsletters, general info, nothing requiring action
- **Already handled** — if MEMORY.md or LOG.md shows it was dealt with

Output:

**Triage table:**
| Subject | From | Date | Category | Action needed |
|---------|------|------|----------|---------------|

**Action checklist** (Action needed today items only, as a short checklist):
- [ ] ...

After outputting, append any new dates or deadlines to `resources/school/notices.md` (class: `school-triage/append-notices`). If this class is H5 in TOOLS.md, also emit an `[AUTO-APPLIED]` LOG entry per the schema in TOOLS.md → LOG.md tag conventions.
Ask if any of the FYI items need to be logged or acted on.

**Maple parallel forwarding:** Gmail-based triage above is the source of truth. Maple (`lovellwarner@mapleinbox.com`) runs in parallel — school senders should be auto-forwarded to that address via a Gmail filter, so Maple gets its own copy on the phone. If the user mentions an email is missing from Maple, check that the Gmail filter is still active on that sender. Do not re-triage from Maple; do not send anything to Maple from this skill.
