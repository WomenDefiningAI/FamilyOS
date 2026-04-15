# BOOTSTRAP.md — FamilyOS Setup Guide

**You'll delete this file when setup is complete.** It's a one-time guide, not part of the running system.

---

## What you need before starting

- Claude Pro or Max ($20+/month)
- Claude Desktop installed — [claude.ai/download](https://claude.ai/download)
- A Google account (for Gmail and Calendar)
- A Slack workspace — [slack.com](https://slack.com) (free tier is fine)

---

## Step 1 — Create a folder for your OS

Make an empty folder on your computer where FamilyOS will live. For example:

- Mac: `~/Documents/FamilyOS`
- Windows: `Documents\FamilyOS`

No terminal, no git, no downloads — just an empty folder. Cowork handles the rest.

---

## Step 2 — Open the folder in Cowork and paste this prompt

1. Open Claude Desktop → click **Cowork** at the top
2. Click the **folder icon** in the sidebar → **Choose Folder** → select the empty folder you just created
3. Start a new Cowork session and paste the prompt below

This single prompt pulls the FamilyOS template into your folder, runs an interview to customize it to your household, and sets up the rest (including scheduling the recurring tasks for you) step by step.

```
I want to set up FamilyOS, the household manager, in this folder.

The template is at https://github.com/WomenDefiningAI/FamilyOS

1. Copy all of the template files from that repo into this folder so we
   have README.md, BOOTSTRAP.md, SETUP-CHECKLIST.md, skills/, and
   workspace/ locally.
2. Read workspace/SOUL.md, workspace/USER.md, and workspace/TOOLS.md.

Now run the onboarding interview with me. Ask about:
- Who's in my household (adults and children — ages, schools, grades)
- Our weekly rhythm — pickups, activities, work schedules
- Any helpers (cleaners, babysitters, tutors) and how we pay them
- Meal preferences and dietary restrictions
- Home basics (owned or rented, anything notable)
- What I want in my morning brief
- What I'll name my Slack household inbox channel

Ask one topic at a time. After the interview:

1. Rewrite workspace/USER.md with everything I told you. Show me the
   result and confirm.
2. Walk me through enabling these connectors in Claude Desktop →
   Customize → Connectors: Gmail, Google Calendar, Slack.
3. Walk me through creating my Slack household inbox channel and
   updating workspace/TOOLS.md with the name.
4. Use the schedule skill to create these four scheduled tasks for me —
   don't walk me through the UI, schedule them directly, then confirm
   each one was created:
   - Morning Brief — daily at 7:00 AM — prompt: contents of skills/morning-brief.md
   - Household Inbox — hourly — prompt: contents of skills/slack-inbox.md
   - Evening Memory — daily at 9:00 PM — prompt: contents of skills/memory-consolidation.md
   - Weekly OS Review — weekly on Sunday at 6:00 PM — prompt: contents of skills/weekly-review.md
5. Tell me to go to Cowork → Scheduled, find each of the four tasks,
   and click "Run now" on each one. When prompted, I should approve the
   tools it asks for (Gmail / Calendar / Slack / file access). This
   pre-approves those tools so future automated runs don't silently
   pause waiting for permission. Takes two minutes. The Morning Brief
   in particular — running it now will also confirm the connectors are
   actually working.
6. Walk me through creating a FamilyOS project in Claude Desktop →
   Projects → + New Project with the instructions from README.md Step 7.
7. When everything is confirmed working, tell me to delete BOOTSTRAP.md
   from my folder — it's only needed for setup.
```

---

## What happens after the interview

The agent handles the rest — one step at a time, in plain language.

**Total setup time: about 20–30 minutes.**

When Cowork tells you setup is complete and to delete this file — delete it. It won't be needed again and keeping your folder clean matters.

---

## Your daily workflow after setup

**Morning** — check the brief in your Cowork scheduled task history.

**During the day** — drop notes in your Slack inbox channel. Filed within the hour.

**Evening** — memory consolidation runs automatically.

**Sunday** — check your Slack channel or `workspace/OS-LOG.md` for the weekly system review and improvement proposals.

**On demand** — open your FamilyOS project in Cowork and ask anything:
`"Any urgent school emails?"` / `"Make a meal plan"` / `"What's on the shopping list?"`
