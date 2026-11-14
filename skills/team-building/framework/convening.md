# Convening

How this team is run on a question. Read at the start of every convening, after `rules.md`.

## Every convening

1. Read `rules.md`, `team.md`, `house.md`, and the persona file of each persona who will speak.
2. Pick the way of convening from the user's words (next section) and say which in one line.
3. At the recording level in force (`memory.md`), read what that level says to read.
4. Run it. Each voice follows its own persona file: its criterion decides what it says, its "how to answer" part
   decides the form.
5. Close as that way of convening closes, then write what `memory.md` says to write.

A way of convening is stated in six lines: who speaks (and whether the two seniors are in), order, independence,
length, what it ends with, what is kept. Four are worked out below. When a request fits none of them (a debate
between two members, one persona reviewing a draft, a brainstorm, a toast), compose one in the same six lines,
show the lines to the user in brief, and run it. `house.md` may hold further ways this team has settled on; they
are used like the ones here.

## Picking the way

| The user's words | Way |
| --- | --- |
| Names a persona, or asks a bare question | Ask |
| "Talk to", "sit down with" a persona | One-on-one |
| "The team", "a meeting", "discuss" | Meeting |
| "Initiate", "set the team up here", "get to know the project" | Initiate |
| Unclear | Ask, closing with a one-line offer to take it to a Meeting |

Ask the user only when a wrong guess is costly: the signs point to a way that starts subagents or writes files
and their words do not clearly ask for it. Then ask one short question with two choices.

## Ask

- **Who:** the named persona. For a bare question, the two or three who know something relevant.
- **Order:** the one who knows the matter first.
- **Independence:** none; one context.
- **Length:** one answer per voice, in the form its persona file gives.
- **Ends with:** one line: the next step, or what would settle the open point.
- **Kept:** nothing. If the topic looks likely to continue, one line offering to save it.

When a question falls where the persona is quiet, the answer says so.

## One-on-one

- **Who:** one persona and the user, for as long as the user likes.
- **Independence:** none; one context. Re-read the persona's file quietly every six exchanges or so.
- **Ends with:** no formal close unless the user asks for a summary.
- **Kept:** nothing by default. Once, past about fifteen exchanges or when the talk turns to the project's
  direction, offer to keep it; what is kept is in `memory.md` (a review; at full recording also a raw log).

## Meeting

- **Who:** the leader and the members the question touches. Whether the two seniors join: the leader judges at
  the opening and says why in one line. They join on questions of direction or priorities, on a disagreement the
  members cannot settle from evidence, or when the user asks. Otherwise questions for them are written down for
  a One-on-one. The user can summon or dismiss them at any point. When the user leads, propose who attends and
  let the user confirm.
- **Order:** the leader frames the question in a line or two. Members give first positions, juniors before
  seniors; on each point the one who knows the matter opens. The two seniors, if present, speak after the
  members. The leader closes.
- **Independence:** first positions only. Where the host has subagents, each speaking persona writes its first
  position in its own subagent, on its model tier if the host can choose models, given `rules.md`, its persona
  file, its recent memory entries and the question, and seeing none of the others. Say beforehand in one line
  how many agents will run. Without subagents, write the first positions one after another and say once that
  they are not independent. The exchange and the close run in the main context. Subagents are started only by
  the main agent.
- **Length:** opening, first positions, at most two rounds of exchange, close; in one reply. On request, run it
  round by round so the user can interject, as the person the team serves.
- **Ends with:** the close, in four parts: **agreed**; **still disputed**, with dissent named by persona;
  **unchecked**; **proposed next steps**, each marked *proposed* and each with an owner, a rough cost and a
  stopping condition. Keep the list short: one step for a project of an evening or a day, three for weeks, five
  for months or more. A minority view that was not answered stays in the close.
- **Kept:** minutes, in the close's shape (`memory.md`).

The leader's authority is stated in `team.md`. A Chair sums up and proposes the next step. A Coordinator also
assigns who looks into what. A Lead also sets priorities between sessions. At every level the user authorises.

When opening a Meeting the leader reads the latest reviews of One-on-ones and puts their open proposals on the
agenda.

**A team of two.** The Meeting is the two seniors' debate: independent first positions where possible, at most
two exchanges, then together they state where they agree, where they differ and what would settle it. The user
decides.

## Initiate

The team gets to know a project. Run it only on request. When the team is convened in a project that looks long
and has no memory home yet, offer it once, in one line at the close of that convening. It can be repeated as a fresh look; earlier ones are kept and dated.

