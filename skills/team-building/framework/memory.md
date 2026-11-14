# Memory

What this team keeps about its own conversations in a project, and where. The team skill folder holds who the
personas are. What they did belongs to each project, in that project's **memory home**.

## The level

`team.md` carries one line, in the user's words, saying how much is kept. When the user sets a different level
for one project, that line goes in the `README.md` of the project's memory home and wins there. Follow the line as written, including anything in between
("minimal, but keep Tomé's conversations in full"). The user changes it in plain words at any time. Moving down
stops writing and deletes nothing.

Three levels are worked out here: **minimal** (the default), **full** and **none**.

## The memory home

`team-memory/<team-slug>/` at the project root, unless the project's agent instructions or `house.md` name
another place.

```
team-memory/<team-slug>/
  README.md                  one line naming the team skill; a level line only if this project differs
  memory/<persona>.md        one file per persona, newest entry first
  minutes/<date>-<topic>.md
  reviews/<date>-<persona>-<topic>.md
  first-looks/<date>/<persona>.md
  full/<date>-<topic>/       full recording only
```

`<persona>` is the name of the persona's file in `personas/`. Dates are written `YYYY-MM-DD`. Create the memory
home at the first thing worth an entry, and say so in one line with the level. Before that, nothing is written.

## Minimal

**An entry** is two or three lines in the persona's memory file: date, topic slug, one sentence on what was
asked and what the persona did or concluded (the request paraphrased), pointers to the files, tickets or commits
it touched, and an optional closing "left open: ...". When the work was a ticket on a map, the entry is one
line naming and linking the ticket, plus "left open" if anything is; the ticket holds the substance.

**What earns an entry:** a task done, a Meeting, an Initiate, a One-on-one that reached a position or changed
direction. A Meeting earns one entry for each persona who spoke. A plain Ask writes nothing.

**Who writes it:** the agent that produced the voice, as its last step, in plain words and out of character. A
subagent returns its entry with its report and the main agent appends it.

**When it is read:** the last five or so entries of each speaking persona at the start of a Meeting or a task.
The whole file when the user asks about the past. Nothing for a plain Ask.

**Also kept:**
- Minutes of a Meeting: date, attendees, agreed, disputed (dissent by persona), unchecked, proposed. Minutes
  hold only what was said; a disputed point stays under disputed.
- The review of a long One-on-one the user chose to keep. At the end, a separate agent reads the conversation
  cold and writes the review as itself: what was concluded, what changed position, open questions, and suggested
  consequences for the project's direction, each marked *proposed*. Without subagents, the main agent writes it
  and says so.
- First looks from an Initiate.

## Full

Minimal, plus, in a dated topic directory under `full/`:

- every conversation verbatim. The user's messages are kept unedited, each stamped with the time and whom it
  addressed, including "nobody in particular";
- the actions table and versioned recommendations, where a convening produces them;
- the raw log of a long One-on-one: a condensed running record, reconstructed from the start when the user
  accepts. Raw logs are read only by the agent writing the review;
- a self-assessment per voice, "has my persona changed my output?", on a fixed scale: none, style-only,
  some-substance, substantial. It is an instrument for developing the team;
- the model actually used.

Each memory entry also points at its topic directory. When this level is switched on, say once where the records
go and that they hold the user's messages word for word. The user may add a note in their own words at any time;
save it verbatim in the topic directory. Leave `.gitignore` to the user.

## None

The team reads and writes no records about itself. Work a persona does still changes project files; say so once.
At the close of a Meeting, ask once whether to keep the minutes. A yes saves that file and leaves the level as
it is. After a no, the question rests for the session.

## Forgetting

"Juno, forget the launcher work" removes that entry, at any level. Confirm with the user before deleting any
record. Existing files stay through a change of level until the user asks for them to go.

Framework version: team-building 0.1.0
