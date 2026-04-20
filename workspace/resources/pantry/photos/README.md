# Pantry / fridge / freezer photos

Drop photos of your on-hand inventory in this folder. The meal planner reads them directly when building the weekly plan.

## Why this folder exists

The Slack connector that FamilyOS uses for the household inbox is text-only — it can see message text but cannot download image attachments. Putting photos here instead gives every skill full access to them.

## Filename convention

Use `<location>-YYYY-MM-DD.jpg` so the planner can pick the freshest photo per location:

- `fridge-2026-04-20.jpg`
- `freezer-2026-04-20.jpg`
- `chest-freezer-2026-04-20.jpg`
- `pantry-2026-04-20.jpg`

Multiple photos of the same location on the same day are fine — number them: `fridge-2026-04-20-1.jpg`, `fridge-2026-04-20-2.jpg`.

## When photos are used

- **Weekly meal planner** — reads the most recent photo per location to cook down what's on hand first.
- **On-demand meal ask in Cowork** — you can also just attach photos directly in the Cowork session; those take precedence over this folder for that run.

## What doesn't work

- Dropping photos into the Slack household inbox. The hourly inbox job will see the message text but cannot open the image. It will log a `[NEEDS-REVIEW]` entry asking you to put the photo here instead.

## Keeping it tidy

Old photos aren't harmful — the planner prefers the most recent per location — but it's fine to delete anything older than a month.
