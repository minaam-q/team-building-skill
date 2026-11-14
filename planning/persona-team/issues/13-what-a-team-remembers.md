# What a team remembers

Type: grilling
Status: resolved
Blocked by: 08

## Question

In a longer project the conversations with the team advance the work, so what is kept matters. The user has set
the frame (2026-11-14): three levels of retention, not fixed schemes.

1. **Full recording**, for research and development of the team itself. This is what the Ir project does: every
   discussion verbatim with its actions and recommendations, plus a self-assessment by each voice of whether the
   persona changed its output.
2. **Minimal recording**, the default: a small, essential, indexed memory per persona, enough that "Arje, what did
   you work on two weeks ago?" gets an answer that points at the work.
3. **No recording**: the personas live in the moment.

The user can ask for 1, 3 or something in between, and handles the team as they see fit.

Settle: what one entry of the minimal memory holds and who writes it and when; whether there is one index for the
team or one per persona; how a persona reads it (at every convening, or only when asked about the past); what
full recording adds on top, and which of the Ir instruments (verbatim discussions, actions tables, versioned
recommendations, persona self-assessment records) belong to it; how the level is chosen, changed and stated; what
"in between" looks like without becoming a settings menu; and how the records decided in
[Ways to convene](08-ways-to-convene.md) (minutes, raw log, review) sit across the levels. That answer may need
amending. Short pairs kept in chat and one-off questions should cost nothing.

Evidence to read first: `../research-group-skill/skills/research-group/workflows/persona-record.md`, the Ir
records in `../ir/planning/persona_records/` and `../ir/planning/discussions_with_*/`, and the case study in
`../research-group-skill/research/ir_case_study_2026-11-14/report.md`.

## Answer

Resolved 2026-11-14 by grilling.

**Three levels, stated as prose.** Minimal is the default. The level is one line in `team.md`, written in the
user's own words, so anything in between ("minimal, but keep Bafar's conversations in full", "nothing for Niko's
errands") is simply written there and followed. There are no flags. Nothing is asked at casting: when a team is
first saved, the skill says in one line that it will keep a short memory per persona and that the user can ask
for everything or nothing. The user changes the level in plain words at any time. Moving down stops writing and
deletes nothing.

**Minimal recording (level 2).**
- One memory file per persona, newest first. No team index: the memory files are the index at every level.
- One entry is two or three lines: date, topic slug, one sentence on what was asked and what the persona did or
  concluded (the request paraphrased, never verbatim), pointers to the files, tickets or commits it touched, and
  an optional closing "left open: ...". No status ledger.
- When the work was a ticket on a map, the entry is one line naming and linking the ticket, plus "left open" if
  anything is. The ticket holds the substance.
- What earns an entry: a task done, a Meeting, a One-on-one that reached a position or changed direction. A plain
  Ask writes nothing. A pair kept in chat writes nothing at any level.
- Who writes: the agent that produced the voice, as its last step, in plain words and not in character. A
  subagent returns its entry with its report and the main agent appends it.
- When read: the last five or so entries of each speaking persona at the start of a Meeting or a task; the whole
  file only when the user asks about the past; nothing for a plain Ask.
- Also kept at this level: minutes of a Meeting and the review of a long One-on-one.

**Full recording (level 1)** is level 2 plus:
- every conversation verbatim in a dated topic directory. The user's messages are kept unedited, each stamped
  with the time and whom it addressed, including messages addressed to nobody in particular;
- the actions table and versioned recommendations, where a convening produces them;
- the raw log of a long One-on-one;
- a self-assessment record per voice ("has my persona changed my output?") on a fixed scale: none, style-only,
  some-substance, substantial. It is presented as an instrument for developing the team;
- the actual model used.
Each memory entry also points at its topic directory and self-assessment record. When level 1 is switched on the
skill says once where the records go and that they hold the user's messages word for word. It does not touch
`.gitignore`.

**Why the user's words are the primary data.** The user's hypothesis: agents may act much the same under
different personas, but the human leading them asks differently depending on whom they address, and that changes
what comes back. So the agent makes no verdict on how the user prompted; level 1 only guarantees the prompts
survive with their addressee, for later comparison. The user is never asked whether a persona changed what they
asked, since asking would change how they talk. They can add a note in their own words at any time, saved
verbatim in the topic directory.

**No recording (level 3).** The team reads and writes no records about itself. Work a persona does still changes
project files; the skill says so once. If the user calls a Meeting, the skill asks once at the close whether to
keep the minutes. A yes saves that file and does not change the level; after a no it does not ask again in that
session.

**Forgetting.** "Arje, forget the launcher work" removes that entry at any level. Deleting records is confirmed
first. Existing files stay through a change of level until the user asks for them to go.

**Memory belongs to the project.** If the team is installed as its own skill
([The team as its own skill](15-the-team-as-its-own-skill.md)), the skill carries who the personas are and each
project holds what they did there, with its own retention line.

**Amends [Ways to convene](08-ways-to-convene.md), Records:** minutes and the review are level 2; the raw log is
level 1; "no actions table in v1" holds at level 2 and is lifted at level 1.

**For other tickets.** Where the files sit:
[Where a team lives and how it changes](12-where-a-team-lives-and-how-it-changes.md). The through-the-user effect
and the test campaign: noted in [What makes a team good](09-what-makes-a-team-good.md) and
[Test campaign design](10-test-campaign-design.md).
