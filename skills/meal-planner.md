# Meal Planner — On-Demand Skill

**How to use:** Open a Cowork session and paste everything below the line. Run this whenever you want to plan the week's meals.

---

Read SOUL.md, USER.md, and MEMORY.md from the FamilyOS workspace folder.
Check `resources/meal-plans/` for the last 2–3 weeks of plans (to avoid repeating the same meals).
Check `resources/shopping/current-list.md` for items already on hand.

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
```

**After I approve:**
1. Save the plan to `resources/meal-plans/[YYYY-MM-DD].md`
2. Append the new ingredients to `resources/shopping/current-list.md`

Before saving, ask: "Does this work, or would you like to swap anything out?"
