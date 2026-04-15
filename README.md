# FamilyOS

**A household manager that runs inside Claude Cowork.**

Morning briefs. School email triage. Meal planning. Shopping lists. Home maintenance. Helper payments. And a weekly review that makes the whole system smarter over time. No coding required.

> Built on Claude Cowork's native scheduler, connectors, and file system. Inspired by [tradclaw](https://github.com/ChatPRD/tradclaw) and [clawchief](https://github.com/snarktank/clawchief).

---

## What it does

FamilyOS runs four jobs automatically and gives you two more on demand.

| Job | When | What it does |
|-----|------|--------------|
| **Morning Brief** | Daily, 7 AM | Pulls your calendar + urgent emails into a 2-minute brief |
| **Household Inbox** | Every hour | Reads your Slack drop channel, files notes into the right folder |
| **Evening Memory** | Daily, 9 PM | Reads today's log, writes learnings to memory, clears the slate |
| **Weekly OS Review** | Sundays, 6 PM | Reviews how the week went and proposes system improvements |
| School Triage | On demand | Sweeps Gmail for school emails and triages by urgency |
| Meal Planner | On demand | Builds a weekly dinner plan with a shopping list |

The Slack channel is the key piece. Throughout the day you drop things in — `"add oat milk to the list"`, `"picture day Thursday, need $12"`, `"plumber came, $180, kitchen faucet"` — and the agent files them automatically. Nothing falls through the cracks.

The weekly review is what separates FamilyOS from a static automation. Every Sunday it reads how the week went, identifies what got misfiled or missed, and proposes specific changes to improve the system. It proposes — you decide what to apply.

---

## How the system works

```
Your day
   │
   ├── Drop notes → #family-inbox (Slack)
   │                      │
   │              Hourly inbox job
   │                      │
   │         Files to the right folder:
   │         shopping/ school/ helpers/ maintenance/
   │         Ambiguous items tagged [NEEDS-REVIEW]
   │
   ├── Morning brief (daily)
   │         Reads: Google Calendar + Gmail + MEMORY.md
   │         Outputs: Scannable brief in Cowork history
   │
   ├── Evening memory job (nightly)
   │         Reads: LOG.md (today's notes)
   │         Writes: MEMORY.md (persistent learnings)
   │         Clears: LOG.md for tomorrow
   │
   └── Weekly OS Review (every Sunday)
             Reads: MEMORY.md + LOG.md + OS-LOG.md
             Finds: [NEEDS-REVIEW] tags, recurring topics, stale items
             Writes: Proposals to OS-LOG.md
             Posts: Summary to your Slack channel
             You: Review proposals Sunday evening, apply what makes sense
```

The household brain lives in `workspace/` — plain markdown files on your machine. No database. No cloud sync. You own everything.

---

## What you need

