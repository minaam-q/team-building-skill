# Prior art on persona teams

Type: research
Status: resolved
Blocked by: none

## Question

What is known about getting a single LLM to hold several distinct personas in one discussion? Cover: published work on multi-persona and multi-agent debate prompting (does it improve reasoning, and when does it not); known failure modes (personas collapsing into one voice, false disagreement, agreement with the user, stereotype and caricature); what makes persona descriptions hold up over a long exchange; and existing persona-team or 'board of advisors' skills and prompts worth learning from. End with what this implies for how a persona should be written and what a rubric should look for.

## Answer

Full findings with sources: [research/02-prior-art-on-persona-teams.md](../research/02-prior-art-on-persona-teams.md).

- **Does it improve reasoning?** Not dependably. Solo Performance Prompting (Wang et al., NAACL 2024) and multi-agent debate (Du et al., ICML 2024) reported gains, but later studies (Smit et al. 2024; Wang et al. 2024; Zhang et al. 2025; Choi et al., NeurIPS 2025; Wynn et al. 2025) find that debate rarely beats a careful single pass or a simple vote, and can lose a correct minority answer. The gains that hold come from independent samples and from participants that really differ (distinct roles, different models, personas chosen for the task).
- **So the team's value is coverage** (which questions, alternatives and checks get named), never extra correctness. With one model voicing everyone there are no independent samples, so role difference has to be real.
- **Failure modes.** Voices merge into one polite cooperative register (Park et al. 2023); prescribed dissent is the weak kind (Nemeth et al. 2001, human groups); agents defer to peers and to the user (Sharma et al. 2024; Yao et al. 2025; Choi et al. 2026); label-based personas become stereotypes, more so when the model invents the detail (Cheng et al. 2023; Li et al. 2025); a persona prompt can lower task accuracy (Zheng et al. 2024).
- **Long exchanges.** Personas drift toward the generic assistant within about eight rounds, fastest in goal-directed work (Li et al. 2024; Luz de Araujo et al. 2026). Re-reading a short description at each convening is the remedy that follows from the mechanism. No study compares description styles, so that part is inference.
- **Existing skills.** BMAD `bmad-party-mode` is the closest relative: four convening modes with fallback, separate agents only where independence changes the answer, a blind-identification bar. Karpathy's `llm-council` contributes independent first answers and anonymised review. None casts a team from the user's project or gives personas blind spots; the predecessor already does the latter.
- **Writing a persona.** Lead with working habits and let biography explain them; give a criterion, not a stance; name the excess and its correction; say what the persona does when the user is wrong and what would change its mind; define the seniors by their relation to each other; keep it short and re-read it; end with a line in voice; bound the colour; take detail from the user and the project.
- **Rubric.** Blind identification (voice and substance separately), difference from an unpersonified baseline, disagreement that changes the next step, earned concessions, a planted-flaw user-challenge test, minority survival, stability after round 8, evidence discipline, and items surfaced beyond a single careful answer.
- **Not settled.** Whether single-context voicing collapses faster than separate agents, and whether any description style or re-reading measurably improves stability. Both can be tested in tickets 03 and 10.
