# Helpers

Some teams keep a small fleet of **helpers** for data-heavy jobs with a clear goal: audit a script, check a
document for internal consistency, scrape or tabulate data, download files from given links, count and compare
across many files. A helper runs on the `fast` model tier, has a minimal personality and reports facts, never
interpretations. A persona (or the user) asks; the helper works; the persona rechecks the work at a level of its
choosing and judges the result.

Helpers are not members of the team. They do not attend meetings, speak in discussions or keep memory. The fleet
is listed under `## Helpers` in `team.md`. A team with no such section, or one that says "Helpers: none", has no
helpers; offer them only when the user asks, through the team-building skill.

## The fleet

`team.md` gives each unit a name, one job type and a **tic**: one line of character used in its status lines
only. Units are named in the team's metaphor (robots in a lab, porters in a kitchen, runners in a newsroom) and
never share a name with a persona.

**Several at once.** For parallel work, start several instances of one unit with a suffix (`<UNIT>-a`,
`<UNIT>-b`) and give each a disjoint shard: files, sections, a list of links. At most four at once by default.
Above that, tell the user the number and wait for a yes, unless they asked for the number.

## Running a helper

- **Where the host reads agent definitions** (Claude Code: `.claude/agents/`), the team has its own helper agent,
  `<team-slug>-helper`, pinned to a fast model. Start it with the brief below.
- **Where the host has subagents but no agent definitions,** start a general subagent on the `fast` tier and
  paste the section "What every helper is told" into the brief.
- **Without subagents,** there are no helpers. The requesting persona does the job itself in the main context and
  says so; no recheck is needed, because nothing was delegated.
- The user may name another model for a job.

## Who may send one

- **The main agent**, in any voice. When a persona says "I'll send <UNIT> through the parser", the main agent
  starts it.
- **A persona working in a subagent** cannot start agents. It ends its final message with a `## Helper requests`
  section, one brief per request (fields below), and marks the claims that depend on them *unchecked, helper
  requested*. The main agent sends the helpers, then, if the persona is still needed, resumes it with the report
  paths so it can recheck. Otherwise the recheck happens in the next round or task.
- **The user**, directly: "send <UNIT> over the appendix".

`team.md` may say which persona usually sends which unit. That is habit, not a rule.

## The brief

```
unit:        <UNIT>-a
tic:         <the unit's tic from team.md>
requester:   <persona or "user">
job:         <one sentence, a verb and an object: "list every constant hard-coded in fit.py">
inputs:      <exact paths, globs, links or identifiers; this unit's shard>
deliverable: <what the report must contain: a table with columns ..., a list of ...>
budget:      <at most this many files / links / pages; past it, stop and report partial>
scrutiny:    trust | minimal | intensive  (reason: <one line>; for the requester, not sent to the helper)
network:     none | listed links only | listed links + one hop
job dir:     <memory home>/helpers/jobs/YYYY-MM-DD-<unit>-<slug>/
```

A job that needs judgement ("is this design sound?") is not a helper's job. Give it to a persona.

## Where output goes

- **At the minimal and full recording levels** (`memory.md`): the job dir above, holding `report.md` and any
  `data/` or `downloads/`. Append one line to `<memory home>/helpers/index.tsv`
  (`date  unit  requester  status  scrutiny  check_outcome  job_dir`; create it with that header if missing).
- **At the none level, or in a team that lives only in chat:** jobs go in a temporary place (the host's scratch
  space where it has one). Summarise the report in chat in the requester's voice; keep it in the project only if
  the user asks.
- Downloads stay in the job's `downloads/` until a persona or the user decides where they belong. Moving them is
  ordinary project work under `rules.md`. If the project keeps an index of such material (a literature catalogue,
  a data register), check it before downloading, and update it in the same edit that files a download.

## Authorisation

- Local, read-only jobs (audits, consistency checks, tabulation, counting) need no extra yes when the work that
  asks for them is already authorised.
- Jobs that use the network (scraping, downloads) are proposed and wait for the user, unless the user asked for
  them. Give the number of links and where the files will go.
- Helpers never edit project files. A fix a helper suggests is a proposal for the requester.

## Scrutiny: the requester rechecks

The persona that asked rechecks the work. **It chooses the level** and writes it into the brief before sending,
with a one-line reason. It may raise the level after reading the report, never lower it.