- **Who:** each member. The two seniors read the first looks and write none unless asked; in a team of two, the
  two write them.
- **Order and independence:** in parallel, each in its own subagent on its model tier, seeing none of the
  others. One line beforehand says how many agents will run. Without subagents, one after another, saying once
  that they are not independent.
- **Length:** each writes a **first look**, about half a page from its seat: what the project is and where it
  stands, what it would check first, one open question, what it did not read. A first look may end with "what I
  would want to read", each item marked held, not held or unverified, listing only what was checked to exist.
  Leave that part out when it is empty. Fetching anything waits for the user.
- **Ends with:** the leader's one screen: where the first looks agree, where they differ, what nobody covered,
  and an offer to take the differences to a Meeting.
- **Kept:** each first look, and one memory entry per member pointing at it (`memory.md`). At the no-recording
  level, say before starting that nothing will be written, and give the first looks and the summary in chat.

## Personas work

After the user authorises a piece of work, a persona does it, holding its file as working context, and reports
in its voice: what was done, the result, what is unchecked.

- **Owner:** the persona the user names; otherwise the one the leader assigned (Coordinator or Lead); otherwise
  the one who proposed it.
- **Where:** one subagent per owner where the host has them, in parallel when the tasks are independent;
  otherwise in the main context, one at a time.
- **Limits:** `rules.md`, in full. In an everyday project a proposal is one line ("Proposed: ..., want me to do
  it?").

## When the project has a map

Some projects keep a map: a tracked set of tickets toward a destination, kept by a planning skill outside this
team. [Wayfinder](https://github.com/mattpocock/skills) is the worked example. The team works on a map when one
exists and leaves charting to the user's own planning sessions. With no map, open work stays in the proposals in
minutes and the "left open" lines in memory; when those pile up, the leader says once, in one line, that the
project looks big enough for a map.

**Finding it.** At a Meeting or a task, read the project's tracker doc if its agent instructions name one;
otherwise look for a file labelled as a map at the project root and under `docs/` and `planning/`. If there are several, ask which is meant. Follow the project's tracker doc for status words
and file layout.

**Owner.** A ticket may carry one header line, `Owner: <persona>` or `Owner: user`. The leader proposes owners
and the user confirms; the line is written after that yes. The owner is separate from the tracker's claim, which
the team leaves as the tracker defines it. `Status:` holds only the tracker's own status words.

**Working a ticket, by type.**

| Ticket type | How the team works it |
| --- | --- |
| Research | The owner's task, in a subagent that receives the persona file with the research brief |
| Task, unattended | The owner's work, reported in their voice |
| Task for the human | `Owner: user`, with a checklist |
| Grilling | A One-on-one led by the owner: the persona asks, the user answers and decides |
| Prototype | The owner builds, the user chooses |

On research and unattended task tickets the `Owner:` line is the standing authorisation, so "take your next
ticket" needs no second yes; the stop list in `rules.md` still applies. A ticket that needs the human's decision
is resolved by the human's answer.

**Next ticket.** For a named persona: the first unblocked, unclaimed open ticket with their `Owner:` line, in
order. If there is none, say so and list the unowned ones.

**A Meeting and the map.** The leader opens by reading the map's destination, its decisions so far and its open
unblocked tickets. A Meeting prepares tickets (positions, the questions to put to the user) and its proposals
may be "new ticket, owner X" or "proposed answer for ticket Y", with links in the minutes. Each ticket is then
resolved in its own session.

**What the team writes on a map:** an `Owner:` line; a new ticket the user approved (a task ticket carries a
`## Done when` line); and, when a ticket is worked, its resolution with the map's pointer to it. The substance
goes under the ticket's `## Answer`, whose first line is "Resolved <date> by <owner>".

**Parking and reassigning,** on the user's word. Parking sets the status back to open, keeps the `Owner:` line
and adds one dated line under the ticket's `## Comments` saying who parked it and why. Reassigning rewrites the
`Owner:` line and adds the same kind of line.

**The pointer in the map.** The first time the team works on a map, propose one line for the map's notes that
names this team skill and the `Owner:` convention. Write it on the user's yes.

**Initiate with a map.** Each member reads the destination, the decisions so far and the open unblocked tickets
as the project's own statement of where it stands. A first look gains one optional line, "tickets I would take".
The leader's closing screen ends with proposed owners for those tickets, each marked *proposed*.

Framework version: team-building 0.1.0
