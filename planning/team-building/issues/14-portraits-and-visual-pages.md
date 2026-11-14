# Portraits and visual pages

Type: grilling
Status: resolved
Blocked by: 07

## Question

The research group skill now ships visual templates: a shared art direction per set (an editorial portrait set,
a cartoon character-card set), a base prompt with slots, per-character cues, and a page pairing, for hosts with
an image generator (`../research-group-skill/skills/research-group/references/visual-templates.md`). The general
skill should offer the same for any generated team.

Settle: whether visual cues are part of a persona file (a ninth part) or are derived when asked; how a shared art
direction is chosen for a team (from the group metaphor, from the user's taste, a few worked examples plus
composition, as with metaphors and ways of convening); what the skill produces on a host with no image generator
(prompts only, or nothing); when it is offered (at casting, or only on request); where images and prompts are
saved; and the limits that always hold (fictional people, no real likeness, no logos or readable text), including
what happens when the user asks for a persona who looks like a real person.

## Answer

Resolved 2026-11-14 by grilling.

**Visual cues are derived, not stored in a persona.** There is no ninth part. When a portrait is asked for, the
skill derives one line of cues per persona (what they look like, wear and hold) from the eight parts. The prop
and posture come from the persona's habits. Any joke is in the prop, never in the body or the background. All
are adults.

**One art direction per set, from worked examples plus composition.** The team-building skill ships the two
existing directions (editorial portrait, character cards) in a fixed form: use, art direction, base prompt with
slots, page pairing. It proposes one in two or three lines, taken from the group metaphor and the team's tone,
and the user revises in plain words. No taste questionnaire.

**One screen before anything is generated.** The art direction and every persona's cue line are shown together.
The user revises, then says go. One line then says how many images will be made. The skill makes one portrait
first, shows it, and makes the rest only on the user's yes.

**When it is offered.** On request, plus one line once, right after a team is saved and only on a host that can
generate images. Never in the casting proposal. Never for a pair in chat unless asked.

**Where it lives.** In the team skill folder, as `portraits/`: `prompts.md` (the art direction as used, the
filled base prompt, one cue line per persona), the images, and the team page if one was made. A refresh never
touches it. A persona edit leaves it alone unless the user asks. A team keeps one current set: a regenerated
portrait replaces the old file, and a new art direction removes the old set only on the user's yes. Each change
leaves the dated line at the foot of `team.md`. This adds `portraits/` to the folder described in
[The team as its own skill](15-the-team-as-its-own-skill.md).

**Which skill makes them.** New sets and new art directions go through the team-building skill only. A team
skill that has `portraits/prompts.md` can regenerate one portrait or add a new member's from that file. Without
the file and without the team-building skill, it says so and does not improvise.

**No image generator.** Prompts only, and only on request: the same `prompts.md`, one line saying this host
cannot make images, and that the prompts can be pasted into any generator. No placeholder images.

**The team page.** On request, one self-contained HTML page in `portraits/`. Its text comes from `team.md` and
the personas: name, seat, what each is heard first on, their one line in voice. Captions are plain and
respectful. The line saying the team is fictional is always on the page. The skill never publishes it.

**Limits that always hold.** Written into every base prompt, and not removable by a revision:
- fictional adults only;
- no real person's likeness;
- no logos or institutional insignia;
- no readable text.

**A likeness request.** Declined in one sentence, with an offer of the describable traits (era, dress, bearing)
and no name in the prompt. A real person's name or photo never enters a prompt. That includes the user's own,
since the user is not a character on the team. No "in the style of" a named living artist.

**When the generator disobeys.** Where the host lets it see images, the skill looks at each one before showing
it and regenerates once if there is readable text, a logo or insignia. It cannot reliably recognise a likeness,
says so once, and adds no recognition step. If the user says a portrait looks like someone real, it is
regenerated with changed cues, without argument.

**Pairs and retention.** Retention levels do not apply: portraits are part of the team, not a record of its
conversations. A pair gets portraits in chat. To keep them, the pair is saved as a team skill and the portraits
go into its `portraits/`.

**Sharing.** When the user passes the team on, the skill mentions that `portraits/` goes with it and offers a
copy without it.

**For later tickets.** Portraits are outside the rubric and the test campaign in v1. One text-only check is
suggested, not decided here: a likeness request is declined and no real name reaches a prompt. Noted in
[Guardrails in practice](18-guardrails-in-practice.md) and [Test campaign design](10-test-campaign-design.md).
