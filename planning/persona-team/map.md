# Map: a general persona-team skill

Label: wayfinder:map

## Destination

Two specs in this repo, ready to hand off: (1) a spec for an agent-agnostic, installable skill that helps a user
generate their own persona team for any kind of project (a thesis, a dinner) and then convene it; (2) a spec for
the test campaign that runs the skill with several model agents over a wide variation of hypothetical projects and
users, including the rubric for what makes a generated team good. Building the skill and running the campaign come
after the map.

## Notes

- **Foundation:** [docs/some_ideas.md](../../docs/some_ideas.md). Founding example: the research group in
  `../research-group-skill/` (kept untouched, a sibling repo) and
  `../ir/docs/agents/research_group/group_dynamics.md`.
- **The philosophy is a field, not a structure.** Moon and sun, the seven chakras and the family are the
  underlying metaphorical field. They guide how teams are designed and are not binding on what the user sees. By
  public release they sit in the background.
- **Settled at charting (2026-11-12):**
  - The smallest team is two personas: the sun-moon pair.
  - The family is one group metaphor among many. Different metaphors suit different projects. The research group
    is not explicitly a family, though the field connects its members to family roles.
  - The skill both generates a team and convenes it. Ways to convene are generic in v1 (a quick opinion, a
    one-on-one with an elder, a group discussion that ends in a next step). Domain-specific workflows (ground
    work, classics shelf) are not in v1.
  - Eventually public, and agent-agnostic: any agent that supports installable skills should be able to use it.
  - Prototype tickets may produce rough artifacts to react to. Nothing else is built inside the map.
- **Tracker:** local markdown. Map is this file; tickets are `issues/NN-<slug>.md` with `Type:`, `Status:`
  (`open`/`claimed`/`resolved`) and `Blocked by:` lines; answers go under `## Answer` in the ticket; research
  findings go in `research/`.
- **Skills each session should consult:** `/grilling` and `/domain-modeling` for grilling tickets (glossary in
  [CONTEXT.md](../../CONTEXT.md)); `/prototype` for prototype tickets; `/writing-for-agents` once spec text is
  being written.

## Decisions so far

<!-- one line per closed ticket: gist + link -->

- [Agent-agnostic skill packaging](issues/01-agent-agnostic-skill-packaging.md) — the Agent Skills format
  (a `SKILL.md` folder) is the common denominator; install paths differ (`.agents/skills/` vs `.claude/skills/`),
  so target both; structured questions, subagents and file-backed records must each fall back to plain chat.
- [Prior art on persona teams](issues/02-prior-art-on-persona-teams.md) — a one-model team adds coverage of
  questions, not correctness; voices merge and drift within about eight rounds; write personas as working habits
  with a criterion and a named excess, re-read at each convening; rubric centres on blind identification and
  disagreement that changes the next step.
- [Testing skills with simulated users](issues/03-testing-skills-with-simulated-users.md) — no eval tool runs a
  multi-turn simulated user, so the campaign needs a custom loop (Claude Agent SDK has the pieces); simulated
  users are too cooperative and must not be the raters; judge per rubric dimension with yes/no questions and two
  model families; start with a 12-conversation pilot read by a human.
- [The field and what every team shares](issues/04-the-field-and-what-every-team-shares.md) — the sun-moon
  pair is coequal, most senior, defined by traits not gender; the user leads a pair-only team and a leader persona
  appears beyond that; the seven qualities are written into personas in plain words; the field is explained only
  on request; six rules hold for every team; the user stays outside the team.
- [Group metaphors](issues/05-group-metaphors.md) — a metaphor specifies seats, authority, friction, tone and the
  pair's form; the skill ships a few worked examples and composes new ones; it designs in the field first and
  dresses in the metaphor second; types give a seat and a register, never behaviour; the team is presented as
  fiction, with no real names and the user not a character.
- [The casting conversation](issues/06-the-casting-conversation.md) — draft, don't interrogate: a quick look, at
  most three opening questions, then a one-screen team proposal the user revises in plain words; a deeper
  interview only on request; short and full lengths; leader authority in three levels; six voices by default.

- [What a generated team looks like](issues/07-what-a-generated-team-looks-like.md) — a persona is 25 to 30 lines
  of second-person prose in eight parts, habits first and at most two sentences of story; `team.md` describes the
  group and ends with one sample meeting; rules, uncovered qualities and design notes are not saved; a pair is
  shown in chat, three or more get a folder.

## Not yet specified

- **Name and public face.** What the skill is called, how much of the field (moon and sun, chakras) appears in
  its public text, and how those traditions are credited. Waits on the field and metaphor decisions.
- **Persistence and evolution of a team.** The file form is settled. Still open: where the folder sits, how a
  team carries across sessions, how the user adds, retires or re-tunes a persona later, and whether a team can be
  reused across projects. Waits on the convening decision.
- **Scaling to the project.** Casting now has a short and a full length. Still open: whether convening and
  record-keeping scale the same way. Probably falls out of the convening ticket.
- **Guardrails.** What the skill does when a user insists on a persona of a real person (the default, set in
  Group metaphors, is to encourage fictional names), and personas that flatter the user. Some of this belongs in
  the rubric; the rest may need its own decision.
- **The research group as a check.** Whether the general skill should be able to regenerate something like the
  existing research group, and whether that is a test case in the campaign.
- **Spec shape and the revision loop.** How the two specs are structured, and how campaign results feed back into
  the skill (what counts as a pass, when to stop revising). Waits on the rubric and campaign design.

## Out of scope

- Building the skill and running the test campaign (they follow the map).
- Changing or replacing the existing `research-group` skill.
