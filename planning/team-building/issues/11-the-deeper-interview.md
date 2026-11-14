# The deeper interview

Type: grilling
Status: resolved
Blocked by: 06, 07

## Question

The casting conversation offers an optional longer interview for a user who wants to go deep into the team's design. What does it ask? A persona file now has eight parts (see [What a generated team looks like](07-what-a-generated-team-looks-like.md)). Settle: which of those parts the interview asks the user about and which the skill still drafts; whether it runs before the first proposal or as a revision of it; how many questions and in what order; what it asks about the team as a whole (the pair's relation, the leader, the sample meeting); and when it stops.

## Answer

Resolved 2026-11-14 by grilling.

**Always on top of a proposal.** Even when the user asks for the interview at the very start, the skill does the
quick look, the opening round and the one-screen proposal first, then interviews against it. Every answer shows
up as a visible change to the proposal.

**It is about the user and the project, not a persona form.** The interview never walks the eight parts persona
by persona. It asks things only the user knows, such as where a similar project went wrong before, what kind of
objection they tend to wave away, what "done" means here, and whose advice they actually take. The skill turns
each answer into persona changes and says which.

**What the answers shape.** Four of the eight parts:
- the criterion in "how they work" (part 2);
- what they overdo, and which excess the user can live with (part 4);
- what they do when the user is wrong (part 6);
- how to answer: what to lead with and how much (part 7).

The skill always drafts story, relations and the line in voice (parts 3, 5, 8) and shows them for correction.
Name, seat and temperament (part 1) come from the proposal.

**Three rounds, at most three questions each.** Numbered, in plain chat. A question already answered is skipped,
and the user may skip any. Nine questions at most unless the user asks for more.
1. The user and the project.
2. The team as a whole:
   - the pair: what the two should disagree about on this project;
   - the leader: which of the three authority levels, asked through a scenario ("the team splits on what to do
     next; who settles it?");
   - the sample meeting: one real open question from the project, on which the sample meeting in `team.md` is
     written.
3. Individual members, only those the user names as wanting to go deeper on, one question each.

Friction and tone are asked again only if the opening round skipped them. After each round the skill redrafts
only what changed and shows the difference.

**Every question carries the skill's own recommended answer** (user's addition). Two kinds, both marked:
- a design choice gets a plain recommendation with a one-line reason;
- a fact about the user gets a guess that names its source ("from your README I would guess…"). With no basis
  for a guess, the skill says so and asks the question open. It never invents a history for the user.

A recommended answer is always the one the current draft is built on, so the user sees what was assumed and
overrides only where it is wrong. The same rule now applies to the three opening questions; this amends
[The casting conversation](06-the-casting-conversation.md).

**When it stops.** When the user says enough; after round 3; or by itself when a round changed nothing, which
includes the user accepting every recommendation. The skill then says in one line that the proposal already
reflects those answers and offers the next round or the audition. After each round it offers to stop in one
line. Anything unasked stays as drafted. Stopping early never costs the user the team.

**It ends with an audition.** Every persona gives one or two lines in voice on the real question from round 2.
This is the user's last check and becomes the draft of the sample meeting. Then the user accepts or keeps
revising, as in ordinary casting.

**The user's answers are not saved.** The skill shows once, in three or four lines, what it took from them.
Personas are written as habits and never quote or cite the user's remarks ("you repeat a point once if it is
passed over", not "the user ignores juniors"). Questions are about how the user works, never about their life.

**The field.** Unchanged from [The field and what every team shares](04-the-field-and-what-every-team-shares.md):
plain words throughout, explained only if the user asks. The interview does not offer it.

**A pair, and a saved team.** A pair gets rounds 1 and 2 without the leader question. A saved team can be
interviewed through the team-building skill, whole or one member ("go deeper on Niko"); this counts as a change
through the builder, shows the previous wording and leaves the dated line in `team.md`.
