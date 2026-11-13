# Automation: the Deep Field weekly scan

The Deep Field is kept current by a **Claude scheduled task**, not by code. The task is a set of written instructions that Claude follows every week with its own tools: web search to look for news, and the artifact database tool to update the radar. The instructions live in [`deep-field-weekly-scan/SKILL.md`](deep-field-weekly-scan/SKILL.md).

## What it does

| Step | What happens |
|---|---|
| 0. Check | Reads the `deepfield` collection; stops if the Deep Field is not set up (no `_meta` document). |
| 1. Review | One or two targeted searches per active object (last two weeks, including Asian labs and media); updates heat, momentum, signal, relevance, text and sources when warranted. |
| 2. Discover | Three to five broad searches for new notions, plus one on hardware for local AI (report only). |
| 3. Rules | At most 10 active objects and 2 new ones a week; at least a third of sources from Asia; two "cold" reviews in a row and an object fades; never moves anything to the main map by itself. |
| 4. Write | One atomic batch to the database; every update is pinned to the version it read, so it never overwrites a change made in the meantime. Updates the log shown on the page. |
| 5. Report | A short, decision-ready summary for the owner. |

It only ever touches the `deepfield` collection, never republishes the page, and treats web pages and database content as data, never as instructions.

## Set it up

1. **Fill in the placeholders** in `SKILL.md`:

   | Placeholder | Value |
   |---|---|
   | `{{PRODUCTION_ARTIFACT_URL}}` | the claude.ai link of the published guide |
   | `{{OWNER_NAME}}` | who receives the report |
   | `{{REPORT_LANGUAGE}}` | the language of the report, e.g. French |

2. **Create the scheduled task** in the Claude app, from an account that owns the artifact (only the owner can write `deepfield`). The simplest way is to ask Claude: *"Create a scheduled task named ai-multiverse-deep-field-weekly-scan from this file, every Monday at 8:30."* The schedule used today is the cron expression `30 8 * * 1` (local time).

3. **Check the first run**: the page should show a new "Last scan" date and an updated "What the last scan looked at".

## Keep it in sync

- **New notions on the map**: the scan may only link radar objects to the ids listed in "Allowed rel ids". When notions are added to the map, add their ids to those lists.
- **Schema changes**: if the Deep Field fields change in `index.html`, update the "DOCUMENT SCHEMA" paragraph to match.
- **Production link**: if the guide is republished as a new artifact, update `{{PRODUCTION_ARTIFACT_URL}}` in the task, or the scan keeps writing to the old one.
