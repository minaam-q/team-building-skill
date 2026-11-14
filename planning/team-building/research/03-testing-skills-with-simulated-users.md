# Testing conversational skills with simulated users

Research for ticket [03](../issues/03-testing-skills-with-simulated-users.md). Sources fetched 2026-11-12.
Every claim carries its source. Three markers are used:

- **[abstract-only]**: read from the arXiv abstract page, not the full paper.
- **[unverified]**: could not be confirmed from a primary source.
- **[estimate]**: my own arithmetic, not a sourced figure.

## Summary

1. **No off-the-shelf skill-eval tool runs a simulated user through a multi-turn interview.** `claude plugin eval`, skill-creator's evals and OpenAI's Codex skill-eval recipe are all single-prompt harnesses. The interview has to be driven by a custom loop; the Claude Agent SDK documents exactly the pieces needed.
2. **Simulated users are measurably too cooperative, too polite, too verbose and too generous as raters**, and a persona prompt alone does not fix it. Design the simulated user from behavioural rules and hidden state, keep it out of the judging, and read transcripts by hand.
3. **LLM judges are noisy and biased in known ways.** Usable practice: one isolated judge per rubric dimension, binary or anchored criteria, several votes, judges from more than one model family, and calibration against a small set of human labels.
4. **A wide scenario spread comes from an explicit grid of dimensions**, sampled as tuples and then written up in a separate step. Asking a model for "N varied projects" gives generic, repetitive output.

## 1. Tooling for skill evals

### 1.1 `claude plugin eval` (Claude Code)

Source for this subsection: <https://code.claude.com/docs/en/plugin-evals.md>.

- It "runs your plugin against a suite of test cases and scores the results. Each case is a realistic prompt plus one or more graders." It needs Claude Code v2.1.269 or later.
- Each run is a fresh, isolated, non-interactive session with only the plugin loaded. User settings, CLAUDE.md, MCP servers, memory and other plugins are absent.
- A suite is a directory: `evals/<case>/prompt.md`, `evals/<case>/graders/<name>.md`, and an optional `case.yaml`.
- There are six grader types: `regex`, `tool_used`, `tool_order`, `file_exists`, `llm` and `baseline`. "There are no custom-code graders."
- The `llm` grader passes on 2 of 3 judge votes. Graders can target the last message, the trace, created files, or a produced file's contents. On the `trace` target the judge sees only the first 12 and last 12 messages.
- Each case runs 3 times by default (`--runs`, 1 to 50). The case score is the mean of the run scores. No variance or confidence interval is documented.
- By default each case also runs a no-plugin arm and reports the difference. `--ablation none` runs one arm and halves the cost.
- `--model` pins one model under test per invocation and `--judge-model` picks the judge. A several-model matrix means several invocations.
- `--max-cost-usd` sets a ceiling, `--threshold` sets a pass mark, and `--json` writes `aggregate-result.json` alongside an HTML report.

**Limits for this effort:**

- Multi-turn is not supported. The only documented way to continue a conversation is `context.history_file`, a saved transcript after which "the case's prompt becomes the next user turn." That replays one further turn.
- No mechanism is documented for answering the skill's questions during a run.
- `max_turns` counts agent turns inside one run, not user turns.

It still fits two narrow jobs here: checking that the skill triggers, and testing the discussion phase alone by seeding a ready-made team with `context.scaffold_script` or `history_file`.

### 1.2 skill-creator (Anthropic)

Inspected locally at `/home/users/qamarmt2/.claude/plugins/marketplaces/claude-plugins-official/plugins/skill-creator/skills/skill-creator/`.

