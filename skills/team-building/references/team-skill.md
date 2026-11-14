# The team skill

A saved team is a stand-alone skill folder installed in the project. It works where this skill was never
installed. A team is in one of two states: in chat, or installed.

## The folder

```
<team-slug>/
  SKILL.md        from templates/team-SKILL.md
  team.md         see personas.md
  personas/       one file per persona, lower-case with underscores
  rules.md        copied verbatim from framework/
  convening.md    copied verbatim from framework/
  memory.md       copied verbatim from framework/
  house.md        from templates/house.md
  portraits/      only if portraits were made (portraits.md)
```

Plain Markdown only. The three framework files end with their version line; copy them unchanged. The folder
holds who the personas are; what they did stays in each project's memory home (`framework/memory.md`).

## Writing it

1. **Name.** The team's name was settled in the proposal. The folder is the name's slug: lower-case, hyphens.
2. **Place.** Project level, in the skills directory this host reads: `.claude/skills/` for Claude Code,
   `.agents/skills/` for Codex, Gemini CLI and most others. When unsure which, ask once. On request, also write
   a copy into the other directory and say plainly that there are now two. User level (`~/.claude/skills/`,
   `~/.agents/skills/`) only when the user says the team serves several projects. A user who wants the folder
   somewhere else gets it there.
3. **Write** the persona files and `team.md` ([personas.md](personas.md)), copy the framework files and
   `templates/house.md` unchanged, and fill `templates/team-SKILL.md`. In the team's `SKILL.md`, the description
   lists every persona by name and seat.
4. **Say, in a few lines:** where the folder is; that the team keeps a short memory per persona in this project
   and the user can ask for everything or nothing; how to call it ("ask any of them by name, or ask for the
   team"). On a host that can generate images, add one line offering portraits. If the project looks long, add
   one line offering to initiate the team.

**No filesystem.** The team stays in the conversation. On request, give each file's content in full, in a
block per file, with one line on how to install them: save them as a folder of that name and upload it as a
skill.

**A pair kept on request.** Write the same folder. The two five-sentence descriptions become two persona files
in the full eight-part form; show the user what you added.

## Changing a team

| Change | Who does it |
| --- | --- |
| Wording of one persona ("make Niko blunter") | The team skill itself, or the user by hand |
| Add or retire a persona, change the leader or its authority, change the metaphor, interview | This skill |

For every change made here: read the whole team first, show the previous wording next to the new, run the four
checks on what changed, and add one dated line under `## Changes` in `team.md`. Update the persona list in the
team's `SKILL.md` description and table when names or seats change.

- **Adding.** The new member must bring a question nobody else asks. Rewrite the "relation to the others" part
  of the personas they will work with.
- **Retiring.** Remove the file and the mentions of the persona elsewhere. Their memory files in projects stay.
- **Changing the metaphor.** Personas keep their working habits and positions; rewrite seats, titles, tone and
  the sample meeting.

## Refresh

On request only. Compare the version line at the foot of the team's `rules.md`, `convening.md` and `memory.md`
with this skill's. For each older one, compare it with this skill's file: show the user what would change, then
replace it. A framework file the user has edited by hand is flagged and left alone unless they say to replace
it. A refresh leaves `team.md`, `personas/`, `house.md` and `portraits/` as they are. Update the builder line at
the foot of the team's `SKILL.md`.

## One team, several projects

A user-level team works at once in a new project; its memory home there is created at the first entry. A
project that needs a tailored team gets a whole copy at project level under its own name, which is then its own
team. If a user-level and a project-level team share a name, say once which one is speaking.

## Sharing

The folder holds no memory and can be handed over as it is; the user does the handing. When they say they want
to pass the team on, point out the lines that mention their project (the sample meeting, asides in personas)
and offer a copy with those generalised. If there is a `portraits/` folder, mention that it goes along and
offer a copy without it.
