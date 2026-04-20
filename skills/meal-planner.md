# Meal Planner — On-Demand Skill

**How to use:** Open a Cowork session and paste everything below the line. Run this whenever you want to plan the week's meals.

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
1. Save the plan to `resources/meal-plans/[YYYY-MM-DD].md`
2. Append the new ingredients to `resources/shopping/current-list.md`

Before saving, ask: "Does this work, or would you like to swap anything out?"
