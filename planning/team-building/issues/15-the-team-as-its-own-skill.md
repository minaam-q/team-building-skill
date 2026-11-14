# The team as its own skill

Type: grilling
Status: resolved
Blocked by: 01, 07, 08, 13

## Question

The user's proposal (2026-11-14): for a longer project, the team that casting produces becomes a stand-alone
installable skill of its own, as the research group is for the Ir project. It stays modifiable. So there are two
skills: the **team-building skill** (this effort's product), which builds a team and the framework it works in,
and the **team skill** it emits, which is the team.

Settle: what the emitted skill contains (persona files, `team.md`, the six rules, the ways of convening, the
retention line) and what it leaves behind in the builder; whether the team skill works with the builder
uninstalled; how it is named and described so a host triggers it on the personas' names; where it is installed
(project or user level, on each kind of host, see
[Agent-agnostic skill packaging](01-agent-agnostic-skill-packaging.md)); how the user modifies it afterwards
(by hand, by talking to the team, or by going back to the builder); when a team is emitted as a skill and when it
stays a plain folder or stays in chat; and how the builder's later improvements reach a team already emitted.

## Answer

Resolved 2026-11-14 by grilling.

**The team skill stands alone.** It works where the team-building skill was never installed. This amends
[What a generated team looks like](07-what-a-generated-team-looks-like.md): the rules travel with the team.

**Two states only: in chat, or installed in the project.** Every saved team is written as a skill folder from the
start; there is no export step and no saved-but-not-installed state. A pair stays in chat. A user who wants the
folder somewhere else says so and gets it there. This supersedes "three or more get a folder" in the same ticket
and the "offered for a longer project" wording in [What a team remembers](13-what-a-team-remembers.md).

**What the folder holds.**
- `SKILL.md`: names the personas, says how to pick a way of convening, links everything else. Its description
  lists every persona by name and seat and says to use it when the user names any of them or asks for the team.
  Its last line names the builder and its version (the credit, and what a refresh checks).
- `team.md` and `personas/`, as already decided. `team.md` also carries the retention line and, at its foot, one
  dated line per change to the team.
- `rules.md`, `convening.md`, `memory.md`: the framework, copied verbatim from the builder and stamped with its
  version.
- `house.md`: this team's own ways of convening and standing habits, written by the user or by the team at the
  user's request. A refresh never touches it. This is how domain-specific workflows enter a team without entering
  the builder.
- No scripts and no host-specific manifest.

**What stays in the builder:** the field, group metaphors, the casting conversation, the deeper interview and the
visual art directions. Moon, sun and the chakras appear in no emitted skill.

**Name.** The user names the team at casting or accepts a suggestion; the folder is its slug. Fictional names
only. At casting the builder looks at the teams it can see and avoids reusing a persona's name; if two installed
teams still share one, the skill asks which is meant.

**Where.** Project level by default, in the directory the current host reads; on request also the other
directory as a copy, named plainly so the user knows there are two. User level only when the user says the team
serves several projects. On a host with no filesystem the team stays in the conversation, and on request the
builder produces the folder as files to download and upload, with one line on how.

**Changing the team.**
- By hand: the files are plain prose.
- By talking to the team: an edit to one persona file ("make Niko blunter"). The team skill does it, shows the
  change and gives the previous wording in the reply.
- Through the builder: adding or retiring a persona, changing the leader or the metaphor. If the builder is not
  installed, the team skill says so and does not improvise.
- Every change leaves one dated line at the foot of `team.md`.

**Refresh.** Never automatic. On request the builder replaces the three framework files if their stamp is older,
shows what changed, and never touches `team.md`, `personas/` or `house.md`. Framework files edited by hand are
flagged and left alone.

**One team, several projects.** A user-level team works at once in a new project. Nothing is created until the
first thing worth a memory entry; then the project's memory home is created and the skill says so in one line,
with the retention level. `team.md` holds the team's usual level; the project's memory home may carry its own
line, which wins there. There are no layered overrides of personas: a project that needs a tailored team gets a
whole copy at project level, which is then its own team. If a user-level and a project-level team share a name,
the skill says once which one is speaking.

**Sharing.** The folder holds no memory and can be handed over as it is. The skill never shares anything itself.
When the user says they want to pass the team on, it points out the lines that mention their project (the sample
meeting, persona asides) and offers a copy with those generalised.

**Initiate: a fourth worked way of convening** (user's addition), in every team's `convening.md`.
- Trigger: only on request ("initiate", "set the team up here", "get to know the project"). Offered once, in one
  line, the first time a team is convened in a project that looks long. Repeatable as a fresh look; earlier ones
  are kept and dated.
- Who and order: each member, in parallel, in their own subagent on their model tier, without seeing the others.
  Without subagents, one after another, and the skill says once that they are not independent. One line
  beforehand says how many agents will run. The pair read the first looks and write none unless asked; in a
  pair-only team the two write them.
- What each produces: a **first look**, about half a page from their seat: what the project is and where it
  stands, what they would check first, one open question, what they did not read. Saved in the project's memory
  home; the member's first memory entry points at it.
- Reading list: a first look may end with "what I would want to read", each item marked held, not held or
  unverified; nothing cited that was not checked; nothing downloaded without the user. Left out when empty. A
  team can make it a standing shelf in `house.md`.
- Ends with: the leader's one screen (where the first looks agree, where they differ, what nobody covered) and an
  offer to take the differences to a Meeting.
- At the no-recording level: first looks and summary are given in chat, nothing is written, and the skill says so
  before starting.

**For later tickets.** The exact paths:
[Where a team lives and how it changes](12-where-a-team-lives-and-how-it-changes.md), whose questions on
changing a team and reuse across projects are answered here. Initiate with a map present:
[The team and a wayfinder map](16-the-team-and-a-wayfinder-map.md).


*Note from [Portraits and visual pages](14-portraits-and-visual-pages.md), 2026-11-14: a team skill folder may also hold `portraits/` (prompts, images, team page), which a refresh never touches.*
