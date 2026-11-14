# Testing skills with simulated users

Type: research
Status: resolved
Blocked by: none

## Question

How are conversational skills tested with model agents today? Cover: tooling for skill evals (Anthropic skill-creator evals, `claude plugin eval`, equivalents for other agents); methods for simulated users in a multi-turn interview (how to give them a project, a temperament and hidden preferences, and how to stop them being too cooperative); LLM-as-judge practice for subjective qualities (rubric design, pairwise comparison, judge reliability, variance across runs); and how to generate a deliberately wide spread of hypothetical projects. End with the options open to this effort and their rough cost.

## Answer

Findings: [research/03-testing-skills-with-simulated-users.md](../research/03-testing-skills-with-simulated-users.md)

- **Tooling.** No existing skill-eval tool runs a simulated user through a multi-turn interview. `claude plugin eval`, skill-creator's evals and OpenAI's Codex skill-eval recipe are all single-prompt. `claude plugin eval` can replay one further turn from a saved transcript, which is enough for trigger checks and for testing the discussion phase on a seeded team.
- **Harness.** The interview needs a custom loop. The Claude Agent SDK documents the pieces: `ClaudeSDKClient` for turn-by-turn driving, the skill loaded as a local plugin, `canUseTool` to answer `AskUserQuestion`, a per-run model option and cost totals. Other agents need their own adapter (not verified).
- **Simulated users.** Measured against 451 real people, simulators are too cooperative, polite and verbose (3-word turns: 1% against 29% for humans) and rate too generously. Persona prompting alone did not fix it. Give the simulated user a private fact sheet, a disclosure rule, temperament written as behaviours, and a stop action. Never use it as the rater. Vary the model that plays it.
- **Judging.** Use one isolated judge per rubric dimension, yes/no questions with cited evidence, an "unknown" exit, several votes, and judges from at least two model families. Calibrate on about 30 human-labelled transcripts. Pairwise comparison only for a specific A/B question, blind and in both orders; it is the less stable protocol. Report standard errors clustered by scenario.
- **Scenario spread.** Define dimensions (size, domain, stakes, expertise, temperament, hidden preference, engagement, session shape), sample tuples for pairwise coverage, and write each up in a separate step. Hand-write the first 10 to 20.
- **Options (token figures are my estimates).**
  - A, pilot: 12 conversations, one model, a human reads everything. About 8M input tokens.
  - B, scored matrix: 24 scenarios x 3 models x 2 repeats = 144 conversations, 5 dimension judges from 2 families. About 210M input tokens, half of it judging.
  - C, paired A/B: 120 conversations, blind pairwise. About 120M input tokens.
  - Suggested order: A, then B, with C only if "is the skill better than no skill" needs an answer.
- **Uncertain.** Most paper figures come from abstracts. The list of agents supporting Agent Skills could not be read. Dollar costs are not estimated.
