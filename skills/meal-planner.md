# Meal Planner — On-Demand Skill

**How to use:** Open a Cowork session and paste everything below the line. Run this whenever you want to plan the week's meals.

**Action trust:** see `workspace/TOOLS.md` → Action trust levels for which actions auto-apply (H5) vs. propose (H4) vs. require explicit approval (H4-forever). This skill's state-changing actions are `meal-planner/save-week-plan` and `meal-planner/append-shopping-ingredients` (both H4 baseline; eligible for H5 promotion via weekly-review). Maple pushes are `external/maple-push` (H4-forever — always confirm).

---

Read SOUL.md, USER.md, MEMORY.md, and TOOLS.md from the FamilyOS workspace folder.
Check `resources/meal-plans/` for the last 2–3 weeks of plans (to avoid repeating the same meals).
Check `resources/shopping/current-list.md` for items already on hand.

**Check on-hand inventory from photos.** The Slack connector cannot read images, so pantry/fridge/freezer photos live locally:

1. List `resources/pantry/photos/`. For each image file, read it and extract what's visible. Prefer the most recent photo per location (fridge / freezer / chest-freezer / pantry) based on filename date or file mtime.
2. Also consider any photos attached directly to this Cowork session — they take precedence over folder photos for the same location.
3. If any photo is older than 14 days, note it in the output ("fridge photo is 18 days old — inventory may be stale") so the user knows to refresh it.
4. If no photos are available at all, say so and plan from pantry assumptions + shopping list only.

Build the plan to use what's on hand first — especially items nearing expiration or flagged by the user (e.g., "beef cubes I might toss", "scallops defrosting"). Favor those ingredients early in the week.

Generate a meal plan for the coming week (Monday through Sunday).

**Apply these constraints from USER.md:**
- Dietary restrictions and preferences
- Weeknight cooking time budget
- Cuisines the family enjoys
- Check Google Calendar for days with late pickups or activities that make cooking harder — suggest simpler meals or leftovers on those days

**Output format:**

```
## Week of [DATE]

| Day | Dinner | Notes |
|-----|--------|-------|
| Monday | | |
| Tuesday | | |
| Wednesday | | |
| Thursday | | |
| Friday | | |
| Saturday | | |
| Sunday | | |

## Ingredients to add to shopping list
- [Items needed beyond what's likely already on hand]

## Using up what's on hand
- [Items from the pantry/fridge/freezer photos being cooked this week, with the day]
```

**After I approve:**
1. Save the plan to `resources/meal-plans/[YYYY-MM-DD].md` (class: `meal-planner/save-week-plan`)
2. Append the new ingredients to `resources/shopping/current-list.md` (class: `meal-planner/append-shopping-ingredients`)
3. Mirror the newly added ingredients into Maple via Chrome MCP (class: `external/maple-push` — H4-forever, always confirm). `current-list.md` stays the source of truth — Maple is for shopping on the phone.

If steps 1 and 2 are H5 in TOOLS.md (after a weekly-review promotion), perform them and emit `[AUTO-APPLIED]` LOG entries per the schema in TOOLS.md → LOG.md tag conventions. While they remain H4 baseline, perform them as today (no `[AUTO-APPLIED]` tag — they're already inside an explicit "After I approve" gate).

   **How to push to Maple:**
   - Open `https://app.growmaple.com/` (the assistant chat works from the home page; the meal-plan page at `/mealplan` also has a chat).
   - Use the "Ask Maple…" field at the bottom-right. The natural-language pattern is: `add THING to STORE grocery list` (one item per message). Maple maintains separate lists per store — common stores in this household: **Save-On**, **Costco** (confirm with user when unsure).
   - Group items by store first, then send one chat message per item. After each, wait for Maple's confirmation before sending the next.
   - Before pushing, show the user the grouped item-by-store plan and the exact chat phrasing for each, and ask for approval. Don't push without confirmation — pushing to Maple is on the approval-required list in TOOLS.md.

Before saving, ask: "Does this work, or would you like to swap anything out?"
