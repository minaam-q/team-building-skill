# team-building-skill

An agent skill that casts a team of fictional personas around a project and a user. A dinner gets two voices in
chat. A thesis gets a team of up to six, saved as its own installable **team skill** that can hold meetings,
do authorised work, send cheap **helpers** on data-heavy jobs and keep a short memory in the project.

Status: **0.2.0.** 0.1.0 was the first version; 0.2.0 adds helpers and a fuller refresh. Written from the decisions recorded on the
[`dev` branch](https://github.com/minaam-qamar/team-building-skill/blob/dev/planning/team-building/map.md).
It has not been through the planned test campaign.

## Install

The skill is the folder [skills/team-building/](skills/team-building/), in the Agent Skills format (a `SKILL.md`
plus Markdown files). Copy or symlink it into the skills directory your agent reads:

```bash
git clone git@github.com:minaam-qamar/team-building-skill.git
cd team-building-skill

# Claude Code
mkdir -p ~/.claude/skills && ln -s "$PWD/skills/team-building" ~/.claude/skills/team-building

# Codex, Gemini CLI, Cursor, opencode, Copilot
mkdir -p ~/.agents/skills && ln -s "$PWD/skills/team-building" ~/.agents/skills/team-building
```

Use `cp -r` in place of `ln -s` for a snapshot. For one project only, use `.claude/skills/` or `.agents/skills/`
inside that project. On claude.ai, zip the `team-building` folder and upload it as a skill.

## Use

- *"Give me a couple of voices to think this dinner through with."* A pair in chat, usable in the same reply.
  Nothing is saved.
- *"Build me a team for this project."* A quick look at the project, at most three questions, then a whole team
  proposed on one screen for you to revise in plain words.
- *"Go deeper."* An optional interview about you and the project, each question with a recommended answer.
- Once you accept, a team of three or more is written into the project as its own skill. From then on you
  address it directly: *"Noor, what do you make of this?"*, *"hold a meeting on the web edition"*,
  *"initiate the team here"*.
- *"Add a typesetter to the team"*, *"give the team helpers"*, *"refresh the team"*, *"make portraits"* go
  through this skill again.

## Helpers (0.2.0)

A full team can keep a small fleet of helpers: named units (robots in a lab, porters in a kitchen) that run on
a fast, cheap model for data-heavy jobs with a clear goal, such as a code audit, a consistency sweep over a
document, tabulating logs, downloading files from given links, or counting across many files. They report facts
with locations and never interpret. The persona who sent one chooses a scrutiny level before sending it
(`trust`, `minimal`, `intensive`), rechecks the report at that level, records the recheck, and draws the
conclusion. Persona subagents ask for helpers in a `## Helper requests` section, and the main agent sends them.
On Claude Code the team gets its own helper agent (`.claude/agents/<team>-helper.md`, pinned to Haiku);
elsewhere a fast subagent is used, and without subagents the persona does the job itself.

The founding research group's robots (KLONK, PIP-7, SCOOP, COURIER-3, TALLY) are the worked example.

## Refreshing a team

*"Refresh the team"* compares the team's framework files with this skill's version, shows one screen of what
would be replaced or added, offers any new feature that needs your choice (since 0.2.0: helpers, for a team
that has none), and applies it on your yes. Personas, `house.md`, portraits and project memory are left alone.
A team built with 0.1.0 is offered helpers once; declining writes "Helpers: none" so it is not asked again.

## What is in the folder

| Path | What it is |
| --- | --- |
| `SKILL.md` | The casting conversation and where everything else is |
| `references/field.md` | The philosophy under every team; read when designing, explained only on request |
| `references/metaphors.md` | Group metaphors: the five-part form and six worked examples |
| `references/personas.md` | The form of a persona file and of `team.md`; the checks run before a proposal |
| `references/interview.md` | The deeper interview |
| `references/team-skill.md` | Writing, changing, refreshing and sharing a team skill |
| `references/portraits.md` | Portraits and the team page |
| `framework/` | `rules.md`, `convening.md`, `memory.md`: copied verbatim into every team skill; `helpers.md`: into teams with helpers |
| `templates/` | The team skill's `SKILL.md` and `house.md`; `helper-agent.md`, the helper agent for hosts with agent definitions |

## Provisional since 0.1.0

Three planning tickets were still open when 0.1.0 was written. It makes these working choices, which may
change when the tickets close:

- **Where things live** ([ticket](https://github.com/minaam-qamar/team-building-skill/blob/dev/planning/team-building/issues/12-where-a-team-lives-and-how-it-changes.md)):
  the project's memory home is `team-memory/<team-slug>/`.
- **Guardrails** ([ticket](https://github.com/minaam-qamar/team-building-skill/blob/dev/planning/team-building/issues/18-guardrails-in-practice.md)): a request for a persona
  of a real person gets a fictional character with the same working habits; a persona holds a point until a new
  reason arrives; a working persona stops before a short list of hard-to-undo steps.
- **Testing** ([ticket](https://github.com/minaam-qamar/team-building-skill/blob/dev/planning/team-building/issues/10-test-campaign-design.md)): one smoke run by a fresh agent (a short pair, and a full team
  written and convened for one meeting); its findings were folded in.

The name and how the underlying traditions are credited in public text are also undecided.

## Branches

- `main` holds only the skill and this README. Install from here.
- `dev` also holds the planning map, tickets, research, glossary and prototypes behind the skill. Work happens
  there; `main` is updated from it with `git checkout dev -- skills README.md`.

## Licence

[MIT](LICENSE), copyright Minaam Qamar. One added permission: the files in `framework/` and `templates/` are
copied into every team skill, and those copies need not carry the licence notice.

## Origin

The founding example is the [research group](https://github.com/minaam-qamar/research-group-skill) used on a
materials-science project. The idea behind it is in [docs/some_ideas.md](https://github.com/minaam-qamar/team-building-skill/blob/dev/docs/some_ideas.md).
