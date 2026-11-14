# Prior art on persona teams

Research for ticket [02](../issues/02-prior-art-on-persona-teams.md). Compiled 2026-11-12.

**Question.** What is known about getting a single LLM to hold several distinct personas in one discussion: does it help reasoning, how does it fail, what keeps personas stable over a long exchange, which public skills are worth learning from, and what follows for how a persona is written and how a team is judged?

## How to read this file

Every source below was located by web search on 2026-11-12 and its title, authors and identifier were matched against arXiv, a publisher page, Crossref or the source repository. The "Checked" column says how far the reading went:

- **full**: the relevant section of the paper or the source file itself was read.
- **abstract**: the arXiv abstract page was read.
- **listing**: the paper's existence, authors and headline claim were confirmed from search results that quote the arXiv or proceedings listing. The body was not read. Treat the numbers attached to these as unverified detail.

One gap in the evidence shapes everything else. Almost all the quantitative work studies either (a) several separate model calls debating each other or (b) a single persona in a single chat. The case this skill cares about most, one model voicing several personas inside one context, has one well-known paper (Solo Performance Prompting) and otherwise practitioner testimony. Findings from (a) and (b) are applied to it here by argument, and each such step is labelled as inference.

## 1. Does multi-persona or multi-agent discussion improve reasoning?

### The original positive results

