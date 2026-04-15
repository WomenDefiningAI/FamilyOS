# Setup Checklist

Quick reference for the setup flow. For the guided walkthrough, use the bootstrap prompt in README.md Step 3.

---

- [ ] Create an empty folder (e.g. `~/Documents/FamilyOS`)
- [ ] Open Claude Desktop → **Cowork** → **Choose Folder** → select your empty folder
- [ ] Paste the bootstrap prompt from README.md Step 3 into a new Cowork session

Cowork handles the rest. Check these off as it walks you through them:

- [ ] Template files copied into your folder (README.md, BOOTSTRAP.md, skills/, workspace/)
- [ ] Onboarding interview complete → `workspace/USER.md` filled in
- [ ] **Customize → Connectors:** Gmail, Google Calendar, Slack enabled
- [ ] Slack inbox channel created (private, just you) → `workspace/TOOLS.md` updated

**Four scheduled tasks** — Cowork creates these for you via the `schedule` skill. Verify in **Cowork → Scheduled**:
- [ ] Morning Brief — Daily, 7 AM — prompt from `skills/morning-brief.md`
- [ ] Household Inbox — Hourly — prompt from `skills/slack-inbox.md`
- [ ] Evening Memory — Daily, 9 PM — prompt from `skills/memory-consolidation.md`
- [ ] Weekly OS Review — Weekly, Sunday 6 PM — prompt from `skills/weekly-review.md`

**Pre-approve tool permissions:**
- [ ] Click **Run now** on each of the four tasks in Cowork → Scheduled
- [ ] Approve Gmail / Calendar / Slack / file access when prompted

**Cowork project** (Projects → + New Project):
- [ ] Name: FamilyOS — paste project instructions from README.md Step 7

**Cleanup:**
- [ ] Delete `BOOTSTRAP.md` once setup is confirmed working

**Verify:**
- [ ] Morning brief appeared in Cowork history
- [ ] Test Slack drop filed to the right resource file within an hour
- [ ] MEMORY.md has first entry after evening job runs
- [ ] OS-LOG.md has first entry after first Sunday
