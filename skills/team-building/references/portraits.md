# Portraits and the team page

Made on request. Offer them once, in one line, right after a team is saved on a host that can generate images.

## Limits in every prompt

Four constraints are written into every base prompt and stay through every revision:

- fictional adults only;
- no real person's likeness;
- no logos or institutional insignia;
- no readable text.

A real person's name or photo stays out of every prompt, the user's own included. When the user asks for a
persona who looks like a real person, decline in one sentence and offer the describable traits (era, dress,
bearing) under no name. Describe a style by its medium and qualities, never by a living artist's name.

## Steps

1. **Visual cues.** Derive one line per persona from its file: what they look like, wear and hold. The prop and
   posture come from the persona's habits. Any joke lives in the prop. Cues are made for the portrait and are no
   part of the persona file.
2. **Art direction.** One per set, so the members read as one group. Propose it in two or three lines, taken
   from the group metaphor and the team's tone, starting from a worked example below or composed in the same
   form.
3. **One screen.** Show the art direction and every cue line together. The user revises in plain words, then
   says go.
4. **Generate.** Say in one line how many images will be made. Make one portrait, show it, and make the rest on
   the user's yes. Where the host lets you see images, look at each before showing it and regenerate once if it
   carries readable text, a logo or insignia. Say once that you cannot reliably recognise a likeness. If the
   user says a portrait looks like someone real, regenerate it with changed cues.
5. **Save** in the team skill folder as `portraits/`: `prompts.md` (the art direction as used, the filled base
   prompt, one cue line per persona) and the images, named after the persona files. Add the dated line under
   `## Changes` in `team.md`.

A team keeps one current set. A regenerated portrait replaces the old file. A new art direction replaces the old
set on the user's yes. A pair in chat gets its portraits in chat; to keep them, save the pair as a team skill.

**No image generator.** On request, write `portraits/prompts.md` alone, say in one line that this host cannot
make images, and that the prompts can be pasted into any generator.

## Art directions

Each has four parts: use, art direction, base prompt with slots, page pairing. Rewrite the bracketed domain
words for the team at hand.

### Editorial portrait

**Use:** a polished page, a report, a landing page.

**Art direction:** photorealistic, high-end editorial photography; believable, richly textured, human; 4:5
vertical, waist-up, eye level; a light environment related to the person's work, props softly out of focus;
muted mineral tones, cream, and one accent per seat.

```text
Asset type: Editorial portrait for a fictional [GROUP METAPHOR] page
Primary request: A richly textured, photorealistic portrait of a fictional [SEAT] who is [TEMPERAMENT].
Scene/backdrop: [WORKPLACE] with [MOTIF FROM THEIR HABITS] out of focus.
Subject: [APPEARANCE], wearing [CLOTHING], [POSE/EXPRESSION], holding [PROP].
Style/medium: High-end editorial photography, grounded and believable.
Composition/framing: 4:5 vertical portrait, waist-up, eye level, generous breathing space.
Lighting/mood: [LIGHTING], [PALETTE].
Constraints: A wholly fictional adult, resembling no real person; no watermark, no logos or insignia, no
readable text anywhere in the image.
```

**Page pairing:** spacious cream-and-ink layout, serif display type, compact mono labels, a dark photo-card
grid.

### Character cards

**Use:** an informal team page, an onboarding guide, workshop material.

**Art direction:** editorial cartoon in hand-painted gouache with crisp coloured-pencil linework; expressive,
dignified adult characters; rounded, slightly exaggerated shapes, subtle paper grain; pale cream background with
one bold geometric colour block. Same line weight, crop and palette for every character.

```text
Asset type: Character card for a fictional [GROUP METAPHOR] page
Primary request: A warm, funny editorial cartoon illustration of a fictional [SEAT] whose signature habit is
[HABIT].
Scene/backdrop: Pale cream studio background, one [ACCENT COLOUR] geometric shape, a small playful vignette:
[MOTIF FROM THEIR HABITS].
Subject: [APPEARANCE], wearing [CLOTHING], [POSE/EXPRESSION], holding [PROP]. The personality reads through
posture and the prop, kindly.
Style/medium: Hand-painted gouache editorial illustration, crisp coloured-pencil outlines, flat textured
colours, subtle paper grain, slightly exaggerated adult proportions.
Composition/framing: 4:5 vertical character card, three-quarter or full body, centred, generous margins.
Colour palette: ink navy, tomato red, mustard, teal, pale cream; [ACCENT COLOUR] leads.
Constraints: A wholly fictional adult, resembling no real person; no watermark, no logos or insignia, no
readable text anywhere in the image.
```

**Page pairing:** off-white paper background, irregular coloured cards with rounded borders, oversized friendly
display type, simple ink doodles, concise captions.

## The team page

On request: one self-contained HTML page in `portraits/`, following the art direction's page pairing. Its text
comes from `team.md` and the personas: name, seat, what each is heard first on, their one line in voice.
Captions are plain and respectful. The page always carries the line saying the team is fictional. It is a file
for the user; publishing it is theirs to do.

## From a team skill

A team skill that has `portraits/prompts.md` can regenerate one portrait or add a new member's from that file. A
new set or a new art direction is made here.
