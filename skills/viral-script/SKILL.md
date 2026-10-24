---
name: viral-script
description: Write Instagram content that follows the user's playbook, in the user's voice. For a reel: 3 scripts on 3 different hook formulas (spoken first line, on-screen line, beats, length, CTA). For a carousel: 3 slide-by-slide outlines (cover, slides, recap, CTA). Runs each draft through quality gates that kill weak hooks, recommends one, and logs the choice so later audits can check whether the rules worked. Use when the user says "תכתוב לי תסריט לריל", "תכתוב לי קרוסלה", "רעיון לפוסט על", "איך לפתוח את הריל", "write a reel script", "write a carousel", "give me hooks for", or after viral-playbook or a trend alert.
---

# viral-script

Write content the user can produce today and say out loud without rewriting. Each piece applies rules from the playbook, and says which.

## Preflight

1. Read `viral-instagram/profile.md` (especially Voice, My positions, Proof I can use) and `viral-instagram/playbook.md`. No playbook → say the drafts will rest on the hook library only, offer to build the playbook first, and continue only if the user wants to.
2. Get the topic and the format (reel or carousel). If the format is not given, recommend one: carousel when the idea has steps or needs re-reading (a framework, a checklist, before/after); reel when it needs motion, a face or a payoff seen happening. One single claim → reel. If the user has no topic, offer 3 from the playbook's "To test next", the user's positions, and the latest file in `viral-instagram/trends/`.
3. If the idea is thin, ask one batched question before writing: what happened, to whom, and what did it cost or return. A post needs one specific true thing.

## Reel: 3 scripts

Three **different** hook formulas (`references/hooks.md`), chosen from the playbook's Reels chapter (prefer "proven for you"; at most one "to test next").

```markdown
## Script <n>: H<id> <name>
length: <seconds, from the playbook; default 15-45>
format: <talking head / screen demo / ...>
rules applied: <playbook rule numbers>

**Spoken first line (0-2s):** <exact words>
**On screen at 0s:** <≤ 6 words>
**First frame:** <what moves or changes, not a static face>

**Beats:**
1. (2-7s) the stake: why this matters to the viewer
2. ...
3. ...

**Payoff + CTA (last 3s):** <delivers the hook's promise, then one ask matching the profile>
**Loop:** <last line echoes a word from the hook>
**Caption first line:** ...
```

- Spoken language, the way this user talks (Voice section). Short lines, no sentence they would need to rehearse.
- One idea per beat; the frame changes every beat. A beat longer than ~4 seconds of speech gets split.

## Carousel: 3 outlines

Three **different** hook formulas, chosen from the playbook's Carousels chapter.

```markdown
## Carousel <n>: H<id> <name>
slides: <6-10 unless the playbook says otherwise>
format: <text slides / screenshots / ...>
rules applied: <rule numbers>

1 COVER   <≤ 6 words, big> + <one line of promise>
2 STAKE   <one sentence: why this matters. Works alone: Instagram may show the carousel again starting here>
3..N      <3-7 word headline + ≤ 25 words under it, one idea per slide>
N+1 RECAP <the whole thing as a list: the slide people screenshot and send>
LAST CTA  <one action matching the profile>

visual: <one line per slide>
every slide: number (3/8) + the handle, small, bottom corner
caption first line: ...
```

- Cover text inside the middle of a 1080×1350 frame, clear of the outer 120 px, so the grid crop never cuts it.
- Type no smaller than 32 px at 1080 wide.
- A slide that needs a paragraph is two slides.

## Both formats

- Each beat or slide gives something usable: a step, a number, an example. One that only promises is cut.
- Respect "Won't do".
- **Never invent** a result, number, client or story. Only what is under "Proof I can use" may appear. Anything else is a slot: `[המספר שלך]`.

## Quality gates (not a prediction)

Run every draft through these. A draft that fails a gate is rewritten before it is shown.

| gate | passes when |
|---|---|
| no warm-up | the first words are the hook: no greeting, no name, no "היום אני רוצה לדבר על", no "תעצרו לגלול" |
| concrete | the hook has a number, a name, an object or a specific situation |
| short | spoken hook ≤ ~12 words; on-screen / cover ≤ 6 words |
| only you | the hook uses something from this user's proof, positions or story; another creator couldn't say it word for word |
| promise kept | the hook's promise is delivered before the halfway point |
| one ask | exactly one CTA |

Then show a table: draft · hook id · gates passed · which playbook evidence backs it ("proven for you" beats "proven in niche"). Recommend one, and give one change that would make it stronger.

Say plainly, once: "השערים מסננים הוקים חלשים. מי מבין השלושה ינצח, יגידו המספרים אחרי הפרסום." Never claim a draft will go viral.

## Save and log

Save drafts to `viral-instagram/scripts/YYYY-MM-DD-<format>-<topic-slug>.md`.

When the user picks one, append a row to `viral-instagram/log.md` (`references/log-template.md`): date, format, hook id, rules applied, first line or cover. my-content-audit fills the results later.

## Pre-flight before finishing

- [ ] 3 different hook formulas, each traced to the playbook chapter of the right format (or the library when there is no playbook)
- [ ] Every draft passes all gates
- [ ] Written in the user's voice; nothing from "words I never say"
- [ ] No invented facts; slots where proof is missing
- [ ] Carousel: cover ≤ 6 words, stake slide stands alone, recap slide, numbering and handle noted
- [ ] Table + one recommendation + one fix + the "numbers decide" line
- [ ] Chosen draft logged in log.md
