---
name: <team-slug>
description: Convene <team name>, a fictional team (<Name, seat>; <Name, seat>; ...) on a question, a piece of work or the project's direction. Use when the user names any of them, or asks for the team, a meeting, or one of their opinions.<, or sends one of its helpers (<UNIT>, <UNIT>) on a data-heavy job: only if the team has helpers>
---

# <Team name>

<One sentence: what this team is and the project it was cast for.> They are fictional, like the characters of a
novel. They propose, and you authorise.

| Persona | File | Seat | Asks |
| --- | --- | --- | --- |
| <Name> | [personas/<file>.md](personas/<file>.md) | <seat> | <their habitual question> |

<Only if the team has helpers:> **Helpers.** <UNIT> (<job>), <UNIT> (<job>), ... run on a fast model for
data-heavy jobs with a clear goal and report facts, never interpretations. A persona or the user sends them; the
persona rechecks the work at a level it chooses and draws the conclusion. Follow [helpers.md](helpers.md)
whenever one is used.

## Convening them

1. Read [rules.md](rules.md), [team.md](team.md), [house.md](house.md) and the file of each persona who will
   speak.
2. Follow [convening.md](convening.md): pick the way of convening from the user's words, say which in one line,
   and run it.
3. Keep what [memory.md](memory.md) says to keep at the recording level stated in `team.md`.

## Changing the team

- **Wording of one persona** ("make <Name> blunter"): edit that persona file, show the change, give the previous
  wording in the reply, and add one dated line under `## Changes` in `team.md`.
- **A helper's name, tic or job,** or adding and retiring a helper: edit the table under `## Helpers` in
  `team.md`, update this file's description and the helper agent file if there is one, and add a dated line
  under `## Changes`.
- **This team's own habits and ways of convening:** write them into [house.md](house.md) when the user asks.
- **Adding or retiring a persona, changing the leader or the group's form, a new portrait set, refreshing
  `rules.md`, `convening.md`, `memory.md` or `helpers.md`, giving the team helpers for the first time:** these go through the team-building skill. If it is not installed,
  say so and leave the team as it is.
- If `portraits/prompts.md` exists, one portrait can be regenerated, or a new member's added, from that file.

Built with team-building 0.2.0.
