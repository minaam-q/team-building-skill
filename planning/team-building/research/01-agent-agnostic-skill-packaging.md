# Agent-agnostic skill packaging

Research for ticket [01](../issues/01-agent-agnostic-skill-packaging.md). Checked 2026-11-12 against the
primary sources listed at the end. Claims are tagged:

- **[V]** read in a first-party doc or spec during this session
- **[F]** first-party but not documentation (vendor forum, vendor blog, issue tracker)
- **[U]** unverified: third-party source, search snippet, or inference

## 1. Short answer

"An installable skill that any agent can use" means, today, **a directory named after the skill that contains a
`SKILL.md` with `name` and `description` YAML frontmatter, plus plain Markdown files beside it**, written to the
Agent Skills specification (agentskills.io). Every agent surveyed loads that format and says so in its own docs.

Three things are *not* standardized and must be handled by the skill or its installer:

1. **Where the directory goes.** The spec deliberately does not say. There are two de facto locations,
   `.agents/skills/` and `.claude/skills/`, and no single path is read by every agent.
2. **What tools the host offers.** Subagents and a structured ask-the-user tool exist in most hosts, but under a
   different name, with different limits, in each one. Chat surfaces may have neither.
3. **Packaging above the skill folder.** Plugin manifests and marketplaces are per vendor.

So the portable core is the skill folder. Everything the predecessor does with `Agent`, `AskUserQuestion`,
`.claude-plugin/plugin.json` and `install.sh` is a Claude Code convenience that needs a capability-neutral wording
and a fallback.

## 2. The common denominator: the Agent Skills format

Source: <https://agentskills.io/specification> [V], <https://agentskills.io/home> [V].

- A skill is a directory containing at minimum `SKILL.md`. Optional conventional subdirectories are `scripts/`,
  `references/`, `assets/`; any other files or directories are allowed.
- Frontmatter fields in the spec: `name` (required), `description` (required), `license`, `compatibility`,
  `metadata` (string-to-string map), `allowed-tools` (experimental). Nothing else is in the spec.
- `name`: 1 to 64 characters, lowercase `a-z`, digits and hyphens, no leading, trailing or doubled hyphen, and it
  **must match the parent directory name**.
- `description`: 1 to 1024 characters, says what the skill does and when to use it. This is the only trigger
  mechanism the spec defines.
- `compatibility`: up to 500 characters, free text for environment requirements. The spec's own example is
  `Designed for Claude Code (or similar products)`.
- Progressive disclosure: name and description at startup (about 100 tokens), the `SKILL.md` body on activation
  (under 5000 tokens recommended, under 500 lines), other files only when the instructions reference them.
- File references: relative paths from the skill root, **one level deep from `SKILL.md`**; avoid nested chains.
- Validation: `skills-ref validate ./my-skill` (reference library in the `agentskills/agentskills` repo).
- Origin: "originally developed by Anthropic, released as an open standard". Governance beyond "open to
  contributions" is not described on the pages read.

