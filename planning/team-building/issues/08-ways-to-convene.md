# Ways to convene

Type: grilling
Status: resolved
Blocked by: 01, 04

## Question

Once a team exists, how is it run? Settle the generic set for v1 (a quick opinion, a one-on-one with an elder, a group discussion that ends in a next step): how each is triggered, which personas speak and in what order, whether voices are produced in one context or by separate agents where the host supports it, what record is kept if any, and how proposals and authorisation work outside research. State what changes when the team is only the sun-moon pair. Also settle how it is decided whether the sun-moon pair joins a group discussion, given that there is no fixed rule and they do what the situation asks.

## Answer

Resolved 2026-11-13 by grilling.

**An open set, with three worked examples.** Ways to convene work like group metaphors: the skill ships a few
worked examples and composes new ones when a request fits none of them (a debate between two members, one
persona reviewing a draft, a brainstorm, a toast at the dinner). Every way of convening is stated in six lines:
1. Who speaks, and whether the sun-moon pair is in.
2. Order.
3. Independence: which parts are separate first takes.
4. Length.
5. What it ends with (always something to carry forward, rule 5).
6. What is kept.

The skill tells the user in one line which shape it chose. The six rules hold in every form. The three examples:
- **Ask:** one voice or a few, one answer, in chat. A bare question gets the voices that know something
  relevant, usually two or three. Ends with one line: the next step, or what would settle the open point.
- **One-on-one:** a sustained conversation with any one persona, most often one of the pair. No formal close
  unless the user asks for a summary.
- **Meeting:** a group discussion that ends in something to carry forward (detail below).

**Triggering.** Inferred from the user's words, never triaged up front. Naming a persona means Ask, or a
One-on-one with "talk to" or "sit down with"; "the team" or "meeting" means a Meeting; when unclear, Ask, closing
with a one-line offer to take it to a meeting. The skill asks only when a wrong guess is costly: the signs point
to a way that spawns subagents or writes files and the user's words don't clearly ask for it. Then it asks one
short question with two choices.

**One context or separate agents.** Only a Meeting's first positions are independent: where the host has
subagents, each speaking persona gives a first take in its own subagent, without seeing the others. The exchange
and the close run in the main context. Ask and One-on-one always run in one context. Without subagents, first
takes are written one after another and the skill says once that they are not independent. Orchestration stays
flat ([Agent-agnostic skill packaging](01-agent-agnostic-skill-packaging.md)).

**Re-reading.** At the start of every convening the skill reads the rules from its own files, `team.md`, and the
file of each persona who will speak. In a One-on-one it quietly re-reads that persona's file every six exchanges
or so. A subagent receives its persona file and the rules in its prompt. A team that exists only in chat is
restated from the conversation, and the skill offers to save it if the conversation grows long. This answers the
consequence noted in [What a generated team looks like](07-what-a-generated-team-looks-like.md).

**A Meeting.**
- Order: the leader frames the question in a line or two; members speak juniors before seniors, and on each
  point the one who knows the matter opens; the pair, if present, after the members; the leader closes.
- Whether the pair joins: the leader judges at the opening and says why in one line (if the user leads, the
  skill proposes and the user confirms). The pair joins on questions of direction or priorities, on a
  disagreement members cannot settle from evidence, or when the user asks. Otherwise questions for them are
  written down for a One-on-one. The user can always summon or dismiss them.
- Length: opening, first positions, at most two rounds of exchange, close; in one reply by default. The user can
  ask to run it round by round and interject, as the person the team serves, not as a character.
- Close: agreed; still disputed, with dissent named by persona; unchecked; proposed next steps, each marked
  *proposed*.

**A pair-only team.** All ways still exist. A Meeting is the pair debate: independent first positions where
possible, at most two exchanges, then together they state where they agree, where they differ and what would
settle it. The user decides; nobody closes on a leader's behalf.

**Personas work.** After the user authorises, a persona can do the work, holding its file as working context and
reporting in its voice: what was done, the result, what is unchecked. The owner is the persona the user names;
otherwise the one the leader assigned (Coordinator or Lead); otherwise the one who proposed it. One subagent per
owner where the host has them, in parallel when tasks are independent; otherwise in the main context, one at a
time. Rule 2 holds fully: no invented finished checks. A working persona may carry the work into whatever suits
it, saying plainly what it does and reporting it. Nothing hard to undo or outward-facing (deleting, overwriting,
sending, publishing, spending money) happens without the user. This amends rule 6 in
[The field and what every team shares](04-the-field-and-what-every-team-shares.md). In everyday projects a
proposal is one line ("Proposed: ..., want me to do it?"), not a status ledger.

**Records.** *(Amended 2026-11-14 by [What a team remembers](13-what-a-team-remembers.md): what follows is the
minimal level, except the raw log, which belongs to full recording; at the no-recording level none of it is kept
and minutes are offered once per Meeting.)*
- Ask and One-on-one: nothing by default; one line offering to save, only if the topic looks likely to continue.
- Meeting of a saved team: minutes in the close's shape (date, attendees, agreed, disputed, unchecked,
  proposed). No raw transcript of a meeting and no actions table in v1.
- A long One-on-one (any persona, most useful with the pair): the skill offers once, past about 15 exchanges or
  when the talk turns to direction, to keep a **raw log** that condenses most of the discussion, reconstructed
  from the start if accepted. At the end a separate agent reads the log cold and writes a **review**, as itself
  and not the persona: what was concluded, what changed position, open questions, and suggested consequences for
  the project's direction, each *proposed*.
- The leader reads the latest reviews when opening the next Meeting and puts their open proposals on the agenda.
  Raw logs are never read at convenings.
- Where minutes, logs and reviews are stored belongs to [Where a team lives and how it changes](12-where-a-team-lives-and-how-it-changes.md).

**Model tiers.** A persona may carry a tier, not a model name: `strongest`, `default` or `fast`. The pair default
to `strongest`, members to `default`. A host that can choose models for subagents maps the tiers; one that cannot
ignores them silently. The tier sits in the persona file's header, not its prose.
