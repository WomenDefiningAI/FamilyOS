# BOOTSTRAP.md — FamilyOS Setup Guide

**You'll delete this file when setup is complete.** It's a one-time guide, not part of the running system.

---

## What you need before starting

- Claude Pro or Max ($20+/month)
- Claude Desktop installed — [claude.ai/download](https://claude.ai/download)
- A Google account (for Gmail and Calendar)
- A Slack workspace — [slack.com](https://slack.com) (free tier is fine)

---

## Step 1 — Clone the repo to your computer

Open Terminal (Mac) or Command Prompt (Windows) and run:

```bash
git clone https://github.com/[OWNER]/FamilyOS.git ~/Documents/FamilyOS
```

Or use the GitHub Desktop app — click **Code → Open with GitHub Desktop**.

This creates `~/Documents/FamilyOS/` with the full folder structure already in place.

---

## Step 2 — Set your working folder in Cowork

1. Open Claude Desktop → click **Cowork** at the top
2. Click the **folder icon** in the sidebar
3. Navigate to `Documents/FamilyOS/workspace/` and select it

---

## Step 3 — Paste this prompt into Cowork

This single prompt starts an interview that customizes FamilyOS for your household and walks you through the rest of setup step by step.

```
Read the FamilyOS household manager at https://github.com/[OWNER]/FamilyOS

Start with BOOTSTRAP.md so you understand the setup process.
Then read SOUL.md, USER.md, and TOOLS.md from my working folder.

Now run the onboarding interview with me. Ask about:
- Who's in my household (adults and children — ages, schools, grades)
- Our weekly rhythm — pickups, activities, work schedules
- Any helpers (cleaners, babysitters, tutors) and how we pay them
- Meal preferences and dietary restrictions
- Home basics (owned or rented, anything notable)
- What I want in my morning brief
- What I'll name my Slack household inbox channel

Ask one topic at a time. After the interview:

1. Rewrite USER.md with everything I told you. Show me the result and confirm.
2. Walk me through enabling these connectors in Claude Desktop → Customize → Connectors: Gmail, Google Calendar, Slack.
3. Walk me through creating my #family-inbox Slack channel and updating TOOLS.md with the name.
4. Walk me through creating these four scheduled tasks in Cowork → Scheduled → + New Task:
   - Morning Brief (daily, 7 AM) — prompt from skills/morning-brief.md
   - Household Inbox (hourly) — prompt from skills/slack-inbox.md
   - Evening Memory (daily, 9 PM) — prompt from skills/memory-consolidation.md
   - Weekly OS Review (weekly, Sunday 6 PM) — prompt from skills/weekly-review.md
5. Walk me through creating a FamilyOS project in Claude Desktop → Projects → + New Project with the instructions from README.md Step 8.
6. When everything is confirmed working, tell me to delete BOOTSTRAP.md from my workspace folder — it's only needed for setup.
```

---

## What happens after the interview

The agent handles the rest — one step at a time, in plain language.

**Total setup time: about 20–30 minutes.**

When Cowork tells you setup is complete and to delete this file — delete it. It won't be needed again and keeping workspace/ clean matters.

---

## Your daily workflow after setup

**Morning** — check the brief in your Cowork scheduled task history.

**During the day** — drop notes in your Slack inbox channel. Filed within the hour.

**Evening** — memory consolidation runs automatically.

**Sunday** — check your Slack channel or `workspace/OS-LOG.md` for the weekly system review and improvement proposals.

**On demand** — open your FamilyOS project in Cowork and ask anything:
`"Any urgent school emails?"` / `"Make a meal plan"` / `"What's on the shopping list?"`
