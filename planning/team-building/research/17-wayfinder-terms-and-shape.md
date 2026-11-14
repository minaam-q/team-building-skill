# Wayfinder's terms and shape

Research for ticket [17](../issues/17-wayfinder-terms-and-shape.md). Checked 2026-11-14. Packaging across hosts in
general is in [01](01-agent-agnostic-skill-packaging.md) and is not repeated here.

Sources: **L/** = the local copy, `~/.claude/plugins/cache/mattpocock/mattpocock-skills/1.2.3/`. **U/** = upstream
`main`, `https://github.com/mattpocock/skills/blob/main/`, read 2026-11-14 (last push 2026-11-13 per
`https://api.github.com/repos/mattpocock/skills`). **Ir/** = `/home/users/qamarmt2/workstation_apna_ace/ir/`.

**The local copy is two minor versions behind**: 1.2.3 (`L/package.json`) against 1.3.2 (`U/package.json`,
`U/CHANGELOG.md`). Differences that matter here are flagged "1.3.2" below.

## 1. Licence

- MIT, "Copyright (c) 2026 Matt Pocock" (`L/LICENSE`; `U/LICENSE` is byte-identical; `"license": "MIT"` in
  `L/package.json` and `L/.claude-plugin/plugin.json`).
- It grants use, copy, modify, merge, publish, distribute and sublicense, on one condition: "The above copyright
  notice and this permission notice shall be included in all copies or substantial portions of the Software."
- **Depend on it** (name it, tell the user to install it, read the files it writes): nothing is copied, so the
  condition does not apply.
- **Bundle it** (ship `wayfinder/SKILL.md` or the tracker templates): allowed; the MIT text with the copyright line
  must travel with the copies.
- **Adapt its text**: allowed; the same notice is required if a substantial portion survives. MIT does not require
  changes to be marked.
- (A reading of the licence text, not legal advice.) No trademark or naming clause. Not found in L/ or U/: a `NOTICE` file, a policy on using the author's name, or any
  statement about third-party skills that depend on this set.
- The README invites adaptation: "Hack around with them. Make them your own." (`L/README.md` l.19).
- Upstream's own precedent for copied-in text is a `CREDITS.md` beside the skill. `U/skills/engineering/pr/CREDITS.md`
  credits Dex Horthy's `show-me`: the content is "copied in rather than pointed at" because "a hard dependency would
  break standalone installs", and "this file is the attribution a dependency would otherwise have carried."
- Upstream's scope rules point persona support away from the repo. `U/SCOPE.md`: "a config option, a
  harness-specific branch, or a tweak to suit one person's workflow" belongs "in your own `CLAUDE.md` / `AGENTS.md`,
  or in a fork"; new-skill proposals are closed. `U/CONTRIBUTING.md`: "Pull requests are open to collaborators only."

## 2. Installing wayfinder outside Claude Code

Wayfinder is one skill in a set of 25 (1.2.3, `L/.claude-plugin/plugin.json`) or 27 (1.3.2,
`U/.agents/install-block.md`). The plugin routes install the whole set; skills.sh lets the user pick.

**What wayfinder needs beside itself** (`L/skills/engineering/wayfinder/SKILL.md`):

| Skill | Bucket | Used for | Invocation |
|---|---|---|---|
| `grilling` | `productivity/` | naming the destination; every grilling ticket | model-invoked |
| `domain-modeling` | `engineering/` | same two places, always paired with grilling | model-invoked |
| `research` | `engineering/` | research tickets, run as a subagent | model-invoked |
| `prototype` | `engineering/` | prototype tickets | model-invoked |
| `setup-matt-pocock-skills` | `engineering/` | writes `docs/agents/issue-tracker.md` and the `## Agent skills` block | user-invoked |

Without setup wayfinder falls back: "If no tracker has been provided, default to the local-markdown tracker" (l.25).

**Wayfinder is user-invoked, so no skill can start it.** Frontmatter `disable-model-invocation: true`, and
`policy.allow_implicit_invocation: false` in `L/skills/engineering/wayfinder/agents/openai.yaml`.
`L/.agents/invocation.md`: "nothing but the human can fire it — no other skill can." A team skill can read and write
the same tracker files, but cannot call `/wayfinder`.

| Host | 1.2.3 (`L/README.md`, `L/.agents/install-block.md`) | 1.3.2 (`U/README.md`, `U/.agents/install-block.md`) |
|---|---|---|
| Claude Code | `claude plugins install mattpocock-skills` | `claude plugin install mattpocock-skills@claude-plugins-official`; self-updating |
| Codex | `npx skills@latest add mattpocock/skills`; native plugin deferred (`L/.agents/adr/0002-…`) | `codex plugin marketplace add mattpocock/skills` then `codex plugin add mattpocock-skills@mattpocock`; updates at startup |
| Copilot CLI / VS Code | skills.sh | marketplace add + install, plus a one-time `autoUpdate` setting; VS Code "Chat: Install Plugin From Source". The settings entry "follows the documented schema, but nobody has run it yet" |
| Gemini CLI | skills.sh | `gemini skills install https://github.com/mattpocock/skills.git --path skills/engineering`, and again with `skills/productivity`; manual updates. Both are needed for wayfinder, because `grilling` is in `productivity/` |
| Cursor, opencode, Devin, Windsurf, Amp, Pi | skills.sh | `npx skills@latest add mattpocock/skills -a <agent>`; "Each agent in the comment was run on 2026-10-07"; manual updates |
| Auggie, Droid, Qwen, Goose | not mentioned | "Nobody has run [them] against this repo" |

- One skill at a time: `npx skills@latest add mattpocock/skills --skill=<name>` (`L/.agents/install-block.md`); the
  companions in the table above then have to be added the same way. Plugin and skills.sh are exclusive: "Installing
  both leaves the user with every skill twice."
- Under the Claude Code plugin the skills are namespaced (`mattpocock-skills:grilling`, seen in this session's skill
  listing); under skills.sh they are plain files with bare names.

Changes after 1.2.3 that a dependent skill would meet (`U/CHANGELOG.md`):

- 1.3.0: the glossary file is renamed `CONTEXT.md` → `GLOSSARY.md`; "the skills only look for `GLOSSARY.md` … going
  forward." Ir still uses `CONTEXT.md` (`Ir/CLAUDE.md`). Cross-skill calls are reworded from `/grilling` prose to
  "Call the Skill tool with "grilling"" (#878).
- 1.3.2: "`wayfinder:` labels are the only labels a map and its tickets carry, never a triage label like
  `ready-for-agent`" (`U/skills/engineering/wayfinder/SKILL.md` l.65; #1181). The GitHub template is reorganised
  (#1228, #1188); the changelog says to re-run `/setup-matt-pocock-skills` "to refresh `docs/agents/`".
- The **local-markdown** "Wayfinding operations" section is the same in 1.2.3 and 1.3.2 apart from punctuation
  (diff of `L/…/issue-tracker-local.md` against `U/…/issue-tracker-local.md`).

## 3. What a persona-as-assignee touches

Wayfinder's text has one ownership concept, the **claim**, meaning "a session is working this now". It has no notion
of a ticket owned ahead of time by someone not yet working it.

### 3a. The claim

- "A session **claims** a ticket by assigning it to the dev driving the map, **first**, before any work, so
  concurrent sessions skip it. That assignee _is_ the claim: an open, unassigned ticket is unclaimed." In work mode:
  "**Claim it**: assign it to yourself before any work." (`L/…/wayfinder/SKILL.md`; same in U/.) The frontier is
  "the open, unblocked, unclaimed children".

| Tracker | Claim operation | Frontier filter | Source |
|---|---|---|---|
| GitHub | `gh issue edit <n> --add-assignee @me` | drops tickets with an open blocker "or an assignee" | `L/…/setup-matt-pocock-skills/issue-tracker-github.md` |
| GitLab | `glab issue update <n> --assignee @me` | same | `L/…/issue-tracker-gitlab.md` |
| Local markdown | "set `Status: claimed` and save" | "open, unblocked, and unclaimed; first by number wins" | `L/…/issue-tracker-local.md` |
| Linear, Jira, other | none shipped; setup records the user's description "as freeform prose" | none shipped | `L/…/setup-matt-pocock-skills/SKILL.md` Section A |

Not found: a Linear template. `L/README.md` l.78 says "GitHub, Linear, or local files"; the setup skill and
`U/README.md` say GitHub, GitLab, local, or anything else described in prose. Backlog.md was refused as a first-class
tracker (`https://github.com/mattpocock/skills/issues/902`, `L/.out-of-scope/mainstream-issue-trackers-only.md`).

Consequences for a persona owner:
- **Local markdown: expressible with no change.** The claim is the `Status:` line and carries no identity. An extra
  `Persona:` line is read by no wayfinding operation, so it neither claims the ticket nor hides it from the frontier.
- **GitHub and GitLab: the native assignee is taken.** It is the claim, it is set with `@me` (the account running
  the session), and any assignee removes the ticket from the frontier. Putting a persona there would need wayfinder's
  text and the template changed. (Not verified here: whether either tracker can assign a non-account name at all.)
- **GitHub and GitLab: a body line is expressible with no change.** The templates already use body conventions
  (`Part of #<map>`, `Blocked by: #<n>` "at the top of the child body"). No query reads a `Persona:` line beside them.
- **GitHub and GitLab: a label works mechanically but contradicts 1.3.2's wording.** The frontier query ignores
  labels, but 1.3.2 says `wayfinder:` labels "are the only labels a map and its tickets carry". 1.2.3 does not.
- **"Assigned to the user" has no distinct state on GitHub/GitLab.** The driving dev *is* the user's account, so
  such a ticket reads as claimed and leaves the frontier.
- **The claim's lack of identity is an open upstream problem.** Issue #476 (open, `ready-for-human`): "claim
  protocol fails under concurrent sessions"; a comment reports the same on local markdown ("the claim carries no
  identity"). #1009 and #968 ("an abandoned claim is never released") were closed into it. All at
  `https://github.com/mattpocock/skills/issues/<n>`.

### 3b. Choosing a ticket

- "If the user named one, use it. Otherwise take the first frontier ticket in order." (Work through the map, step 2.)
- "Work the next ticket owned by X" is not in the text. Without changing wayfinder it is reachable two ways: the user
  names the ticket, or the map's **Notes** carry the rule. Notes hold "domain; skills every session should consult;
  standing preferences for this effort", and step 3 says to "invoke the skills the `## Notes` block names". Notes
  are the one extension point wayfinder gives.
- Notes are written by the agent. `L/docs/engineering/wayfinder.md` records an agent writing an override into its
  own Notes and later reading it "as its own licence".

### 3c. HITL and AFK

- "Every ticket is either **HITL** — human in the loop, worked _with_ a human who speaks for themselves — or
  **AFK**, driven by the agent alone. A HITL ticket only resolves through that live exchange; the agent never stands
  in for the human's side of it (a grilling agent that answers its own questions has broken this)."

| Ticket type | Persona as owner | Needs wayfinder changed? |
|---|---|---|
| research (AFK) | fits; the persona is the agent's voice | No. Wayfinder says a subagent runs the research skill; it does not say what the brief contains, so a persona brief is not excluded |
| task, AFK | fits | No |
| task, HITL | this is "hands the human a precise checklist", i.e. the user is the doer | No; this is already the "assigned to the user" case |
| grilling (HITL) | a persona may ask; it may not answer for the user. Personas settling the question among themselves is the stated violation | Yes, if personas are to resolve it without the user |
| prototype (HITL) | a persona may build; the choice between variants is the user's (`L/docs/engineering/wayfinder.md`, "the selection is yours to make") | Yes, if a persona is to choose |

### 3d. One ticket per session, resolution, and the tracker doc

- "**never resolve more than one ticket per session** — with the exception of research tickets." One persona on one
  ticket fits. A convened meeting that closes several non-research tickets in one session does not, unless the
  map's Notes override it. Charting "hand-resolves nothing" (Chart the map, step 6).
- Resolve is "post the answer as a **resolution comment**, **close** the issue, and **append a context pointer**".
  Who answered is not a field; a persona can sign the answer text. No change needed.
- `docs/agents/issue-tracker.md` is the project's file: setup says the user "can edit `docs/agents/*.md` directly
  later" (`L/…/setup-matt-pocock-skills/SKILL.md` step 5). A persona line added to its "Wayfinding operations" is
  therefore not a change to wayfinder's text.
- Not found: whether re-running setup preserves such edits. The skill protects user edits around the `## Agent
  skills` block only, and the 1.3.2 changelog tells users to re-run setup to refresh the docs files.

## 4. How the Ir project expressed persona ownership

Read: `Ir/docs/agents/issue-tracker.md`; in `Ir/planning/ir-brittleness-paper/`, `map.md`, tickets 63, 71, 77, 81,
82, and the header lines of the rest by `grep`.

- Tracker: the stock local-markdown template with `.scratch/` replaced by `planning/`; no mention of personas.
- 26 of 89 tickets carry a `Persona:` line, placed after `Type:`. Typical form:
  `Persona: Arje (owner)`; with a second voice, `Persona: Sibu (owner), Niko (set-up)`.
- The convention is defined in one place, the map's Notes: "the voice that did the work is named on every ticket's
  `Persona:` line" (`map.md` l.88). The Notes also list, per meeting, which persona got which ticket.
- All 26 are `task` (13) or `research` (13). None is `grilling` or `prototype`, so the HITL rule was never tested by a
  persona owner.
- Persona definitions sit beside the tracker docs (`Ir/docs/agents/arje.md`, `niko.md`, `sibu.md`, …). Wayfinder's
  files were not modified; the line was simply added.

**Friction visible in the tickets:**
1. **Owner and claim are different things and only one has a field.** Tickets 69, 70, 72 and 85 have an owner but
   `Status: needs-triage` or `ready-for-agent`: owned, not claimed. The frontier would offer them to any session.
2. **Identity leaked into `Status:`.** The claim carries no name, so names and dates went into the status as free
   text: `resolved (2026-11-13, Arje; Done-when met …)`, `resolved (2026-11-13, Sibu + Niko; …)`.
3. **Status vocabulary drifted.** The template defines `claimed`/`resolved`. In use: those two, `open`, the triage
   roles `needs-triage` / `ready-for-human` / `ready-for-agent` / `wontfix`, and `in-progress, STOPPED (…)` (81).
   Triage roles on wayfinder tickets are what 1.3.2 now forbids.
4. **The user as assignee has no line.** It appears as `Status: ready-for-human` (82), as status prose ("restart
   awaits the user", 81), and as a blockquote ("Assigned by the user the same day", 77, 81, 82).
5. **Authorisation and release are prose.** "the user authorised it via Mirela" (81); "parked 2026-11-13 by Mirela
   on the user's instruction; was: claimed" (11 tickets, 8 persona-owned, e.g. 63). The second is a claim release,
   for which wayfinder has no operation (upstream #968).
6. **Secondary roles are free-form.** `(review)`, `(set-up)`, `(hand check)`, `(geometry)`, a full clause ("Mirela
   commissioned it and reads the figures", 87), and one ticket (88) with two personas and no `(owner)`.
7. **Work preceded the ticket.** 71: "Recorded as a ticket afterwards so the map can index it."
8. **Personas executed, not only decided.** 57 of 89 tickets are `task`. Persona tickets add a `## Done when`
   section and launched runs (81). The map has an explicit execution override for one sub-effort (`map.md` l.417);
   the 2026-11-13 persona tickets are outside it.
9. **Answers went to two places:** `## Answer` (71, 63) as the template says, or dated persona-signed entries under
   `## Comments` (77, 82).

Not confirmable from the files: how many tickets each session resolved. The tickets record dates, not sessions.

## 5. Comparable map-and-ticket tools

`L/README.md` l.17 names "GSD, BMAD, and Spec-Kit" as approaches that "own the process". Checked from each
project's own repository:

| Tool | Unit of work | Owner field? | Source |
|---|---|---|---|
| **Backlog.md** (MIT) | Markdown task files in the repo, with dependencies and milestones; "Work on a single task per agent session" | Yes: `backlog task create "Feature" -a @sara`; list filter `-a <assignee>`. The assignee is a free string | `https://github.com/MrLesk/Backlog.md` (README, `CLI-INSTRUCTIONS.md`) |
| **beads** (MIT) | Issues in a dependency graph; `bd ready` lists unblocked work | Yes, and like wayfinder it is the claim: `bd update <id> --claim` "sets assignee + in_progress" | `https://github.com/gastownhall/beads` (redirect from `steveyegge/beads`), README |
| **Spec Kit** (MIT) | `tasks.md` checklist: `T001` ids, `[P]` parallel marker, `[US1]` story tag | None in the task template (no "assign" or "owner" in `templates/tasks-template.md`) | `https://github.com/github/spec-kit` |
| **BMAD Method** (README says MIT) | Agents and workflows, epics | Not found in the README; not read further | `https://github.com/bmad-code-org/BMAD-METHOD` |

- None of the four plans as a map of *decision* tickets with a fog section; Backlog.md, beads and Spec Kit track
  implementation tasks. Wayfinder's sibling `to-tickets` writes implementation tickets to the same tracker layout
  (`L/…/issue-tracker-local.md`, Conventions), so a persona line on wayfinder tickets meets those too.

## What this means for the decision

**Option A: require wayfinder.**

- For: MIT, no legal obstacle. Maintained (1.3.2, pushed 2026-11-13). Documented installs for Claude Code, Codex,
  Copilot, Gemini CLI and six skills.sh agents. Map, fog, frontier and blocking come for free.
- Against: it is one skill in a set of 27, with four companion skills and a setup step. It is user-invoked, so the
  team skill cannot start it. Its interface moves between minor versions (`CONTEXT.md` → `GLOSSARY.md`, label rule,
  template rewrites) and an installed copy may lag, as here. Upstream's scope rules rule out a persona feature
  upstream. Gemini CLI and skills.sh installs update by hand; four hosts are untested upstream.

**Option B: work with it when present.**

- For: on all three shipped trackers a persona owner fits without touching wayfinder: a `Persona:` line in the
  ticket file (local) or at the top of the issue body (GitHub, GitLab). Ir ran this way for 26 tickets. The map's
  Notes are a sanctioned place to name the team skill. Research and AFK task tickets accept a persona owner as
  written.
- Against: wayfinder does not read the line. Its frontier offers persona-owned tickets to any session, and on
  GitHub/GitLab a ticket assigned to the user looks claimed. Ir shows what happens with no further convention
  (section 4, items 1–6). Under the HITL rule personas alone cannot resolve grilling or prototype tickets, and a
  multi-ticket meeting breaks one-ticket-per-session unless Notes override it. A label-based owner contradicts
  1.3.2's wording.

**Option C: carry a small equivalent.**

- For: MIT allows copying or adapting the text with the notice kept; upstream's `CREDITS.md` shows the form. No
  install dependency and no version drift. Owner, claim and "the user" can each have a field. The local-markdown
  operations are six bullets and were stable from 1.2.3 to 1.3.2.
- Against: it duplicates a maintained skill and must be kept in step by hand if compatibility is wanted. It does
  not inherit GitHub/GitLab native blocking, sub-issues or frontier queries unless those templates are copied too.
  Users who already run wayfinder get two planning conventions unless the equivalent writes the same files.

Bearing on all three: the persona line and wayfinder's claim are independent on every shipped tracker; the HITL
and one-ticket rules are the only places where persona *behaviour*, not labelling, conflicts with wayfinder's text;
and the Ir friction came from missing conventions around the line, not from wayfinder rejecting it.
