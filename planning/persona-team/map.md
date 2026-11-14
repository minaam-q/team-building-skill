# Map: a general persona-team skill

Label: wayfinder:map

## Destination

Two specs in this repo, ready to hand off: (1) a spec for an agent-agnostic, installable **team-building skill**
that casts a persona team with a user for any kind of project (a thesis, a dinner) and, for a longer project,
emits it as a **team skill** of its own; the spec covers what the emitted team skill does (convening, memory,
work on a map); (2) a spec for the test campaign that runs the skill with several model agents over a wide
variation of hypothetical projects and users, including the rubric for what makes a generated team good. Building
the skills and running the campaign come after the map. *(Redrawn 2026-11-14: one skill became builder plus
emitted team skill.)*

## Notes

- **Foundation:** [docs/some_ideas.md](../../docs/some_ideas.md). Founding example: the research group in
  `../research-group-skill/` (kept untouched, a sibling repo) and
  `../ir/docs/agents/research_group/group_dynamics.md`.
- **Field evidence (added 2026-11-14):** the research group has been used on the Ir project for three days and
  audited: `../research-group-skill/research/ir_case_study_2026-11-14/` (report, timeline, proposed controlled
  test). The Ir records are in `../ir/planning/persona_records/` and `../ir/planning/discussions_with_*/`. Read
  the report before any ticket on records, the rubric or the campaign.
- **Retention has three levels (user, 2026-11-14):** full recording, minimal recording (the default) and none,
  with anything in between on request.
- **Two skills (user, 2026-11-14):** the builder casts a team; a team for a longer project can become its own
  installable, modifiable skill, as the research group is for Ir. Emitting a skill is offered, never mandatory:
  a pair stays in chat and a small team can stay a folder. The team skill is meant to work with wayfinder maps,
  with tickets assigned to personas or the user. The earlier v1 limits (no domain-specific workflows) stand.
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
- [Ways to convene](issues/08-ways-to-convene.md) — an open set, with Ask, One-on-one and Meeting as worked
  examples in a six-line form; inferred from the user's words; only a meeting's first positions are independent
  subagents; juniors speak before seniors; personas may do authorised work (rule 6 amended: nothing irreversible
  or outward-facing without the user); minutes for meetings, raw log plus cold review for long one-on-ones;
  personas carry a model tier.
- [What a team remembers](issues/13-what-a-team-remembers.md) — three levels written as one line of prose in
  `team.md`: minimal by default (a short memory file per persona, two or three lines per piece of work, plus
  minutes and reviews), full (everything verbatim, the user's messages stamped with their addressee, plus
  self-assessment), none (minutes offered once per meeting); memory stays with the project, not the team skill.

## Not yet specified

- **Name and public face.** What the skill is called, how much of the field (moon and sun, chakras) appears in
  its public text, and how those traditions are credited. Waits on the field and metaphor decisions.
- **Guardrails.** What the skill does when a user insists on a persona of a real person (the default, set in
  Group metaphors, is to encourage fictional names), personas that flatter the user, and working personas that
  overreach (Ways to convene allows them to extend authorised work). Some of this belongs in
  the rubric; the rest may need its own decision.
- **The research group as a check.** Whether the general skill should be able to regenerate something like the
  existing research group, and whether that is a test case in the campaign. The Ir case study now offers five
  frozen tasks from real project states that such a check could replay.

## Out of scope

- Building the skill and running the test campaign (they follow the map).
- Changing or replacing the existing `research-group` skill.