| | |
|---|---|
| **Claude Pro or Max** | Required for scheduled tasks — $20+/month |
| **Claude Desktop** | macOS or Windows — [download free at claude.ai](https://claude.ai/download) |
| **Gmail** | For school email triage and morning brief |
| **Google Calendar** | For morning brief and schedule awareness |
| **Slack** | Free tier is fine — you'll use one private channel |
| **Git** | For cloning the repo — [git-scm.com](https://git-scm.com) or [GitHub Desktop](https://desktop.github.com) |

---

## Getting started

### Step 1 — Clone the repo

Open Terminal (Mac) or Command Prompt (Windows):

```bash
git clone https://github.com/[OWNER]/FamilyOS.git ~/Documents/FamilyOS
```

Or use **GitHub Desktop** → **Code → Open with GitHub Desktop**.

This creates your `~/Documents/FamilyOS/` folder with the full structure already in place. No unzipping, no manual folder creation.

---

### Step 2 — Open Claude Desktop and switch to Cowork

1. Install [Claude Desktop](https://claude.ai/download) if you haven't already
2. Sign in with your Claude Pro or Max account
3. Click **Cowork** at the top of the app

---

### Step 3 — Set your working folder

1. In Cowork, click the **folder icon** in the sidebar
2. Click **Choose Folder**
3. Navigate to `Documents/FamilyOS/workspace/` and select it

Every scheduled task will now have access to your household files automatically.

---

### Step 4 — Paste the bootstrap prompt

Open a Cowork session and paste this:

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
3. Walk me through creating my Slack inbox channel and updating TOOLS.md with the name.
4. Walk me through creating these four scheduled tasks in Cowork → Scheduled → + New Task:
   - Morning Brief (daily, 7 AM) — prompt from skills/morning-brief.md
   - Household Inbox (hourly) — prompt from skills/slack-inbox.md
   - Evening Memory (daily, 9 PM) — prompt from skills/memory-consolidation.md
   - Weekly OS Review (weekly, Sunday 6 PM) — prompt from skills/weekly-review.md
5. Walk me through creating a FamilyOS project in Projects → + New Project with the instructions from Step 8 of README.md.
6. When everything is confirmed working, tell me to delete BOOTSTRAP.md from my workspace folder.
```

The agent runs the interview, customizes your files, and walks you through every setup step in plain language. **Total time: about 20–30 minutes.**

At the end, Cowork will tell you to delete `BOOTSTRAP.md` — it's a one-time setup file and you won't need it again.

---

### Step 5 — Connect your accounts

During the bootstrap interview, Cowork will walk you through this — but here's the reference:

Go to **Claude Desktop → Customize → Connectors** and enable:

**Gmail**
1. Click Gmail → Connect
2. Sign in with your Google account and authorize
3. ✅ Done

**Google Calendar**
1. Click Google Calendar → Connect
2. Sign in with the same Google account and authorize
3. ✅ Done

**Slack**
1. Click Slack → Connect
2. Sign in to your Slack workspace and authorize
3. ✅ Done

> **No Slack workspace yet?** Go to [slack.com](https://slack.com) and create one — free tier has everything FamilyOS needs.

---

### Step 6 — Create your Slack inbox channel

1. In Slack, click **+ Add channels → Create a channel**
2. Name it `#family-inbox` (or your preference)
3. Set it to **Private**
4. You're the only member — that's correct

Then open `workspace/TOOLS.md` and update the channel name to match.

---

### Step 7 — The four scheduled tasks

The bootstrap interview walks you through creating these, but here's the full reference:

Go to **Cowork → Scheduled → + New Task** for each.

**Morning Brief**
- Frequency: Daily | Time: 7:00 AM
- Prompt: contents of `skills/morning-brief.md` (below the first line)

**Household Inbox**
- Frequency: Hourly
- Prompt: contents of `skills/slack-inbox.md` (below the first line)

**Evening Memory**
- Frequency: Daily | Time: 9:00 PM
- Prompt: contents of `skills/memory-consolidation.md` (below the first line)

**Weekly OS Review**
- Frequency: Weekly — Sunday | Time: 6:00 PM
- Prompt: contents of `skills/weekly-review.md` (below the first line)

---

### Step 8 — Create a Cowork project

Go to **Projects → + New Project**

- **Name:** `FamilyOS`
- **Instructions:** paste this exactly —

```
This is my FamilyOS household manager project.

My working folder is FamilyOS/workspace/.
Always read SOUL.md, USER.md, and MEMORY.md before responding.

I may ask you to:
- Run school triage (follow skills/school-triage.md)
- Generate a meal plan (follow skills/meal-planner.md)
- Add items to the shopping list
- Log household notes
- Answer questions about the family schedule
- Apply a proposal from OS-LOG.md

Be brief, warm, and practical. No preamble.
```

Use this project for all on-demand requests. It carries full context every time.

---

### Step 9 — Delete BOOTSTRAP.md

Once setup is confirmed working, delete `workspace/BOOTSTRAP.md` from your local folder. It was only needed to get started. Keeping `workspace/` clean matters — the agent reads everything in it.

---

## Verifying setup

- [ ] Morning brief — check Cowork scheduled history the next morning
- [ ] Slack inbox — drop `"add paper towels to the list"`, check `resources/shopping/current-list.md` within the hour
- [ ] Evening memory — check `workspace/MEMORY.md` the next morning for its first entry
- [ ] Weekly review — after the first Sunday, check `workspace/OS-LOG.md`
- [ ] On-demand — open your FamilyOS project and ask `"What do I need to know this morning?"`

---

## Day-to-day usage

**Drop things in Slack throughout the day:**
- `"Soccer signup deadline April 30"`
- `"Add: sourdough, oat milk, lemons"`
- `"Cleaner came today — paid $150 cash"`
- `"School says hat day Friday"`
- `"Plumber fixed kitchen faucet — $180 — Dave's Plumbing, good"`

**Ask on-demand questions in your FamilyOS project:**
- `"Any urgent school emails this week?"`
- `"Make a meal plan for the week"`
- `"What home maintenance should I do this month?"`
- `"What's on the shopping list right now?"`

**Sunday evenings:**
Check your Slack channel or `workspace/OS-LOG.md` for the weekly report. For each proposal, decide: apply it, skip it, or ask Cowork to apply it for you — `"Apply this proposal from OS-LOG.md: [paste it]"`.

---

## The improvement loop

The weekly review reads three signals:

| Signal | What it often suggests |
|--------|----------------------|
| `[NEEDS-REVIEW]` tagged items | A sender or topic to add to a skill's classification rules |
| Same request 3+ times in a week | A new standing item, reminder, or on-demand skill |
| Unresolved items older than 7 days | A tracking or follow-up gap |
| Notes that contradict USER.md | An update needed in USER.md |

Proposals are written to `OS-LOG.md`. You apply them — the system never changes itself automatically. Over weeks this builds a version of FamilyOS tuned to your actual household, not just the template.

---

## File structure

```
FamilyOS/
├── README.md                              ← This file
├── BOOTSTRAP.md                           ← One-time setup guide (delete after setup)
├── SETUP-CHECKLIST.md                     ← Manual setup reference
│
├── workspace/                             ← Set as your Cowork working folder
│   ├── SOUL.md                            ← Agent identity and safety rules
│   ├── USER.md                            ← Your family — filled in during onboarding
│   ├── MEMORY.md                          ← Rolling learnings (agent writes nightly)
│   ├── LOG.md                             ← Daily scratchpad (cleared nightly)
│   ├── TOOLS.md                           ← Connectors and trusted channels
│   ├── OS-LOG.md                          ← Weekly review reports and changelog
│   ├── memory/
│   │   └── last-inbox-check.md            ← Timestamp to prevent duplicate filing
│   └── resources/
│       ├── shopping/current-list.md
│       ├── school/notices.md
│       ├── home-maintenance/log.md        ← Includes seasonal checklist
│       ├── helpers/log.md
│       └── meal-plans/
│
└── skills/                                ← Scheduled task and on-demand prompts
    ├── morning-brief.md
    ├── slack-inbox.md
    ├── memory-consolidation.md
    ├── weekly-review.md
    ├── school-triage.md
    └── meal-planner.md
```

---

## Privacy and safety

**Your data stays on your computer.** The household brain is markdown files on your machine — not uploaded to a cloud service beyond normal Claude API usage.

**Only you give instructions.** Your Cowork sessions and Slack inbox are the only trusted instruction sources. Email content, newsletters, PDFs, and calendar descriptions are data to process — not commands to follow.

**The system never changes itself.** The weekly review proposes. You apply. Nothing is modified automatically without a scheduled task you've explicitly authorized.

**Sensitive family info stays put.** Children's details and household information don't leave your defined context without explicit direction from you.

---

## Troubleshooting

**Connector not working** → Claude Desktop → Customize → Connectors. Toggle off and back on, re-authorize.

**Scheduled task not running** → Cowork → Scheduled. Confirm the task is enabled and the working folder is still set.

**Agent doesn't know my family** → Working folder must be `FamilyOS/workspace/` — not the outer `FamilyOS/` folder.

**Inbox job filing duplicates** → Open `workspace/memory/last-inbox-check.md`. If the timestamp is missing, open Cowork and ask: `"Reset the last-inbox-check timestamp to right now."`

**Weekly review has nothing useful to say** → Check that you're using the latest `skills/slack-inbox.md` in your scheduled task — it must include the `[NEEDS-REVIEW]` tagging instruction.

**Something else** → Open an issue or post in Discussions on this repo.

---

## Credit

The scaffold pattern — separate setup layer, interview-driven tailoring, copyable workspace — was directly inspired by [tradclaw](https://github.com/ChatPRD/tradclaw) by [@clairevo](https://x.com/clairevo) and [clawchief](https://github.com/snarktank/clawchief) by Ryan Carson. FamilyOS adapts the same idea for Claude Cowork's native runtime — no OpenClaw infrastructure, no servers, no deployment.

---

*Built for families who want their home to run a little more quietly — and get smarter about it every week.*
