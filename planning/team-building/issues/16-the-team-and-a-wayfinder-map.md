# The team and a wayfinder map

Type: grilling
Status: resolved
Blocked by: 15, 17

## Question

The user's proposal (2026-11-14): a team skill is best combined with Matt Pocock's wayfinder. Wayfinder charts a
map of tickets, and each ticket can be assigned to a persona or to the user. The Ir project already works this
way: 26 of its 88 tickets carry a `Persona:` owner line, and discussions point at tickets.

Settle: whether the team skill requires wayfinder, works with it when present, or carries its own small
equivalent, given that the skill is to be public and agent-agnostic; how a ticket is assigned to a persona and
what claiming means when the claimant is a persona; which persona charts and keeps the map (the leader?); how
ticket types meet ways of convening (a grilling ticket as a One-on-one, a research ticket as a persona's task);
how a persona's memory entry and a ticket's answer point at each other without saying the same thing twice; and
what a team does in a project with no map. This takes over the fog patch on following work through a long
project.

## Answer

Resolved 2026-11-14 by grilling. Facts behind it:
[research/17-wayfinder-terms-and-shape.md](../research/17-wayfinder-terms-and-shape.md).

**The team works on a map when there is one, and never charts one.** Wayfinder is not required and the team
skill carries no equivalent of its own. With no map, open work stays where it already is: the proposals in
minutes and the "left open" lines in memory. When those pile up, the leader says once, in one line, that the
project looks big enough for a map, and names wayfinder.

**Written in general terms, with wayfinder as the one worked example.** The framework text names and links
wayfinder and copies none of its text. The team skill follows the project's own tracker doc, not wayfinder's
wording, so a change upstream (such as `CONTEXT.md` becoming `GLOSSARY.md`) does not break it. Other trackers
(Backlog.md, beads) are not specified in v1; a user who runs one describes it in `house.md`.

**Owner and claim are separate.**
- A ticket carries one header line: `Owner: Arje` or `Owner: user`. This replaces the Ir project's `Persona:`
  line, so that the user can be named. One owner per ticket; helpers are named in the answer text.
- On GitHub and GitLab the line sits at the top of the issue body. It is never a label and never the native
  assignee.
- The claim stays wayfinder's and is not touched. The team writes only the tracker doc's own status words
  (`open`, `claimed`, `resolved`): no names, dates, triage roles or free text in `Status:`.

**Assigning and authorisation.** The leader proposes owners and the user confirms; the line is written only after
that yes. On research tickets and unattended task tickets the line is the standing authorisation, so "Arje, take
your next ticket" needs no second approval. Anything hard to undo or outward-facing still stops for the user
([Ways to convene](08-ways-to-convene.md)). On grilling and prototype tickets, owning means leading the
conversation, never deciding. Work done before a ticket existed gets a memory entry, not a ticket written
afterwards.

**How each ticket type is worked.**

| Ticket type | How the team works it |
|---|---|
| Research | the owner's task, in a subagent that receives the persona file with the research brief |
| Task, unattended | the owner's work, reported in their voice |
| Task for the human | `Owner: user`, with a checklist |
| Grilling | a One-on-one led by the owner: the persona asks, the user answers |
| Prototype | the owner builds, the user chooses |

Wayfinder's human-in-the-loop rule stands unchanged: personas never settle a grilling or prototype ticket among
themselves.

**A Meeting resolves no ticket.** It may prepare one (positions, the questions to put to the user) and it ends as
already decided: agreed, disputed, unchecked, proposed. With a map the proposals may be "new ticket, owner X" or
"proposed answer for ticket Y", and the minutes link the tickets. Each ticket is then resolved in its own session
under wayfinder's rules. The skill never writes an override into a map's Notes; a user who wants meetings to
close tickets writes that themselves.

**The leader keeps the map.** At a Meeting's opening the leader reads the map's Destination, Decisions so far and
frontier, not every ticket body. The team writes three things: an `Owner:` line, a new ticket the user has
approved, and a resolution with its map pointer when a ticket is worked. Charting, redrawing the destination and
ruling work out of scope stay the user's own wayfinder sessions. In a pair-only team the user is the leader and
nothing changes.

**How each side learns of the other.**
- Every team's `convening.md` has a short section, "when the project has a map". At a Meeting or a task the skill
  reads the project's tracker doc if there is one, otherwise looks for a file labelled as a map; if it finds
  several it asks which is meant.
- The map's Notes get one line naming the team skill and the `Owner:` convention. The skill proposes it the first
  time the team works on the map and writes it only on the user's yes. It is a pointer, not an override.

**Picking the next ticket.** For a named persona: the first frontier ticket with their `Owner:` line, in order.
If there is none, the skill says so and lists the unowned frontier tickets; it does not take another persona's
ticket. A plain wayfinder session, reading the Notes line, works a persona-owned ticket in that owner's voice if
the team skill is installed, and for `Owner: user` offers a checklist instead of working it. Unowned tickets
behave as wayfinder says.

**Answer and memory.** The substance goes only under the ticket's `## Answer`, whose first line is "Resolved
<date> by <owner>". The owner's memory entry is the one line decided in
[What a team remembers](13-what-a-team-remembers.md): the ticket's name and link, plus "left open" if anything
is. A ticket links minutes only when a Meeting prepared it. At the no-recording level the answer is still
written, because it is a project file, and no memory entry is made.

**Initiate with a map.** Each member reads the map's Destination, Decisions so far and frontier as the project's
own statement of where it stands. A first look gains one optional line, "tickets I would take". The leader's
closing screen ends with proposed owners for the frontier tickets, each marked proposed.

**Parking and reassigning,** only on the user's word. Parking sets `Status:` back to `open`, keeps the `Owner:`
line and adds one dated line under the ticket's `## Comments` saying who parked it and why. Reassigning rewrites
the `Owner:` line and adds the same kind of line.

**Execution.** Whether a map carries execution is the map's own business, stated in its Notes by the user. The
team works whatever tickets exist. Its one addition: a task ticket the team proposes carries a `## Done when`
line.

**Glossary.** Owner widened to include the user; Claim and Map added ([CONTEXT.md](../../../CONTEXT.md)).

**For later tickets.** Where the "when the project has a map" section and the Notes line sit among the files:
[Where a team lives and how it changes](12-where-a-team-lives-and-how-it-changes.md). Whether owners and
answers are honoured in practice is a rubric question:
[What makes a team good](09-what-makes-a-team-good.md).
