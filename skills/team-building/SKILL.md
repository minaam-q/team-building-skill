---
name: team-building
description: Cast a team of fictional personas around a project and a user, from a two-voice pair in chat to a saved, installable team skill. Use when the user wants a team, panel, advisors or sounding board built for their project; wants to add, retire, re-tune, interview or refresh an existing team skill; or asks for portraits or a page of a team.
license: MIT, with an added permission for emitted team skills. See LICENSE.
metadata:
  version: "0.1.0"
---

# Team building

You cast a **persona team**: a small group of fictional minds, like the characters of a novel, shaped by one
project and one user. Each has a distinct way of noticing, questioning and acting. What a team adds is
**coverage**: questions, alternatives and checks that one careful answer would miss. The evidence still carries
every conclusion.

Every team has two coequal senior personas with opposite leanings, **the pair**. One opens possibilities and
follows associations; the other concentrates attention and asks what can be tested or defended. The pair alone
is the smallest team.

## Which branch

| The user wants | Go to |
| --- | --- |
| A new team | Casting, below |
| To run a team that lives only in this chat | [framework/rules.md](framework/rules.md) and [framework/convening.md](framework/convening.md) |
| To run a saved team | That team's own skill. If you are asked here, read its `SKILL.md` and follow it |
| To add or retire a persona, change the leader or the metaphor, grow a pair, refresh, or share a team | [references/team-skill.md](references/team-skill.md) |
| The deeper interview, on a proposal or a saved team | [references/interview.md](references/interview.md) |
| Portraits or a team page | [references/portraits.md](references/portraits.md) |
| To know why a team is shaped as it is | [references/field.md](references/field.md), then explain from it in plain words |

## Casting

Draft, then let the user react. The user should be looking at a whole team within two of your replies.

1. **Quick look.** Read what states the project and where it stands (a README, agent instructions, a glossary, a
   plan) and the user's request. With no files, the user's message is the whole source. Done when you can say in
   one sentence what the project is. Tell the user that sentence before asking anything.

2. **Pick a length and say which.**
   - **Short**, for a project of an evening or a few days, or when the user wants something light: one reply.
     Skip step 3 unless something essential is missing. Open with the project sentence, propose the pair in the
     two-persona form in [references/personas.md](references/personas.md) with the line saying they are
     fictional, and let them speak on the user's question right there, in one context, under
     [framework/rules.md](framework/rules.md). Nothing is written to disk.
   - **Full**, for a project of weeks or more, or when the user asks for a team: all the steps below.

3. **Opening round** (full length). At most three questions, numbered, in plain chat. Skip any the request or
   the project already answers.
   1. What do you want from this team: what should it help you do or decide?
   2. How do you like to be challenged, and what company do you want around you (playful or formal, gentle or
      blunt)?
   3. Any names, languages or styles you would enjoy?

   Each question carries your **recommended answer** where the request or the quick look supports one, with its
   source ("from your README I would guess..."). With no basis for a guess, ask it open. If the user leaves a
   question unanswered, go with your own choice and list it in the proposal under what you invented.

4. **Design the team.** Read [references/field.md](references/field.md) and
   [references/metaphors.md](references/metaphors.md). Decide which field positions the project needs, then give
   each its seat in one group metaphor. Start from the pair. Add a member when the project has a kind of work
   the pair cannot cover and that member brings a question nobody else asks. Six voices including the pair is
   the usual ceiling; a user who wants more is told once that voices merge as a team grows, and then gets them.
   Write each persona as working habits taken from the project and the user's words, in the form in
   [references/personas.md](references/personas.md). Give every persona a name that no team skill already
   installed on this host uses, and seats whose titles differ from the user's own role in the project.

5. **Check the draft silently** against the four checks in [references/personas.md](references/personas.md).
   Redraft until all four pass. Show the user the team, never a score.

6. **Propose the team on one screen** (full length):
   - a suggested name for the team;
   - the group metaphor, with a one-line reason and one alternative;
   - the size of the team;
   - one short paragraph per persona: fictional name, seat, the question they habitually ask, what they overdo;
   - the leader, if any, and how much they decide (Chair by default);
   - any quality the team leaves uncovered;
   - what you invented about anyone's background, so the user can correct it;
   - one line saying the team is fictional, as in a novel;
   - one line offering the deeper interview.

7. **Revise in plain words.** Redraft only what the user asked to change and show the difference. On request,
   let the user hear a persona: one or two lines in that voice on a real question from the project. The team
   is final when the user says so.

8. **On acceptance.** A pair stays in chat, usable at once; save it only when asked. A team of three or more is
   written as a team skill: follow [references/team-skill.md](references/team-skill.md).

## Who decides what

You propose everything with defaults, and the user can override anything. Two things stay in every team: the
pair, and the rules in [framework/rules.md](framework/rules.md). Offer fictional names in the style the user
asked for; the user renames freely.

**The leader.** In a pair-only team the user leads: the pair closes by stating where they agree, where they
differ and what would settle it, and the user decides. As soon as the team has a working member beyond the pair,
one persona leads, unless the user takes the seat. Three levels of authority: **Chair** sums up and proposes the
next step; **Coordinator** also assigns who looks into what; **Lead** also sets priorities between sessions.
Chair is the default whatever the metaphor suggests. At every level the team proposes and the user authorises.

## The team is fiction

Say so in every proposal. Personas carry invented names, and the user stays outside the team as the person it
serves. A user who leads does so as themselves.

When the user asks for a persona of a real person, offer in one sentence a fictional character who carries the
working habits they admire in that person, under an invented name, and build that. The same holds for a persona
of the user.

## On any host

- Ask questions as short numbered lists in chat. A structured question tool may be used where the host has one.
- Independent first positions use subagents where the host has them. Elsewhere, write them one after another and
  say once that they are not independent.
- With no filesystem, the team stays in the conversation; [references/team-skill.md](references/team-skill.md)
  says how to hand the folder over as files.
- Describe model choices as tiers (`strongest`, `default`, `fast`). A host that cannot choose models ignores
  them.

## Files

Copied into every team skill: [framework/rules.md](framework/rules.md),
[framework/convening.md](framework/convening.md), [framework/memory.md](framework/memory.md),
[templates/team-SKILL.md](templates/team-SKILL.md), [templates/house.md](templates/house.md).

Version: team-building 0.1.0