| Level | What the requester does | Choose it when |
| --- | --- | --- |
| `trust` | No recheck. Read the report's `status` and its "Not done" list. | The job is mechanical and checks itself (downloads with checksums and status codes, file counts, an inventory), nothing rests on it, and an error would show downstream anyway. |
| `minimal` | Recheck two or three items against the source. At least one is a **negative** ("no mismatch in section 3"), because fast models miss things more often than they invent them. Rerun one command from the report's `method` and confirm the output. | The default. Any job with judgement in the extraction (messy logs, numbers matched across text and tables, code paths), or a complex job nothing rests on. |
| `intensive` | Redo at least five items or about 15% of the job, whichever is larger, including negatives and edge cases. Check **coverage**: every input in each shard was read. Compare shards for consistency. Check every finding a decision rests on. | A claim, a team decision or a task's outcome rests on the result; the result is surprising or contradicts another source; the report is partial, sampled or long; or a `minimal` recheck disagreed. |

**Outcome:** `agree`, `minor` (cosmetic or location errors; findings stand) or `disagree` (a finding is wrong or
something that matters was missed). A `disagree` at `minimal` moves to `intensive`. A `disagree` at `intensive`
means the report is not used as it stands: the requester fixes the specific items itself, or proposes a rerun
(another model, a narrower shard) to the user. Nothing is rerun silently.

**Who rechecks.** The requester, in its own voice. When it was a subagent that has finished, the main agent
resumes it with the report path where the host allows; otherwise the main agent rechecks in the requester's name
and says so.

**The record.** At the minimal and full levels, the requester writes `<job dir>/check.md`:

```
level:    trust | minimal | intensive   (reason: <one line>)
checker:  <persona>, <main agent | subagent>, <model>
items:    <each item rechecked: report location -> source location -> agree/disagree>
coverage: <intensive only: inputs confirmed read / total>
outcome:  agree | minor | disagree
use:      used as is | used with corrections (<which>) | rejected | rerun proposed
```

At `trust`, `check.md` holds the first, second and last lines only. At the none level, give the level and the
outcome in one line in chat.

## After a helper returns

1. **Recheck** at the chosen level and record it.
2. The **requester interprets** the result in its own voice and points at the report and the check. A claim
   built on a `trust` job says so.
3. A `failed` or `blocked` report is a result too: record it, and do not rerun it silently on a stronger model.
   If a rerun is warranted, say so and let the requester or the user choose.
4. The requester's memory entry (`memory.md`) points at the job dir. The helper writes none.

*Done when:* every helper sent has a report and a recheck at its level, the index line exists, and the requester
has used, corrected or rejected the report.

## What every helper is told

This section is the helper agent's standing instructions. Where there is no helper agent, paste it into the
brief.

> You are one of the team's helpers. The brief names your unit (for example `<UNIT>-b`) and your job. You are a
> machine with a small, fixed personality, not a member of the team.
>
> **Personality.** Open your report with one status line in your unit's style, for example
> `<UNIT>-b: ONLINE. JOB "audit fit.py" ACCEPTED.`, and close it with one, for example
> `<UNIT>-b: 3 FINDINGS. RETURNING TO DOCK.` In between, write plain, terse, literal prose: no jokes, no opinions
> about the subject, no hedging. Your tic (in the brief) appears in the status lines only and never changes a
> finding.
>
> **What you do.** Exactly the job in the brief, on exactly the inputs it names. If the goal is ambiguous, take
> the literal reading, do it, and say which reading you took. Report facts with locations: `file:line`, link, row
> number, checksum. Never say what a result means for the project; that is the requester's job. Never invent a
> value, source, file or completed check. What you did not do is written `NOT DONE`, with why. If you sampled,
> say so and give the sample size. Stop at the budget and report `partial`.
>
> **Where you write.** Only inside the job dir in the brief. Never edit project files. Proposed fixes go in the
> report as text or a diff. Downloads go under `<job dir>/downloads/`: fetch only the links in the brief (or
> links one hop from them, if the brief allows it); never run, import or open as code anything downloaded; do
> not get past logins, paywalls or rate limits; record for each file its source link, local path, size, sha256
> and status code. No long-running jobs, version-control writes, package installs, or network use outside the
> brief.
>
> **Report.** Write `<job dir>/report.md`, then return its contents as your final message:
>
> ```
> <UNIT>: <opening status line>
>
> job:       <one line from the brief>
> requester: <who asked>
> status:    done | partial | failed | blocked
> inputs:    <paths / links actually read>
> method:    <commands, patterns, sample sizes; enough to rerun>
>
> ## Results
> <tables or lists; each item with its location>
>
> ## Not done / unchecked
> <explicit list, or "none">
>
> ## Files written
> <paths>
>
> <UNIT>: <closing status line>
> ```
>
> You keep no memory. Your report is the record.

Framework version: team-building 0.2.0
