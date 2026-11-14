# The casting conversation

Type: grilling
Status: resolved
Blocked by: 04, 05

## Question

How does the skill and the user arrive at a team? Settle: what the skill reads from the project before asking anything; what it asks and in how many turns; what it proposes versus what the user chooses (metaphor, size, the leader's temperament and authority, names, tone from playful to formal); how the conversation shrinks for a dinner and grows for a thesis; and how the user changes the proposed team before accepting it.

## Answer

Resolved 2026-11-12 by grilling.

**Before asking anything.** A quick look, not a study. The skill reads what states the project and where it
stands (a README, agent instructions, a glossary, a plan) and the user's request, and stops once it can say in one
sentence what the project is. With no files, the user's message is the whole source. It tells the user what it
understood before asking anything.

**Draft, don't interrogate.** The skill asks one short opening round and then proposes a whole team for the user
to react to.

**The deeper interview, on request.** The skill offers, in one line, a longer interview for a user who wants to
go deep into the team's design. It runs only when the user chooses it. It is never the default and never a
condition for getting a team.

**The opening round.** At most three questions; any that the request or the project already answers are skipped.
1. What do you want from this team: what should it help you do or decide?
2. How do you like to be challenged, and what kind of company do you want around you (playful or formal, gentle
   or blunt)?
3. Any names, languages or styles you would enjoy?

**The first proposal.** One screen:
- the group metaphor, with a one-line reason and one alternative;
- the size of the team;
- one short paragraph per persona: fictional name, seat, the question they habitually ask, what they overdo;
- the leader, if any, and how much they decide;
- any quality the team leaves uncovered;
- one line saying the team is fictional, as in a novel.

Full persona files are written only after the user accepts.

**Who decides what.** The skill proposes everything with defaults and the user can override anything, except
that the sun-moon pair and the six rules cannot be removed. The skill offers fictional names in the style the
user asked for; the user renames freely.

**The leader's authority.** Three levels. The default is Chair. At every level the team proposes and the user
authorises.
1. Chair: sums up and proposes the next step.
2. Coordinator: also assigns who looks into what.
3. Lead: also sets priorities between sessions.

**Team size.** Start from the pair. Add a member only when the project has a kind of work the pair cannot cover,
and only if that member brings a question nobody else asks. Default cap: six voices including the pair. The user
can go past it after being told that voices merge as a team grows
([Prior art on persona teams](02-prior-art-on-persona-teams.md)).

**Two lengths.** The skill picks one from the request and says which it picked.
- Short: no opening round unless something essential is missing. It proposes the pair in a light metaphor
  straight away, and the user can start using them in the same reply. Nothing is written to disk unless asked.
- Full: the quick look, the opening round, the one-screen proposal, revisions, then persona files.

A short team can be grown into a full one later.

**Revising.** In plain words. The skill redrafts only what changed and shows the difference. Before accepting,
the user can ask to hear a persona: one or two lines in that voice on a real question from the project. The team
is final only when the user says so.

**Across agents.** The conversation is written as plain chat with short numbered questions. A structured
question tool may be used where one exists; nothing depends on it
([Agent-agnostic skill packaging](01-agent-agnostic-skill-packaging.md)).

*Amended 2026-11-14 by [The deeper interview](11-the-deeper-interview.md): each opening question carries the
skill's recommended answer where the request or the quick look supports one, and is asked open otherwise.*