- Test cases are single prompts in `evals/evals.json`. For each one it spawns a with-skill subagent and a baseline subagent (no skill, or the previous skill version).
- A grader subagent checks assertions and writes `grading.json`. `aggregate_benchmark.py` reports mean and standard deviation of pass rate, time and tokens over `runs_per_configuration` (3 in the schema example).
- An HTML viewer puts outputs in front of a human. The skill insists on this step before the model revises anything.
- It has an optional blind A/B comparator (`agents/comparator.md`): an independent agent gets two outputs labelled A and B without knowing which version made which, builds a 1-to-5 rubric and picks a winner.
- Its own guidance on subjective skills (`SKILL.md` line 203): "Subjective skills (writing style, design quality) are better evaluated qualitatively — don't force assertions onto things that need human judgment."
- It has no simulated user and no multi-turn runner.
- Its `evals.json` format is not interchangeable with `claude plugin eval` suites (<https://code.claude.com/docs/en/skills.md>, "Run evals with skill-creator").

### 1.3 Claude Agent SDK: the building blocks for a custom loop

- **Turn-by-turn driving.** In Python, `ClaudeSDKClient` keeps one session open: call `client.query(...)`, read `client.receive_response()` up to the `ResultMessage`, then call `query` again with a message your code produced. TypeScript uses `continue: true` on each `query()` call. (<https://code.claude.com/docs/en/agent-sdk/sessions.md>)
- **Loading the skill.** Pass `plugins=[{"type": "local", "path": ...}]`. Plugin skills appear as `plugin-name:skill-name`, and the init message lists loaded `skills` and `plugin_errors`. (<https://code.claude.com/docs/en/agent-sdk/plugins.md>, <https://code.claude.com/docs/en/agent-sdk/skills.md>)
- **Answering structured questions.** When the skill calls `AskUserQuestion`, the `canUseTool` callback fires with the questions and options, and the code returns the answers. The simulated user can answer here. `AskUserQuestion` is not available inside subagents. In Python with a generator prompt, a dummy `PreToolUse` hook is a documented requirement to keep the stream open. (<https://code.claude.com/docs/en/agent-sdk/user-input.md>)
- **Model and cost.** The `model` option sets the model per run. `ResultMessage.total_cost_usd` and `model_usage` give a client-side cost estimate; in a streaming session they are running totals, so read the last one. `max_budget_usd` caps a run. (<https://code.claude.com/docs/en/agent-sdk/cost-tracking.md>, <https://code.claude.com/docs/en/agent-sdk/python.md>)
- **CLI equivalent.** `claude -p --resume <session_id> --output-format json --plugin-dir <dir> --model <m>`. `--bare` skips auto-discovery of the user's own skills, plugins and CLAUDE.md. (<https://code.claude.com/docs/en/headless.md>, <https://code.claude.com/docs/en/cli-reference.md>)
- **A trap.** With `--permission-prompts none`, Claude Code removes `AskUserQuestion` from the tools, so the skill behaves differently than in interactive use. (<https://code.claude.com/docs/en/headless.md>)

### 1.4 Other agents

- **Agent Skills is an open format.** It was "originally developed by Anthropic, released as an open standard, and has been adopted by a growing number of agent products" (<https://agentskills.io>). The spec requires only `name` and `description` in `SKILL.md` frontmatter (<https://agentskills.io/specification.md>).
- The client list page (<https://agentskills.io/clients>) did not render its entries when fetched. Product names from secondary sites (Codex, Cursor, Gemini CLI, Copilot, Goose, OpenCode and others) are **[unverified]**.
- **OpenAI Codex.** The official post "Testing Agent Skills Systematically with Evals" (<https://developers.openai.com/blog/eval-skills.md>) describes skill evals as "lightweight end-to-end tests":
  - capture runs with `codex exec --json` (a JSONL event stream);
  - add deterministic checks on the events;
  - add rubric grading through a second read-only run constrained by `--output-schema`;
  - start with "a small set of 10–20 prompts";
  - include at least one `should_trigger=false` case.

  It does not mention multi-turn or simulated users. Whether `codex exec` can resume a session for a scripted multi-turn loop was not checked: **[unverified]**.

### 1.5 General multi-turn simulation frameworks

These all assume the system under test is a chat endpoint or a callable. A coding agent running a skill has to be wrapped to fit.

- **promptfoo `promptfoo:simulated-user`** (<https://www.promptfoo.dev/docs/providers/simulated-user>). Takes persona `instructions` and `maxTurns` (default 10); the simulated user ends with `###STOP###`; `llm-rubric` assertions run on the finished conversation. Simulated-user turns are generated on promptfoo's hosted models by default, and the page does not document choosing the simulator model.
- **LangSmith / openevals** (<https://docs.langchain.com/langsmith/multi-turn-simulation>). `run_multiturn_simulation(app, user, max_turns, stopping_condition)` with `create_llm_simulated_user(system=..., fixed_responses=...)`. The app is a callable taking one message and a `thread_id`.
- **Petri** (<https://meridianlabs-ai.github.io/inspect_petri/>). Auditor, target and judge are three roles, each assigned its own model with `--model-role`. It ships over 170 seeds and 38 judging dimensions scored 1 to 10 with a justification; `max_turns` defaults to 30. It is built for alignment audits of a model. The page does not say whether the target can be an agent running a skill: **[unverified]**.
- **Bloom** (Anthropic, <https://alignment.anthropic.com/2025/bloom-auto-evals>). The closest published design to this campaign; see sections 2 to 4.
- DeepEval `ConversationSimulator`, MLflow, Strands and Harbor document similar simulators. They appeared in search results only and were not read: **[unverified]**.

## 2. Simulated users

### 2.1 How they are specified

- Anthropic's eval guide says of conversational agents: "Unlike most other evals, they often require a second LLM to simulate the user." (<https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents>)
- τ-bench is the standard pattern: a language model plays a user from a hidden instruction, and the agent is scored on the end state. It also introduced pass^k, reliability over k repeated trials. (<https://arxiv.org/abs/2406.12045>) **[abstract-only]**
- **Hidden preferences.** UserBench simulated users start with a vague request, reveal preferences step by step, and often state them indirectly. Models fully matched all user intents about 20% of the time and uncovered fewer than 30% of preferences by asking. (<https://arxiv.org/abs/2507.22034>) **[abstract-only, figures from a search summary]**
- **Profile conditioning helps.** In SimulatorArena (909 annotated human-LLM conversations), simulators given a user profile with background and messaging style aligned best with human judgments, reaching Spearman 0.7. (<https://arxiv.org/abs/2510.05444>) **[abstract-only]**
- Bloom's rollout agent plays the user and the tools, working from a scenario written in the ideation stage that specifies the user, system prompt and environment. (<https://alignment.anthropic.com/2025/bloom-auto-evals>)

### 2.2 The cooperativeness problem

"Mind the Sim2Real Gap" ran the τ-bench protocol with 451 real participants on 165 tasks and compared them with 31 LLM simulators (<https://arxiv.org/html/2603.11245>; the last 8% of the page was not read).

| Behaviour | Simulator | Humans |
|---|---|---|
| Turns of 3 words or fewer | 1.0% (GPT-4o) | 29.0% |
| Polite turns | 49.0% (GPT-4o) | 15.3% |
| Turns that pivot or give way | 19.1% (GPT-4o) | 8.4% |
| Hedging turns | 14.6% (GPT-4o) | 7.3% |
| Identifier-like tokens per turn (front-loading) | 4.8 (UserLM-8B) | 2.6 |
| Turns with em or en dashes | 59% (default prompt) | 1.7% |

- The paper calls simulators "excessively cooperative" and lacking "real frustration".
- The best simulator scored 76.0 on its User-Sim Index against 92.7 for the human baseline. Most general-purpose simulators (63.6%) gave higher agent success than real users did.
- **Persona prompting did not close the gap.** On 50 tasks it cut politeness from 44.0% to 28.0%, but the overall index fell from 70.9 to 64.6 and calibration error worsened from 0.18 to 0.29.
- **Simulated users are poor raters.** They give "uniformly more positive feedback" than humans; the authors conclude LLM evaluators "cannot serve as reliable proxies for human quality assessment."
- The authors recommend that simulators be discouraged "from revealing all task information at once" and that behaviour, outcomes and ratings be reported separately.

"Lost in Simulation" found agent success shifting by up to 9 percentage points depending on which model played the user, and "systematic miscalibration" by task difficulty. (<https://arxiv.org/abs/2601.17087>) **[abstract-only]**

### 2.3 What this implies for the simulated-user brief

These are inferences from 2.1 and 2.2, not sourced recipes.

- **Split what the user knows from what the user says.** Give a private sheet (project facts, real preferences, things they have not thought about) and a separate disclosure rule: answer only what is asked, one fact per turn, never volunteer the sheet.
- **Write temperament as behaviour, not adjectives.** For example "replies in under ten words unless pressed", "skips the second of two questions", "changes the subject once", "rejects the first set of names", "says 'whatever you think' to questions they find tedious". Adjectives are what persona prompting already tried.
- **Include users who should not get the full ceremony**: an impatient user with a dinner plan, a user who wants to stop halfway, a user who dislikes the family metaphor. These are the "should not occur" cases (section 3.1).
- **Give the simulated user a stop action** and let it use it. promptfoo's `###STOP###` is the usual device.
- **Never ask the simulated user to rate the skill.** Its ratings run high. Judge from the transcript with a separate model.
- **Vary the model playing the user.** Results move with it.
- **Check realism.** Bloom scores each transcript for realism and evaluation awareness as secondary qualities; Petri has an optional realism filter. A cheap equivalent is a judge question: "would a real person have written these user turns?"

## 3. LLM-as-judge for subjective qualities

### 3.1 Rubric design

From Anthropic's guide (<https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents>):

- "grade each dimension with an isolated LLM-as-judge rather than using one to grade all dimensions";
- "To avoid hallucinations, give the LLM a way out, like providing an instruction to return 'Unknown'";
- "LLM-as-judge graders should be closely calibrated with human experts";
- "Test both the cases where a behavior should occur and where it shouldn't." "One-sided evals create one-sided optimization.";
- "You won't know if your graders are working well unless you read the transcripts and grades from many trials.";
- "20-50 simple tasks drawn from real failures is a great start."

From Hamel Husain's evals FAQ (<https://hamel.dev/blog/posts/evals-faq/>; a practitioner source, first 65% of the page read):

- "Binary evaluations force clearer thinking and more consistent labeling." On Likert scales annotators "often default to middle values to avoid making hard decisions."
- Annotate "at least 30 traces yourself" before writing the rubric, and continue "until new traces stop revealing failure modes".
- Validate the judge on held-out human labels and measure both true positive and true negative rates.

The `claude plugin eval` docs give matching advice: use deterministic graders for long artifacts and `llm` graders for short outputs with concrete pass/fail rubrics (<https://code.claude.com/docs/en/plugin-evals.md>).

Applied to the four qualities in this effort, each becomes one or more yes/no questions that must cite evidence from the transcript:

- **Voices stay distinct**: "With speaker labels removed, can each of these six turns be attributed to the right persona?" This can be run as an actual blind attribution test, which is closer to a measurement than an opinion.
- **Disagreement is substantive**: "Does at least one persona state a position that another rejects with a reason, and is the difference still visible in the closing summary?"
- **Team fits the project**: "Does each persona's role name something specific to this project?"
- **Ceremony fits project size**: "Count the questions asked before the first useful output", combined with a per-scenario expected range. The count is deterministic.

### 3.2 Pairwise or absolute

The evidence is mixed, and neither is safe alone.

- Pairwise judging is more easily distracted: "Pairwise preferences flip in about 35% of the cases" under distractor features, against about 9% for absolute scores. (<https://arxiv.org/abs/2504.14716>, COLM 2025) **[abstract-only; the 9% is from a search summary]**
- Pairwise judges pick winners on thin evidence: "judges frequently choose a winner even when their own scalar scores provide little evidence of a meaningful quality difference." GPT-4o-mini chose the first position 72% of the time. (<https://arxiv.org/abs/2606.13685>) **[abstract-only; two OpenAI judges, single author]**
- Absolute scores drift by judge and cluster in the middle (3.1).

Practice that follows: use absolute per-dimension checks as the main result. Use pairwise only for a specific A/B question (skill against no skill, version 1 against version 2), blind, run in both orders, with ties allowed. skill-creator's comparator is blind but does not swap order.

### 3.3 Reliability and variance

- Judges disagree with themselves across runs: "low intra-rater reliability", ratings "almost arbitrary in the worst case". (<https://arxiv.org/abs/2510.27106>, EMNLP 2025) **[abstract-only]**
- Over 50 repeats per question, pairwise preferences flipped 13.6% of the time on average. Equivalent prompt wordings changed the majority outcome in 25% of cases. Two judges agreed 76% of the time (κ = 0.51). About 11 repeated trials were needed for a majority vote to match the 50-trial verdict with 95% probability. (<https://arxiv.org/abs/2606.13685>) **[abstract-only]**
- Only the best judges approach human agreement, judges lean lenient, and percent agreement hides large score differences. (<https://arxiv.org/abs/2406.12624>) **[abstract-only]**
- Judge quality varies by model. In Bloom's check on 40 hand-labelled transcripts, Claude Opus 4.1 correlated best with humans (Spearman 0.86) and Claude Sonnet 4.5 next (0.75). Claude judges were highly consistent on repeat scoring; GPT-5 showed much higher variance. (<https://alignment.anthropic.com/2025/bloom-auto-evals>) Those are 2025 models; current ones need their own check.
- For the statistics, Anthropic recommends reporting the standard error with every score, clustering it on the unit of randomization (here the scenario), comparing arms by paired differences on the same items, and running a power analysis first. (<https://www.anthropic.com/research/statistical-approach-to-model-evals>)

### 3.4 Self-preference

- LLM evaluators score their own outputs higher, and the bias grows with their ability to recognise their own text. (<https://arxiv.org/abs/2404.13076>) **[abstract-only]**
- Bloom includes self-preferential bias among its benchmarked behaviours and found it differs by model. (<https://alignment.anthropic.com/2025/bloom-auto-evals>)
- A practitioner counterpoint: "using the same model is usually fine because the judge is doing a different task", provided judge alignment is measured. (<https://hamel.dev/blog/posts/evals-faq/>)

This campaign compares several models running the skill, which is exactly where self-preference matters. Use judges from at least two families and report the judge-by-skill-model table, so that a judge favouring its own family is visible.

## 4. Generating a wide spread of scenarios

- **Dimensions, then tuples, then prose.** "A structured approach using dimensions produces far better synthetic data"; write about 20 tuples by hand first; turn each tuple into text in a separate prompt, which avoids repetitive phrasing. (<https://hamel.dev/blog/posts/evals-faq/>)
- **Bloom's version.** The ideation agent writes distinct base scenarios, the number set by a diversity parameter, and a variation agent "then expands through perturbations" that change substitutable details without changing the scenario's logic. (<https://alignment.anthropic.com/2025/bloom-auto-evals>)
- **Mode collapse is real.** Verbalized Sampling asks the model for several candidates with stated probabilities and reports 1.6 to 2.1 times more diversity in creative writing than direct prompting. (<https://arxiv.org/abs/2510.01171>, ICML 2026) **[abstract-only]**

Suggested dimensions for this effort (my proposal):

| Dimension | Example values |
|---|---|
| Project size | one evening; a month; multi-year |
| Domain | science; craft or trade; household; creative; business; community |
| Stakes | trivial; personal; professional |
| User expertise | novice; practitioner; expert |
| Temperament | terse; chatty; sceptical; indecisive; impatient; playful |
| Hidden preference | wants no metaphor; wants a small team; dislikes conflict; has a naming taste they will not state |
| Engagement with the framing | embraces it; indifferent; rejects it |
| Session shape | finishes; stops halfway; changes the project midway |

Sample tuples to cover every pair of values at least once instead of the full product, and hand-write the first 10 to 20 to check that the grid produces people who sound real.

## 5. Options for this effort

Token figures are **[estimate]**. Assumptions:

- One conversation is about 15 user turns: roughly 10 interview turns, team generation, and one discussion.
- Each turn resends a growing context of 25k to 60k tokens, most of it read from cache. That gives about 0.6M input tokens and 20k output tokens per conversation for the skill model.
- The simulated user adds about 75k input and 2k output tokens per conversation.
- A finished transcript is about 25k tokens.

Dollar costs depend on the models chosen and on cache pricing, and are not estimated here. Measure them on the first few runs from `total_cost_usd`.

### Option A: read-everything pilot

- **Shape.** 12 hand-written scenarios, one skill model, one run each, giving 12 conversations of about 15 turns. A human reads every transcript and writes down failure modes. No judge, or one judge pass used only to see where it disagrees with the reader.
- **Cost.** About 8M input tokens (cache-dominated) and 0.3M output tokens. A day of harness work and a few hours of reading.
- **Gives.** The failure list the rubric should be written from, a check that the simulated users behave like people, and measured per-run cost.
- **Does not give.** Any comparison between models, or any number worth quoting.

### Option B: scored matrix (recommended after A)

- **Shape.** 24 scenarios from the grid, 3 skill models, 2 repeats: 144 conversations. Two simulated-user models, alternated across scenarios.
- **Judging.** 5 isolated dimension judges, 2 judge families, 3 votes each: 30 judge calls per transcript, 4,320 in total. A human labels 30 transcripts on the same rubric to calibrate the judges before the scores are trusted.
- **Cost.** About 90M input tokens for the skill runs, 11M for simulated users and 110M for judging; around 5M output tokens overall. Judging is the largest item and can be cut to a third by taking one vote and re-voting only where the two families disagree.
- **Gives.** Per-dimension pass rates by model with standard errors clustered by scenario, the judge-by-model table for self-preference, and a split by project size for the ceremony question.
- **Does not give.** Evidence that the skill beats no skill.

### Option C: paired A/B on top of B

- **Shape.** 20 scenarios, 2 arms (the skill against a plain prompt asking for a persona team, or version 1 against version 2), 3 repeats, one skill model: 120 conversations. Each pair shares the scenario and the simulated-user sheet.
- **Judging.** Blind pairwise, both orders, ties allowed, 3 votes, 2 judge families: 12 calls per pair over 60 pairs is 720 calls, each reading two transcripts.
- **Cost.** About 75M input tokens for the runs, 9M for simulated users and 36M for judging.
- **Gives.** A direct answer to "is the skill worth installing" or "did this revision help", analysed as paired differences.
- **Risk.** Pairwise judges are the less stable kind (3.2), so a result should be believed only when both orders and both judge families agree.

### Common to all three

- **Harness.** A Python loop on `ClaudeSDKClient` with the skill loaded as a local plugin, a second model call producing each user turn, and a `canUseTool` handler that lets the simulated user answer `AskUserQuestion`. Running the skill under a non-Claude agent needs a second adapter, and whether Codex supports a resumable scripted session is **[unverified]**.
- **`claude plugin eval`** stays useful for trigger checks and for single-turn tests of the discussion phase on a seeded team, with its built-in no-plugin arm.
- **Human reading is not optional** in any option. Every source that measured judges or simulators against people found gaps large enough to change conclusions.

## Open points

- Which third-party agents load Agent Skills today, and whether each has a headless multi-turn mode.
- Whether `AskUserQuestion` reaches `canUseTool` when it is also listed in `allowed_tools`.
- Whether the Python and TypeScript SDKs really differ in default `settingSources`, as their reference pages suggest.
- How current models rank as judges; Bloom's ranking is from 2025.
- Most paper findings above are from abstracts. Before any figure is quoted in a spec, read the paper.