The client-implementation guide (<https://agentskills.io/client-implementation/adding-skills-support> [V]) is the
closest thing to a statement of what a skill author can expect from a compliant host:

- The spec "does not mandate where skill directories live". `.agents/skills/` (project and `~/`) "emerged as a
  widely-adopted convention for cross-client skill sharing"; some clients also scan `.claude/skills/` "for
  pragmatic compatibility".
- Activation is model-driven from the description, either by the model reading the file or through a dedicated
  tool. Hosts "should also" offer explicit user activation (`/skill-name` or `$skill-name`); the syntax is up to
  the host.
- Hosts are advised to validate leniently (warn and load when the name mismatches the directory), but skip a skill
  with no description.
- The host may strip frontmatter before the model sees the skill, so nothing the model needs may live only in
  frontmatter.
- Relative paths are resolved against the skill directory, which the host tells the model (catalog `location` or
  activation result).
- Running a skill in a subagent is "an advanced pattern only supported by some clients".

The client showcase page (<https://agentskills.io/clients>) renders its list client-side and could not be read, so
the adopter list below comes from each vendor's own docs instead.

## 3. Agent by agent

| Agent | Skill format | Where it loads skills from | Discovery and trigger | Subagents | Structured ask-user |
| --- | --- | --- | --- | --- | --- |
| **Claude Code** (Anthropic) | Agent Skills; "follows the Agent Skills open standard" and extends it with about 14 extra frontmatter fields (`context: fork`, `agent`, `disable-model-invocation`, `model`, `hooks`, ...) [V] | `~/.claude/skills/`, `.claude/skills/` (also nested and parent dirs), plugin `skills/`, managed settings, claude.ai account sync. `.agents/skills/` is **not** listed [V] | Description-matched by the model through the `Skill` tool; user types `/skill-name` [V] | Yes. `Agent` tool (renamed from `Task` in v2.1.63), built-in `general-purpose`, parallel, up to 20 concurrent, nesting to 3 levels [V] | Yes. `AskUserQuestion`, multiple choice plus free text. **Not available inside subagents** [V] |
| **claude.ai / Claude desktop** (chat) | Same folder, uploaded as a ZIP [V] | Customize > Skills > Upload; requires code execution enabled; all plans [V] | Model decides from the description; no manual invocation [V] | Not documented for chat [U] | Not documented for chat [U] |
| **Codex CLI / IDE / ChatGPT** (OpenAI) | Agent Skills; "build on the open agent skills standard". Optional `agents/openai.yaml` for UI metadata and invocation policy [V] | `.agents/skills/` from CWD up to repo root, `~/.agents/skills/`, `/etc/codex/skills`, bundled. Symlinked skill folders are followed [V] | Implicit by description; explicit with `$name` or `/skills` (CLI/IDE), `@` in ChatGPT [V] | Yes, on by default. Spawns when the user asks or when "skill instructions request it". Custom agents are TOML in `.codex/agents/` [V]. Tool names (`spawn_agent` etc.) [U] | A `request_user_input` tool exists but is reported as restricted to Plan mode, with a feature flag for Default mode [F, community forum]. Not found in official docs: **unverified** |
| **Gemini CLI** (Google) | Agent Skills; "based on the Agent Skills open standard" [V] | `.gemini/skills/` or `.agents/skills/` (workspace), `~/.gemini/skills/` or `~/.agents/skills/` (user), extensions, built-in. `.agents` wins within a tier [V] | Description in the system prompt; model calls `activate_skill`; **user must confirm each activation**. `gemini skills install <git-url>`, `/skills link <path>` [V] | Yes, on by default (flag sits under `experimental`). Built-in `generalist`; subagents appear as tools named after the agent; `@name` forces one. **Subagents cannot call subagents.** Parallelism not stated [V] | Yes. `ask_user`: 1 to 4 questions, types `choice`, `text`, `yesno`, 2 to 4 options [V] |
| **Cursor** (editor, CLI, cloud) | Agent Skills; cites agentskills.io. Extra fields `paths`, `disable-model-invocation`, `icon`, `color` [V] | `.agents/skills/`, `.cursor/skills/`, `~/.agents/skills/`, `~/.cursor/skills/`, plus compatibility `.claude/skills/`, `.codex/skills/` and their `~/` forms [V] | Description-matched; `/` menu attaches a skill to one message [V] | Yes. `Task` tool calls, parallel, foreground or background, one level of nesting. Built-ins are only Explore and Browser; **no documented general-purpose subagent**, custom ones are files in `.cursor/agents/` (also reads `.claude/agents/`) [V] | An `AskQuestion` tool exists but availability varies by mode and model (reported missing in some modes, switched off for some models) [F, vendor forum incl. staff reply]. No docs page found |
| **opencode** | Agent Skills-shaped; ID comes from the **path**, not `name`; spec limits recommended but not enforced [V] | `.opencode/skills/`, `~/.config/opencode/skills/`, plus compatibility `.claude/skills/`, `.agents/skills/` and their `~/` forms; extra sources in `opencode.json` [V] | Listed by description; model calls the `skill` tool; user mentions `@skill-id`; permission `allow` / `ask` / `deny` [V] | Yes. `subagent` tool, built-in `general` and `explore`, always background, `general` cannot nest [V] | Yes. `question` tool, "requires a client that supports the interactive form" [V] |
| **GitHub Copilot** (CLI, cloud agent, app) | Agent Skills; "The Agent Skills specification is an open standard" [V] | Project `.github/skills/`, `.claude/skills/`, `.agents/skills/`; personal `~/.copilot/skills/` or `~/.agents/skills/`. "Supported locations depend on the surface" [V] | Loaded "when relevant"; CLI has `/skills` and `copilot skill`; `gh skill install` (preview) [V] | CLI: yes. `task` tool ("Run subagents"), `/fleet` for parallel execution [V] | CLI: yes. `ask_user` tool; `--no-ask-user` disables it, needed for non-interactive runs [V] |
| **VS Code agent mode** (Copilot, plus Claude and Codex harnesses) | Agent Skills; cites agentskills.io. Supports `argument-hint`, `user-invocable`, `disable-model-invocation`, experimental `context: fork` [V] | Per harness: `.github/skills/` + `~/.copilot/skills/`, `.claude/skills/` + `~/.claude/skills/`, `.agents/skills/` + `~/.agents/skills/` [V] | Description-matched; `/` menu [V] | Not checked | Not checked |

Other hosts were not read first-hand. The `vercel-labs/skills` installer lists 79 agents with skill paths
(Windsurf `.windsurf/skills/`, Roo `.roo/skills/`, Goose `.goose/skills/`, Kiro `.kiro/skills/`, Junie
`.junie/skills/`; Cline, Zed, Amp, Antigravity on `.agents/skills/`). That is a third-party table [U] but it is
maintained code that has to work, and it agrees with every vendor doc checked above.

One product note: the Gemini CLI docs carry a banner that for unpaid-tier and Google One users "Gemini CLI was
replaced by Antigravity CLI on June 18th, 2026" [V]. Antigravity's own skill docs were not read [U].

### Where to install: no single path

| Path | Read by |
| --- | --- |
| `.agents/skills/` and `~/.agents/skills/` | Codex, Gemini CLI, Cursor, opencode, Copilot, VS Code. **Not** Claude Code. |
| `.claude/skills/` and `~/.claude/skills/` | Claude Code, Cursor, opencode, Copilot (project level), VS Code. **Not** Codex, **not** Gemini CLI. |

Two locations cover all seven. A symlink from one to the other is the cheap way to keep one copy; Codex documents
that it follows symlinked skill folders [V], and the predecessor already relies on Claude Code doing so. Symlink
handling in the other hosts was not verified [U].

### Installers and packaging above the folder

- **Plain copy or `git clone`** into a skills directory works everywhere and is the only universal method.
- **`npx skills add <repo>`** (vercel-labs/skills, MIT, needs Node 22.20+): detects installed agents and writes to
  each one's path; project default `.agents/skills/<name>`, `-g` for global; telemetry on by default [V for the
  README; third-party tool].
- **`gh skill install <repo> <skill>`**: GitHub CLI, "in preview and subject to change" [V].
- **`gemini skills install <git-url>`**, **Codex `$skill-installer`**: per-vendor [V].
- **claude.ai**: ZIP upload, folder name must match the skill name [V].
- **Plugins** are per vendor. Claude Code: `.claude-plugin/plugin.json` (optional; `name` is the only required
  key), skills under `skills/`, namespaced as `plugin:skill` [V]. Cursor: needs `.cursor-plugin/marketplace.json`
  to import from GitHub; "skills aren't imported on their own" [V]. Codex/ChatGPT: plugins exist and can contain
  skills; manifest layout not read [U]. The same repo can carry several manifests next to one `skills/` directory.

## 4. What the predecessor relies on, and how portable each piece is

Read from `/home/users/qamarmt2/workstation_apna_ace/research-group-skill`.

| Feature used | Where | Portable? | Notes |
| --- | --- | --- | --- |
| `SKILL.md` with `name` + `description`, name equals directory | `skills/research-group/SKILL.md` | **Yes** | Spec-clean already. Body is 66 lines. |
| Sibling Markdown loaded on demand (`personas/`, `workflows/`, `templates/`) via relative links | SKILL.md | **Yes** | Core of the spec. The directory names are non-conventional but allowed. |
| Reference chain two levels deep (SKILL.md -> workflow -> `../templates/x`) | `workflows/project-setup.md` | **Mostly** | Spec says keep references one level deep from SKILL.md. Works where the model knows the skill directory; fragile otherwise. List every file the workflows need in SKILL.md. |
| Trigger by description ("use when the user names any of them...") and `/research-group` | frontmatter, README | **Description yes; slash syntax no** | Explicit invocation is `/name`, `$name`, `@name` or absent (claude.ai) depending on host. |
| `AskUserQuestion` for the quick-vs-project triage | SKILL.md step 2 | **No (name), partly (capability)** | Named differently everywhere: `AskUserQuestion`, `ask_user` (Gemini, Copilot), `question` (opencode), `request_user_input` (Codex, mode-restricted), `AskQuestion` (Cursor, unreliable). Missing in subagents, headless runs and chat. |
| `Agent` tool: "spawn four subagents in one message", per-researcher model `opus` / `sonnet` / `haiku` | `workflows/ground-work.md`, `group-meeting.md` | **No (name, model names), partly (capability)** | Tool is `Agent`, `Task`, `task`, `subagent`, an agent-named tool, or unnamed. Cursor documents no general-purpose subagent. Parallelism is documented for Claude Code, Cursor, Copilot `/fleet`, Codex; not stated for Gemini. Model aliases are Anthropic-only. Chat surfaces: none documented. |
| Subagents told "do not read the other three directories" for independence | `ground-work.md` | **Capability-dependent** | Real isolation needs separate contexts. Without subagents the independence is only simulated in one context. |
| File writes into the project (`research_group/...`), `git rev-parse` | all project-mode workflows | **Coding agents yes; chat no** | claude.ai skills run inside a code-execution sandbox, not the user's project. |
| `.claude-plugin/plugin.json` | repo root | **No** | Claude Code only. Harmless elsewhere; other hosts need their own manifest or none. |
| `install.sh` symlinking into `~/.claude/skills/` | repo root | **No** | Hard-codes one host path. Bash only (no Windows). Codex and Gemini would never see it. |
| Mentions of `CLAUDE.md` / `AGENTS.md` | `project-setup.md` | **Yes** | Already names both. |

## 5. What the skill may assume / what must degrade gracefully

### May assume (true in every host surveyed)

- The host finds a directory `<name>/SKILL.md`, reads `name` and `description`, and shows the description to the
  model before anything else. The description is the trigger; write it with the words users will say.
- When the skill activates, the model gets the Markdown body and can read other files in the skill directory by
  relative path.
- The model can hold a conversation with the user in plain text. **A plain-text question is the only universal
  ask-the-user mechanism.**
- Frontmatter limited to the six spec fields is accepted everywhere. Claude Code states that unknown fields are a
  hard error on some non-Claude-Code Anthropic surfaces, so extra fields are not free.

### Should not assume, and how to degrade

| Do not assume | Write it as | Fallback |
| --- | --- | --- |
| A tool called `AskUserQuestion` | "Ask the user to choose. If you have a structured question tool, use it." | Ask in chat with a short numbered list and wait for the reply. |
| A tool called `Agent`, or any subagents | "If you can delegate to subagents, give each persona its own subagent, in parallel if possible." | Play the personas one after another in the main context, writing each one's output before reading the next, and tell the user that independence is reduced. |
| A general-purpose subagent type | Describe the brief to hand over, not the agent type. | Same sequential fallback (matters for Cursor). |
| Subagents that can question the user or spawn further subagents | Keep all user questions and all spawning in the main agent. | None needed if the design is flat: one level, questions only at the top. |
| Model names (`opus`, `sonnet`, `haiku`) | "a stronger model for the chair, a cheaper one for routine roles, if you can choose". | Use whatever model is running. |
| An install location | Ship the folder; document `.agents/skills/` and `.claude/skills/` as the two targets. | Installer copies or links into both; or point users to `npx skills add` / `gh skill install`. |
| A slash command | "Invoke the skill by name in your agent's usual way." | Rely on the description. |
| A writable project directory or a shell | "If you can write files, save the team to ...". | In chat-only hosts, keep the team in the conversation and offer it as text to copy. |
| Plugin manifests | Treat as optional per-host wrappers around the same `skills/` folder. | Plain folder install. |
| Reference chains deeper than one level | Link every workflow, persona template and asset from `SKILL.md`. | none |
| The model seeing frontmatter | Put nothing behavioural in `metadata` or `compatibility`. | none |
| Skill instructions surviving a long session | Put the essential rules at the top of `SKILL.md`; keep durable state in files the skill writes. | Re-invoke the skill. |

### Design consequences for the persona-team skill

1. One skill directory, spec-only frontmatter, `name` equal to the directory, body under about 500 lines.
2. Capabilities are named by what they do, with an explicit "if you do not have this" branch beside each use.
3. The quick, conversational mode (casting a team and getting opinions in chat) must work with **no tools at
   all**, since that is all claude.ai and other chat hosts offer. File-backed project mode and parallel
   independent reviews are enhancements for coding agents.
4. Flat orchestration only: the main agent asks every question and spawns every helper.
5. Distribution: a public Git repo with the skill under `skills/<name>/`, optional vendor manifests beside it, and
   an install note covering both paths. A custom `install.sh` is optional, not the primary route.

## 6. Not verified

- Codex structured ask-user (`request_user_input`): existence and mode restriction come from OpenAI community
  forum posts and third-party write-ups, not official docs.
- Cursor `AskQuestion`: vendor forum only; no documentation page found.
- Codex subagent tool names; Gemini CLI parallel subagents; Codex/ChatGPT plugin manifest layout.
- Whether claude.ai chat, ChatGPT chat or Copilot cloud agent expose subagents or a question tool to a skill.
- Symlinked skill directories in hosts other than Codex and Claude Code.
- The agentskills.io client list (page is client-rendered) and the standard's governance.
- Hosts not read first-hand: Windsurf, Cline, Roo, Goose, Amp, Kiro, Junie, Zed, Antigravity (paths from the
  vercel-labs table only).
- OpenAI's Codex docs now redirect from `developers.openai.com/codex/...` to `learn.chatgpt.com/docs/...`; the
  redirect was followed and the content is consistent, but the new host was not independently confirmed.

## Sources

Spec
- <https://agentskills.io/specification>
- <https://agentskills.io/home>
- <https://agentskills.io/client-implementation/adding-skills-support>
- <https://github.com/agentskills/agentskills>

Anthropic
- <https://code.claude.com/docs/en/skills>
- <https://code.claude.com/docs/en/sub-agents>
- <https://code.claude.com/docs/en/tools-reference>
- <https://code.claude.com/docs/en/plugins-reference>
- <https://support.claude.com/en/articles/12512180-using-skills-in-claude>

OpenAI
- <https://developers.openai.com/codex/skills> (redirects to <https://learn.chatgpt.com/docs/build-skills>)
- <https://developers.openai.com/codex/subagents> (redirects to <https://learn.chatgpt.com/docs/agent-configuration/subagents.md>)
- <https://learn.chatgpt.com/docs/build-plugins>
- <https://community.openai.com/t/feature-request-allow-request-user-input-in-default-mode/1395392> [F]

Google
- <https://geminicli.com/docs/cli/skills/>
- <https://geminicli.com/docs/core/subagents/>
- <https://geminicli.com/docs/tools/ask-user/>

Cursor
- <https://cursor.com/docs/context/skills>
- <https://cursor.com/docs/context/subagents>
- <https://forum.cursor.com/t/cursor-2-4-clarification-questions-from-the-agent/149406> [F]
- <https://forum.cursor.com/t/how-can-i-use-clarifying-questions-with-my-skill/152102> [F]

opencode
- <https://opencode.ai/docs/skills/>
- <https://opencode.ai/docs/agents/>
- <https://opencode.ai/docs/tools/>

GitHub / Microsoft
- <https://docs.github.com/en/copilot/concepts/agents/about-agent-skills>
- <https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference>
- <https://docs.github.com/en/copilot/concepts/agents/copilot-cli/fleet>
- <https://cli.github.com/manual/gh_skill>
- <https://code.visualstudio.com/docs/copilot/customization/agent-skills>

Third-party installer
- <https://github.com/vercel-labs/skills> (README and `docs/agents.md`)