| Source | Checked | What it shows |
| --- | --- | --- |
| Wang, Mao, Wu, Ge, Wei, Ji. "Unleashing the Emergent Cognitive Synergy in Large Language Models: A Task-Solving Agent through Multi-Persona Self-Collaboration." NAACL 2024. [arXiv:2307.05300](https://arxiv.org/abs/2307.05300) | full (sections 3.4, 4, Limitations of v4) | Solo Performance Prompting (SPP): one model names the personas a task needs, voices them in turn and has a leader persona merge the result. Gains on Trivia Creative Writing, Codenames Collaborative and Logic Grid Puzzle with GPT-4. |
| Du, Li, Torralba, Tenenbaum, Mordatch. "Improving Factuality and Reasoning in Language Models through Multiagent Debate." ICML 2024. [arXiv:2305.14325](https://arxiv.org/abs/2305.14325) | listing | Several instances answer, read each other's answers and revise over rounds. Reported gains over single-call baselines on six reasoning and factuality tasks. |
| Liang, He, Jiao, Wang, Wang, Wang, Yang, Tu, Shi. "Encouraging Divergent Thinking in Large Language Models through Multi-Agent Debate." [arXiv:2305.19118](https://arxiv.org/abs/2305.19118) (v4, 2024) | listing | Names the "Degeneration-of-Thought" problem: once a model is confident, self-reflection stops producing new thoughts. Opposing debaters plus a judge are proposed as the cure. Also reports that an LLM judge favours the side running on its own backbone model. |
| Chan, Chen, Su, Yu, Xue, Zhang, Fu, Liu. "ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate." ICLR 2024. [arXiv:2308.07201](https://arxiv.org/abs/2308.07201) | listing (section 4.1 quoted in search result) | Ablation: giving every agent the same role description removes the benefit; the multi-agent setup then fails to beat a single agent. Distinct role prompts were necessary. |

Three details of the SPP paper matter directly for this skill.

1. **Personas chosen for the task beat fixed general ones.** The paper states that "dynamic, fine-grained personas are more effective than fixed, general personas" (section 4). A "Film Expert" answered where a generic "Expert" did not.
2. **Longer persona descriptions did not help task accuracy.** The SPP-Profile variant, which adds a written profile for each persona, scored slightly below plain SPP (76.7 vs 79.0 on Codenames, 64.8 vs 68.3 on Logic Grid Puzzle). The authors conclude that "a fine-grained persona name without a detailed description may already be sufficient for eliciting certain domain knowledge." This is a result about retrieving knowledge on short tasks. It says nothing about voice or about holding a persona over a long exchange.
3. **The effect depends on model strength.** "The effectiveness of SPP is not seen in smaller and less capable models like GPT-3.5 and Llama2" (section 3.4). Llama2 often stopped after listing the participants.

### The later critiques

| Source | Checked | What it shows |
| --- | --- | --- |
| Smit, Grinsztajn, Duckworth, Barrett, Pretorius. "Should we be going MAD? A Look at Multi-Agent Debate Strategies for LLMs." ICML 2024 (PMLR 235). [arXiv:2311.17371](https://arxiv.org/abs/2311.17371) | listing | In default settings, debate systems did not reliably beat simpler strategies such as self-consistency and ensembling. They are sensitive to hyperparameters. A prompt that tunes how readily agents agree with each other changed results substantially. |
| Wang, Wang, Su, Tong, Song. "Rethinking the Bounds of LLM Reasoning: Are Multi-Agent Discussions the Key?" ACL 2024. [arXiv:2402.18272](https://arxiv.org/abs/2402.18272) | listing | A single agent with a strong prompt nearly matches the best discussion method. Discussion wins mainly when the prompt has no demonstrations. Two recurring errors: judge mistakes and propagation of a wrong answer. |
| Zhang, Cui, Chen, Wang, Zhang, Wang, Wu, Hu. "Stop Overvaluing Multi-Agent Debate -- We Must Rethink Evaluation and Embrace Model Heterogeneity." [arXiv:2502.08788](https://arxiv.org/abs/2502.08788) (v3, June 2025) | abstract | Five debate methods, nine benchmarks, four models: debate often fails to beat chain-of-thought and self-consistency while spending far more compute. Mixing different underlying models consistently helps. |
| Choi, Zhu, Li. "Debate or Vote: Which Yields Better Decisions in Multi-Agent Large Language Models?" NeurIPS 2025. [arXiv:2508.17536](https://arxiv.org/abs/2508.17536) | listing | Majority voting accounts for most of the gain credited to debate. The theory models debate as a martingale over agents' beliefs, so debate alone does not raise expected correctness. Accuracy across rounds is roughly flat. Biasing updates toward correction helps. |
| Wynn, Satija, Hadfield. "Talk Isn't Always Cheap: Understanding Failure Modes in Multi-Agent Debate." [arXiv:2509.05396](https://arxiv.org/abs/2509.05396) (2025) | abstract | Debate can lower accuracy over rounds, even when stronger models outnumber weaker ones. "Models frequently shift from correct to incorrect answers in response to peer reasoning." |
| Cemri, Pan, et al. "Why Do Multi-Agent LLM Systems Fail?" [arXiv:2503.13657](https://arxiv.org/abs/2503.13657) (2025) | listing | MAST taxonomy: 14 failure modes in three groups (system design, inter-agent misalignment, task verification), built from annotated traces of multi-agent frameworks. Useful as a checklist, although it concerns task-executing agent systems more than discussions. |

### What this adds up to

- On questions with one checkable answer, a persona discussion is not a dependable accuracy booster. A single careful pass, or several independent passes with a vote, does as well or better for less cost.
- The reported gains come from two things: **independent samples** (which is why voting captures most of the benefit) and **real differences between the participants** (distinct roles in ChatEval, different models in Zhang et al., task-specific personas in SPP).
- Rounds of exchange add little on average and can remove a correct minority answer.
- Inference for this skill: a persona team should be sold on what the design notes already claim for it, "the different questions it brings into view", and never on being more correct than one voice. Its measurable value is coverage: which assumptions, alternatives and checks got named that a single answer left out. The design notes' sentence "The evidence still has to carry the conclusion" is the correct guard.
- Inference: when one model voices every persona there are no independent samples at all. The only remaining source of benefit is role difference, so role difference has to be real.

## 2. Failure modes

### 2.1 Personas collapsing into one voice

- Park, O'Brien, Cai, Morris, Liang, Bernstein. "Generative Agents: Interactive Simulacra of Human Behavior." UIST 2023. [arXiv:2304.03442](https://arxiv.org/abs/2304.03442). Checked: full for section 7.2 and footnote 3. The authors report that instruction tuning "seemed to make the agents overly cooperative with one another"; one agent "rarely said no" to suggestions. They also attribute the agents' formal conversational style to instruction tuning. So the default pull is toward a single polite, cooperative register.
- Cheng, Piccardi, Yang. "CoMPosT: Characterizing and Evaluating Caricature in LLM Simulations." EMNLP 2023. [arXiv:2310.11501](https://arxiv.org/abs/2310.11501). Checked: abstract. Their first criterion, **individuation**, asks whether a simulated persona's output can be told apart from the model's default answer to the same prompt. A persona that fails it has collapsed into the default voice.
- BMAD-METHOD `bmad-party-mode` skill (section 4). Checked: full source. Practitioner statement, not a measurement: "One model voicing five personas tends to make them agree." The skill offers subagent modes because, in its words, a review "fails if one mind voices every side and they drift into agreement", and for focus groups "one mind voices every customer and they bleed together."

Inference: collapse has two layers. Voices can sound alike (register, sentence shape), and voices can think alike (same first question, same conclusion). The second matters more and a rubric should test it separately.

### 2.2 False disagreement and false agreement

- Nemeth, Brown, Rogers. "Devil's advocate versus authentic dissent: stimulating quantity and quality." *European Journal of Social Psychology* 31, 707–720 (2001). [doi:10.1002/ejsp.58](https://doi.org/10.1002/ejsp.58). Checked: Crossref metadata and abstract as quoted in search results; full text was behind a paywall. In human groups, a person who actually held the minority view stimulated more and better solutions than any of three role-played devil's advocate conditions. Human-subject evidence, so its transfer to LLM personas is an analogy.
- Liang et al. (above) force "tit for tat" opposition; Smit et al. (above) find that the level of prescribed agreement is a sensitive knob. Together these say that disagreement set by instruction is a tuning parameter with no built-in link to whether the disagreement is warranted.
- Yao, Shang, Du, He, Lian, Zhang, Su, Swamy, Qi. "Peacemaker or Troublemaker: How Sycophancy Shapes Multi-Agent Debate." [arXiv:2509.23055](https://arxiv.org/abs/2509.23055) (2025). Checked: abstract. Sycophancy between agents collapses disagreement before a correct answer is reached and gives lower accuracy than a single agent. Debaters and judges fail differently. The paper argues for a balance of agreeable and contrarian dispositions.
- Choi, Zhu, Li. "When Identity Skews Debate: Anonymization for Bias-Reduced Multi-Agent Reasoning." ACL 2026. [arXiv:2510.07517](https://arxiv.org/abs/2510.07517). Checked: abstract. Agents weigh a statement by who said it. Deferring to a peer is "far more common than self-bias". Stripping identity markers reduces the bias.

Inference: a persona told "you are the skeptic, always push back" produces devil's-advocate dissent, the weak kind. A persona given a different **criterion** (what it counts as evidence, what it is afraid of getting wrong, what it looks at first) produces disagreement only when that criterion gives a different answer, and can agree without breaking character. The predecessor's Schnütter is written the second way: her standard comes from a reason in her history, and she is told to "revise your position openly" when Bafar's idea survives.

Inference from the identity-bias result: seniority and titles in a persona team are identity markers. A team in which the elder's line always carries, or the junior always yields, reproduces identity-weighted updating in fiction. The predecessor's rule that "the person who knows the method or has examined the data speaks first" works against this.

### 2.3 Agreement with the user

- Sharma, Tong, Korbak, et al. (19 authors, incl. Perez). "Towards Understanding Sycophancy in Language Models." ICLR 2024. [arXiv:2310.13548](https://arxiv.org/abs/2310.13548). Checked: listing. Five assistants showed sycophancy across four free-text tasks. Human preference data favours responses that match the user's stated view, which gives a training-level reason for the behaviour.
- Chen, Arditi, Sleight, Evans, Lindsey. "Persona Vectors: Monitoring and Controlling Character Traits in Language Models." [arXiv:2507.21509](https://arxiv.org/abs/2507.21509) (2025). Checked: abstract. Sycophancy is one of the traits that corresponds to a direction in activation space, which marks it as a property of the underlying model that a prompt can lean against but does not remove.

Inference: every persona is voiced by the same model and inherits the same pull toward the user. A whole team can therefore agree with the user while appearing to debate each other, with the disagreement confined to side issues. A persona's stated loyalty should be to something other than the user's preference (the evidence, the claim's exposure to a referee, the cost of being wrong), and the description should say what the persona does when the user is the one whose idea is weak. The predecessor has this in Bafar: "Treat my preferred mechanism, your first explanation, and a published account with the same scrutiny."

### 2.4 Stereotype and caricature

| Source | Checked | What it shows |
| --- | --- | --- |
| Cheng, Durmus, Jurafsky. "Marked Personas: Using Natural Language Prompts to Measure Stereotypes in Language Models." ACL 2023. [arXiv:2305.18189](https://arxiv.org/abs/2305.18189) | listing | Model-written personas for marked demographic groups contain more stereotype than human-written ones, including in positive-sounding text (othering, essentializing). |
| Cheng, Piccardi, Yang. CoMPosT (above) | abstract | Caricature = individuation plus **exaggeration** of the persona's defining features. GPT-4 simulations of political and marginalized groups, and of general topics, were most prone. |
| Wang, Morgenstern, Dickerson. "Large language models that replace human participants can harmfully misportray and flatten identity groups." *Nature Machine Intelligence* 2025 (venue not independently confirmed here). [arXiv:2402.01908](https://arxiv.org/abs/2402.01908) | listing | Identity-prompted models misportray groups, flatten within-group variety and essentialize identity. Inference-time fixes reduce the effect without removing it. |
| Gupta, Shrivastava, Deshpande, Kalyan, Clark, Sabharwal, Khot. "Bias Runs Deep: Implicit Reasoning Biases in Persona-Assigned LLMs." ICLR 2024. [arXiv:2311.04892](https://arxiv.org/abs/2311.04892) | listing | Assigning a socio-demographic persona changed reasoning scores on most of 24 datasets; models sometimes declined tasks by citing presumed limits of the persona. Simple prompt mitigations were insufficient. |
| Li, Chen, Namkoong, Peng. "LLM Generated Persona is a Promise with a Catch." [arXiv:2503.16527](https://arxiv.org/abs/2503.16527) (2025) | listing | The more of a persona's detail the LLM itself invents, the more skewed the resulting simulation. Bias enters at the generation step. |
| Park, Zou, Shaw, Hill, Cai, Morris, Willer, Liang, Bernstein. "Generative Agent Simulations of 1,000 People." [arXiv:2411.10109](https://arxiv.org/abs/2411.10109) (2024) | listing | Agents built from two-hour interviews predicted their source person's answers better, and with less demographic bias, than agents built from demographic or short persona descriptions. |

Inference: the stereotype risk is highest where a persona is specified by a label (nationality, gender, age, job title) and the model is left to fill in what such a person is like. It is lowest where behaviour is specified directly and biography explains it. Two consequences for a generator skill: detail should come from the user's answers and from the project, and what the model invents should be shown to the user for correction. Where a persona carries cultural markers, as Bafar bhai does, the description should bound them; the predecessor's "Use 'bhai' sparingly, when it feels natural" is an exaggeration control of the kind CoMPosT would ask for.

### 2.5 A persona can lower task performance

- Zheng, Pei, Logeswaran, Lee, Jurgens. "When 'A Helpful Assistant' Is Not Really Helpful: Personas in System Prompts Do Not Improve Performances of Large Language Models." Findings of EMNLP 2024. [arXiv:2311.10054](https://arxiv.org/abs/2311.10054). Checked: listing. 162 roles, four model families, 2,410 factual questions: a persona in the system prompt did not improve accuracy over no persona, and the effect of any given persona was largely unpredictable.
- Kim, Yang, Jung. "Persona is a Double-edged Sword" (title varies by version; Findings of IJCNLP-AACL 2025). [arXiv:2408.08631](https://arxiv.org/abs/2408.08631). Checked: listing. Role-play prompts degraded zero-shot reasoning on a majority of the datasets tested for one model; an ensemble with a neutral prompt recovered the loss.

Inference: character should not stand between the persona and the facts. The predecessor already separates the two ("Every voice reads the data before it speaks"); a general skill should keep that order explicit: do the reading and the reasoning, then speak in voice.

## 3. What makes a persona hold up over a long exchange

| Source | Checked | What it shows |
| --- | --- | --- |
| Li, Liu, Bashkansky, Bau, Viégas, Pfister, Wattenberg. "Measuring and Controlling Instruction (In)Stability in Language Model Dialogs" (earlier title "Measuring and Controlling Persona Drift in Language Model Dialogs"). COLM 2024 (venue not independently confirmed here). [arXiv:2402.10962](https://arxiv.org/abs/2402.10962) | abstract | Two system-prompted chatbots talking to each other drift from their instructions "within eight rounds". The authors link this to attention on the system prompt decaying as the dialogue grows. |
| Luz de Araujo, Hedderich, Modarressi, Schütze, Roth. "Persistent Personas? Role-Playing, Instruction Following, and Safety in Extended Interactions." EACL 2026. [arXiv:2512.12775](https://arxiv.org/abs/2512.12775) | listing | Dialogues of over 100 rounds, seven models: persona fidelity declines with length, most in goal-oriented tasks, and persona-assigned models converge toward the no-persona baseline. |
| Choi, Hong, Kim, Kim. "Examining Identity Drift in Conversations of LLM Agents." [arXiv:2412.00804](https://arxiv.org/abs/2412.00804) (2024) | abstract | Nine models: "Larger models experience greater identity drift" and "Assigning a persona may not help to maintain identity." |
| Abdulhai et al. "Consistently Simulating Human Personas with Multi-Turn Reinforcement Learning." [arXiv:2511.00222](https://arxiv.org/abs/2511.00222) (2025). Author list not checked. | listing | Defines three consistency measures: prompt-to-line (does a turn match the persona description), line-to-line (does it match the persona's earlier turns), and Q&A consistency (does the persona answer probe questions the same way later). |
| Lu et al. "The Assistant Axis: Situating and Stabilizing the Default Persona of Language Models." [arXiv:2601.10387](https://arxiv.org/abs/2601.10387) (2026). Author list beyond the first author not checked. | listing | A leading direction in activation space measures distance from the default Assistant persona. Task-like conversation (coding, writing) keeps a model near the default; emotionally open or self-reflective conversation moves it away. |

What the evidence supports, and where it stops:

- **Drift is the default, and its direction is toward the generic assistant.** Three independent studies agree on this. A persona description alone does not prevent it.
- **Goal-directed work accelerates the loss.** This is the working mode of a project team, so the risk is highest exactly where the skill is used (Luz de Araujo et al.).
- **Re-exposure to the description is the mechanism-level remedy.** If drift follows from fading attention to the original instruction (Li et al.), then putting the description, or a short form of it, back near the point of generation counters it. The predecessor does this by instruction: "Read the persona file of every voice that speaks." BMAD does it by carrying each persona and the scene "into any spawned brief". This is inference from the proposed mechanism; no study in this list tested re-reading as an intervention in a multi-persona setting.
- **No study found here compares persona-description styles for long-run stability.** The claims below about what kind of description lasts are therefore reasoned from the failure modes, with practitioner support, and are untested:
  - Behavioural specifics survive better than adjectives. "Asks what we would observe if he were wrong" can be checked against a turn; "skeptical" cannot. BMAD's authoring guide makes the same point: "'Skeptical CFO' is a placeholder; 'won't approve anything without a payback under 18 months, and says so in the first thirty seconds' is a persona."
  - A sample line in the persona's voice gives the model a concrete anchor for register. Both predecessor seniors end with one.
  - A persona defined partly by its relation to another persona (the predecessor's "With Bafar bhai" section) is re-anchored each time the other speaks.
  - A short description is cheaper to re-read at every turn, which matters if re-exposure is the remedy. The SPP-Profile result shows that length does not buy accuracy.

## 4. Existing persona-team skills and prompts

All three were read at source on 2026-11-12.

### BMAD-METHOD `bmad-party-mode`

Source: [SKILL.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/skills/core-tools/bmad-party-mode/SKILL.md), its `references/` files, and the [docs page](https://docs.bmad-method.org/customize/run-multi-agent-discussions/). The closest public relative of the planned skill.

- **Four ways to convene.** `session` (one model voices everyone), `auto` (inline by default, separate agents "only for the rounds where independence changes the answer"), `subagent` (one agent per persona every substantive round), `agent-team` (persistent agents addressing each other; Claude Code only). Each mode falls back to `session` where the harness cannot spawn agents. This is a ready model for an agent-agnostic skill.
- **When independence is worth paying for.** Spawn when the round is "a genuine evaluation, review, or critique", when "the personas would plausibly reach *different* conclusions, and that divergence is the point", or when the user asks someone to dig in. Voice inline for the rest. This matches the literature: independence is where the measured benefit lives.
- **One shared room.** In subagent mode every persona receives the whole exchange each round, including turns it did not speak in, so the room stays one conversation.
- **A blind-identification bar.** "Hide the labels and you'd still know who's speaking." Usable directly as a rubric test.
- **Resist the tidy ending.** "Your instinct is to reconcile the voices and tie a bow — resist it. Clean consensus that took no effort is where the party dies." It also ships an "Anti-Consensus Club" that is "not a voting body" and hands the decision back to the user.
- **Draft, then let the user react.** "Draft, don't interrogate. Propose a first cut of each persona and let the user react." Relevant to the casting conversation (ticket 06).
- **Where it differs from our aims.** It optimises for liveliness ("A balanced panel is boring", "get heated") and forbids summaries unless asked. It has no evidence rule. Its data-driven casting compresses customer data into "N stereotypical personas", a phrase that section 2.4 gives reason to avoid, although the same passage sensibly says to cluster "by what actually differentiates behavior ... not surface demographics alone".

### Karpathy `llm-council`

Source: [README](https://github.com/karpathy/llm-council). Different models answer independently; each then ranks the others' answers with identities hidden "so that the LLM can't play favorites"; a chairman model writes the final answer. No personas. The lessons are structural: independent first answers before anyone sees anyone else's, anonymised review, and a separate synthesis step. The anonymisation choice anticipates the identity-bias finding in section 2.2. The author describes it as a weekend project that he does not maintain.

### `aiwithremy/claude-skills-llm-council`

Source: [SKILL.md](https://github.com/aiwithremy/claude-skills-llm-council/blob/HEAD/SKILL.md). Karpathy's three stages inside one vendor, using subagents with five "thinking styles": Contrarian, First Principles Thinker, Expansionist, Outsider, Executor.

- The roster is built from **named tensions**: "Contrarian vs Expansionist (downside vs upside). First Principles vs Executor (rethink everything vs just do it)." The Expansionist/Contrarian pair is the same axis as the moon and sun seniors.
- It states that the advisors are "not job titles or personas. They're thinking styles that naturally create tension with each other", which sidesteps the stereotype problem by having no biography at all, at the cost of voice.
- It says when not to convene: factual lookups, creation tasks and processing tasks get no council.
- Its Contrarian "assumes the idea has a fatal flaw and tries to find it", which is the role-played dissent that section 2.2 flags as the weaker form.

### Others, seen in listings only

Search returned further council-style skills that were not read at source: `affaan-m/everything-claude-code` (council with Skeptic, Pragmatist and Critic subagents), `modbender/council-of-the-wise` (personas as files in a folder, plus a separate synthesis persona), `torpedod/claude-council` (includes a bias-audit step), `nyldn/claude-octopus` (multi-model council with veto gates), `bistromath-works/boardroom` (18-persona advisory board). Their descriptions come from third-party skill directories and are **unverified**. The pattern across them is consistent: fixed archetype rosters, subagents for independence, a synthesis role.

### What the public skills lack

None of the three read at source generates a team from a conversation about the user's project, gives personas blind spots or reasons for their standards, tests for stereotype, or checks for agreement with the user. The predecessor research-group skill already has several of these (blind spots, evidence rules, an explicit senior-pair relationship), so the planned skill starts ahead of the public field on persona writing and behind BMAD on convening modes.

## Implications for how a persona is written

Each point names the evidence it rests on. "Inference" marks points that follow by argument from the failure modes and have no direct test.

1. **Lead with how the persona works, and let biography explain it.** Write what the persona notices first, the question it habitually asks, what it accepts as evidence, and what it fears getting wrong. Add history only where it gives a reason for those habits. Evidence: stereotype enters where the model fills in a label (Marked Personas, CoMPosT, Wang et al.); interview-grounded agents beat demographic ones (Park et al. 2024).
2. **Give each persona a criterion, never a stance.** "Asks which sentence the evidence supports today" lets the persona agree or disagree as the case requires. "Always challenges" manufactures dissent. Evidence: Nemeth et al. on authentic versus role-played dissent (human subjects); the sensitivity of debate to prescribed agreement (Smit et al.).
3. **State the persona's characteristic excess and how it corrects it.** The design notes already ask for "tendencies that can go too far under pressure". Including them gives the persona a way to be wrong in character and to concede in character, which is what keeps disagreement from being decorative. Inference, supported by the predecessor ("You sometimes ask for one control too many").
4. **Write the loyalty and the behaviour toward the user.** Say what the persona serves ahead of the user's preference, and what it does when the user's idea is the weak one. Evidence: sycophancy is a model-level trait every persona inherits (Sharma et al.; Chen et al.).
5. **Write how the persona changes its mind.** Name the kind of observation that would move it. This counters both obstinacy and deference to whoever spoke last (Choi et al. 2026; Wynn et al.).
6. **Define seniors by their relation to each other.** For the two-persona minimum, each description should include a short section on the other: what the other sees that this one misses, and how this one disagrees (restate the other's idea in terms they would accept, then name the disputed step). Evidence: distinct roles are what make a discussion beat a single voice (ChatEval); inference that mutual reference re-anchors both voices.
7. **Keep it short enough to re-read at every convening, and re-read it.** Aim for the length of the predecessor's Schnütter file or shorter. Evidence: drift within eight rounds and decay toward baseline (Li et al.; Luz de Araujo et al.); longer profiles did not help accuracy (SPP-Profile).
8. **End with one or two lines in the persona's voice.** A concrete register anchor. Inference, with practitioner support (predecessor files; BMAD's "pick out blind" standard).
9. **Bound the colour.** Verbal habits, cultural references and humour get a frequency note ("sparingly", "when it fits"). Evidence: exaggeration is half of the definition of caricature (CoMPosT).
10. **Source the detail from the user and the project; show the user what the model invented.** Evidence: model-invented persona detail adds bias (Li et al. 2025); SPP's finding that task-derived personas beat generic ones supports casting from the project.
11. **Keep character out of the way of the facts.** The persona reads the material and reasons before it speaks; the voice shapes which question is asked and how the answer is put. Evidence: persona prompts do not improve and can lower accuracy (Zheng et al.; Kim et al.; Gupta et al.).
12. **Make names carry no behaviour.** The design notes already say "The person's temperament comes from their story and how they work." A name that evokes a place must not be the model's cue for how the person thinks. Evidence: section 2.4.

## What a rubric should look for

Grouped by what is being judged. Each item is phrased so a reader or a judging model can answer it from the persona files or from a transcript.

**The persona files**

- *Behavioural specificity.* Could a reader predict this persona's first question on a new topic from the file? Count checkable behaviours against bare adjectives.
- *Criterion over stance.* Does the file give a standard the persona applies, or an attitude it must always display?
- *Excess and correction.* Is a way of going too far named, with what brings the persona back?
- *User relation.* Does the file say what happens when the user is wrong?
- *Grounding.* Can each trait be traced to something the user said or the project needs? Which details did the model add unprompted?
- *Stereotype check.* Remove the name, gender, nationality and age. Does the behaviour description still stand and still make sense? Does any behaviour follow from a demographic label alone?
- *Re-readable length.* Is it short enough to be loaded every time the persona speaks?

**The team**

- *Real tension.* For the two seniors, name the axis on which they differ. Would they give different first reactions to the same input? For a larger team, does every member differ from every other on at least one working habit?
- *Coequality.* Does either senior's line always close the exchange? Is there any casting vote?
- *Coverage against the project.* Do the personas' questions, taken together, cover what the project needs asked?

**A transcript of a discussion**

- *Blind identification.* With speaker labels removed, can a reader assign turns to personas? Test separately for voice (register) and for substance (which concern was raised).
- *Individuation.* Does each persona's contribution differ from what the model says with no persona at all? Run the same question unpersonified as a baseline.
- *Exaggeration.* Are the persona's markers present at a rate a real person would show, or in every turn?
- *Disagreement that bears weight.* Is each disagreement about a named claim or step, with a reason drawn from the persona's criterion? Does it change the next action or the stated observation that would settle it? Disagreement on side issues with agreement on the main point counts as none.
- *Agreement that is earned.* When a persona concedes, is the evidence or argument that moved it stated? Flag concessions that follow only repetition or seniority.
- *User-challenge test.* Give the team a user proposal with a planted flaw and a user who states a preference for it. Does any persona name the flaw? Does the team hold the point under one round of user pushback?
- *Minority survival.* Plant a correct point with one persona only. Is it still present in the conclusion?
- *Stability over length.* Compare turns 1–3 with turns after round 8 or later on the three consistency measures of Abdulhai et al.: does a late turn still match the file, match the persona's own earlier turns, and answer a repeated probe question the same way?
- *Evidence discipline.* Is every factual claim tied to something read or marked unchecked? Did any persona invent a citation, number or completed check?
- *Value over a single voice.* List the assumptions, alternatives and checks the discussion surfaced. How many are absent from a single careful unpersonified answer to the same prompt? If none, the team added length only.
- *An honest ending.* Does the close say what is established, what remains disputed, and the next step or the observation that would settle it, without forcing consensus?

## Open questions this research could not settle

- Whether one model voicing several personas in a single context collapses faster than separate agents do. BMAD asserts it; no controlled comparison was found.
- Whether any persona-description style measurably improves long-run stability. The drift papers measure drift and propose model-level fixes; none compares ways of writing the description.
- Whether re-reading a persona file at each convening actually restores fidelity in a multi-persona setting. It follows from the attention-decay account and is cheap to test in the skill's own test campaign (tickets 03 and 10).
- How far the Nemeth result on human groups transfers to model personas.
