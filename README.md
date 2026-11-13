# AI Multiverse

From your first prompt to custom GPU kernels. Pick a star, read its card, and log what you have actually built with it.

AI Multiverse is an interactive star map of AI notions, built for an IT team learning to use AI at work. Each star is a notion with a plain definition, a concrete example from IT operations, why it matters (with its honest limits) and sources. Each person tracks their own progress, keeps notes, and sees who on the team can help.

## What's inside

- **The star map.** Rings show depth, from Beginner (inner) to Advanced (outer). Sectors group notions by category. Dashed lines link related notions; black holes flag notions that can swallow a lot of effort for little return.
- **Universes behind Stargates.** Dedicated maps for Claude, Microsoft Copilot and Local AI, reached through portals on the main map. Search covers every universe.
- **Notion cards.** Definition, example, why it matters, sources, related notions, and "moons" for products, model families or versions.
- **Progress.** Each person marks notions as Uncharted, Exploring, Practicing or Mastered. The progress box counts the whole multiverse, with an index for each universe.
- **Flight Plans.** Four guided paths with a mission per step: First Contact (everyone), Autopilot (Copilot agents without code), Steward (governance at team scale) and Builder (developers). The route is drawn on the map, across universes, and a briefing card shows where to start.
- **Field Log.** Dated notes per notion, private or shared with the team.
- **Crew, Missions and Library.** Opt-in levels so colleagues know who to ask, real tools the team has built, and deliverables to reuse.
- **Export My Journey.** A Markdown copy of your own progress and notes.

## How it runs

The whole app is one self-contained file, `index.html`, plus the pictures in `img/`.

It is designed to run as a **Claude artifact**, which gives it a database with access rules, the viewer's identity and file downloads. Opened anywhere else (locally, or on GitHub Pages), the page runs in read-only **offline preview**: the map, cards, universes, search and flight plans work, but progress, notes, Missions, Library and Crew need the Claude runtime.

The data (progress, notes, missions…) lives in the artifact's database, never in this repository.

## Repository layout

```
index.html   the app (HTML, CSS and JavaScript in one file)
img/         original artwork: hero banner, starfield, universe skies, Stargate portals, Mastery core, black hole…
README.md
```

## Data and roles

| Collection | Holds | Who writes |
|---|---|---|
| `journey` | the author's statuses and shared journal | the page owner only |
| `data/users/<id>` | each person's private statuses, notes and flight plan progress | that person only |
| `fieldlog/<id>` | notes a person shares with the team | that person (the owner can moderate) |
| `missions/<id>`, `crew/<id>`, `library/<id>` | missions, shared levels, shared deliverables | that person (the owner can moderate) |

Runtime capabilities declared when publishing the artifact (always pass the full set):

```json
{"db":{"rules":[
  {"path":"journey","read":"view","write":"owner"},
  {"path":"missions","read":"view","write":"owner"},{"path":"missions/{self}","write":"interact"},
  {"path":"crew","read":"view","write":"owner"},{"path":"crew/{self}","write":"interact"},
  {"path":"deepfield","read":"view","write":"owner"},
  {"path":"library","read":"view","write":"owner"},{"path":"library/{self}","write":"interact"},
  {"path":"fieldlog","read":"view","write":"owner"},{"path":"fieldlog/{self}","write":"interact"}]},
 "user":{"scopes":["profile"]},"downloads":true}
```

## Working on it

- **Staging first.** Every change is published to a staging artifact and reviewed before it reaches the team's version.
- **Never rename or delete a notion id.** Statuses, notes and flight plan steps are keyed by id; moving a notion to another universe keeps its id.
- **Facts need sources.** Definitions use dated facts only when a source supports them; each card lists 1 to 3 sources.
- **Check before publishing.** JavaScript syntax, unknown related ids, missing sources and label lengths are checked by a small script before each release.

## Credits

AI Multiverse · original idea, curation and design by Gaëtan Holfeltz.
