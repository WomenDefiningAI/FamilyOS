# Setup Checklist

Quick reference for manual setup. For a guided walkthrough, use the bootstrap prompt in README.md Step 4.

---

- [ ] Clone the repo: `git clone https://github.com/[OWNER]/FamilyOS.git ~/Documents/FamilyOS`
- [ ] Open Claude Desktop → click **Cowork**
- [ ] Set working folder to `FamilyOS/workspace/`
- [ ] **Customize → Connectors:** enable Gmail, Google Calendar, Slack
- [ ] Create `#family-inbox` Slack channel (private, just you) → update `workspace/TOOLS.md`
- [ ] Fill in `workspace/USER.md` with your family details

**Four scheduled tasks** (Cowork → Scheduled → + New Task):
- [ ] Morning Brief — Daily, 7 AM — prompt from `skills/morning-brief.md`
- [ ] Household Inbox — Hourly — prompt from `skills/slack-inbox.md`
- [ ] Evening Memory — Daily, 9 PM — prompt from `skills/memory-consolidation.md`
- [ ] Weekly OS Review — Weekly, Sunday 6 PM — prompt from `skills/weekly-review.md`

**Cowork project** (Projects → + New Project):
- [ ] Name: FamilyOS — paste project instructions from README.md Step 8

**Cleanup:**
- [ ] Delete `workspace/BOOTSTRAP.md` once setup is confirmed working

**Verify:**
- [ ] Morning brief appeared in Cowork history
- [ ] Test Slack drop filed to the right resource file within an hour
- [ ] MEMORY.md has first entry after evening job runs
- [ ] OS-LOG.md has first entry after first Sunday
