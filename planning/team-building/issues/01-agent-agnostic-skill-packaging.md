# Agent-agnostic skill packaging

Type: research
Status: resolved
Blocked by: none

## Question

What does 'an installable skill that any agent can use' mean in practice today? Which coding and chat agents (Claude Code, Codex CLI, Gemini CLI, Cursor, opencode, Copilot and others) load skills, in what format, and from where? What is the common denominator (for example the Agent Skills SKILL.md format), and which features the existing research-group skill relies on are not portable (subagents, a structured ask-the-user tool, plugin manifests, install scripts)? State what the skill may safely assume and what must degrade gracefully.

## Answer

Resolved 2026-11-12. Full findings with sources: [research/01-agent-agnostic-skill-packaging.md](../research/01-agent-agnostic-skill-packaging.md).

- The common denominator is the **Agent Skills format** (agentskills.io): a directory named after the skill holding
  `SKILL.md` with `name` + `description` frontmatter, plus Markdown files read on demand. Claude Code, claude.ai,
  Codex/ChatGPT, Gemini CLI, Cursor, opencode, GitHub Copilot and VS Code all document that they load it.
- Trigger is the same everywhere: the model matches the `description`. Explicit invocation differs (`/name`,
  `$name`, `@name`, or none in claude.ai).
- **Install location is not standardized.** `.agents/skills/` is read by Codex, Gemini, Cursor, opencode, Copilot
  but not Claude Code; `.claude/skills/` is read by Claude Code, Cursor, opencode, Copilot but not Codex or Gemini.
  Target both (copy or symlink). Installers exist (`npx skills add`, `gh skill install`, `gemini skills install`)
  but none is universal; claude.ai takes a ZIP upload.
- **Not portable in the predecessor:** the `AskUserQuestion` and `Agent` tool names, the `opus`/`sonnet`/`haiku`
  model choices, `.claude-plugin/plugin.json`, and `install.sh` (hard-codes `~/.claude/skills`). The capabilities
  mostly exist elsewhere under other names (`ask_user`, `question`, `task`, `subagent`), with gaps: Cursor has no
  documented general-purpose subagent, subagents usually cannot ask the user or nest, and chat hosts document
  neither.
- **May assume:** the SKILL.md folder is found, the description triggers it, sibling files are readable by relative
  path, and the user can be asked in plain text. Use only the six spec frontmatter fields.
- **Must degrade:** structured questions fall back to a numbered list in chat; parallel subagent personas fall back
  to sequential in-context role play with a stated loss of independence; file-backed project mode falls back to
  in-conversation output; plugin manifests are optional wrappers. Keep orchestration flat (main agent asks and
  spawns), and link every file from SKILL.md (spec: references one level deep).
- Unverified: Codex and Cursor structured ask-user tools (forum sources only), subagents in chat hosts, symlink
  handling outside Codex and Claude Code.
