# Wayfinder's terms and shape

Type: research
Status: resolved
Blocked by:

## Question

What can a public skill build on in Matt Pocock's wayfinder? Find: its licence and whether a third-party skill
may depend on it, bundle it or adapt its text; how it is installed on hosts other than Claude Code; which parts
of its ticket and claim model a persona-as-assignee would touch (the claim is "assigned to the dev driving the
map"; tracker docs' wayfinding operations); and whether comparable map-and-ticket skills exist that the team
skill should also work with.

## Answer

Resolved 2026-11-14. Full findings with sources:
[research/17-wayfinder-terms-and-shape.md](../research/17-wayfinder-terms-and-shape.md).

- **Licence:** MIT. Depending on wayfinder carries no condition; bundling or adapting its text needs the copyright
  and permission notice to travel with it (upstream's own habit is a `CREDITS.md` beside the skill).
- **It moves fast.** The local copy is 1.2.3; upstream is 1.3.2 (2026-11-13), which renamed `CONTEXT.md` to
  `GLOSSARY.md` and now says `wayfinder:` labels are the only labels a ticket carries.
- **It is not one skill.** Wayfinder needs `grilling`, `domain-modeling`, `research`, `prototype` and a user-run
  setup skill, out of a bundle of 27. Install routes exist for Codex, Copilot, Gemini CLI and, through
  `npx skills add`, several others; some hosts are untested upstream.
- **Wayfinder is user-invoked.** No other skill can start it. A team skill can only read and write the same
  tracker files.
- **Ownership:** wayfinder's only concept is the claim ("a session is working this now"): the native assignee on
  GitHub and GitLab, `Status: claimed` with no identity on local markdown. An extra `Persona:` line works on all
  three shipped trackers without changing wayfinder, because no wayfinding operation reads it. A label
  contradicts 1.3.2's wording; using the native assignee for a persona would need wayfinder's text changed.
- **Where behaviour conflicts:** personas alone cannot resolve grilling or prototype tickets (human in the loop);
  research and unattended task tickets fit as written. A meeting that closes several tickets breaks
  one-ticket-per-session unless the map's Notes override it. Notes are the one sanctioned extension point.
- **Ir as precedent:** 26 of 89 tickets carry `Persona: Name (owner)[, Name (role)]`, all task or research,
  defined only in the map's Notes. Friction seen: owner and claim conflated, names and dates written into
  `Status:`, no way to name the user as assignee, authorisation recorded only as prose.
- **Comparable tools:** Backlog.md (free-string assignee), beads (`--claim`), Spec Kit (no owner field). None
  plans as a map of decision tickets with fog.
- Not verified: whether GitHub or GitLab can assign a non-account name; whether re-running setup preserves edits
  to the tracker doc.
