---
name: viral-playbook
description: Merge the user's own content audit and the niche research into one playbook of rules for making Instagram reels and carousels that work for this account, with a chapter per format: hooks, formats, structures, length or slide count, CTA, and what to stop doing. Every rule cites the posts behind it. Use when the user says "תבנה לי ספר כללים", "מה הכללים לריל טוב אצלי", "מה הכללים לקרוסלה", "תסכם את המחקר", "build my playbook", "update the playbook", or after niche-research.
---

# viral-playbook

Turn two studies into one page of rules the user (and viral-script) can follow. The playbook is the source of truth; viral-script and trend-watch read it.

## Preflight

Read `viral-instagram/profile.md`, the latest `viral-instagram/audit/*.md`, the latest `viral-instagram/research/*.md` and `viral-instagram/log.md`. A playbook rule whose logged posts came out `below` three times or more gets demoted to "Stop doing" or rewritten, and the playbook says why. If either study is missing, say which one and offer to run it, through `AskUserQuestion` when available (run it now first, then "לבנות רק ממה שיש"), otherwise in chat. If only one exists, build from it and mark the playbook `based_on: audit only` or `research only`.

## Weighing evidence

Within each format:
1. **Proven for you**: a pattern that is in the user's top posts. Strongest.
2. **Proven in the niche, untested by you**: a research pattern the user hasn't tried. Worth testing.
3. **Works in the niche, failed for you**: a research pattern that sits in the user's bottom posts. Say so plainly; don't recommend it again without a changed angle.
4. **Weak signal**: under 3 posts. Listed separately, never as a rule.

When the audit and the research disagree, the audit wins for this account. Carousel niche evidence rests on likes and comments only; mark rules built on it alone as `level: niche (engagement only)`.

A topic that won in both formats may appear in both chapters. Never carry a rule about form (hook style, length, layout) from one format to the other without evidence in that format.

## Write the playbook

`viral-instagram/playbook.md` (overwrite; keep the previous version as `viral-instagram/playbook-<old date>.md`):

```markdown
# Instagram playbook — <name>
updated: <date> · based_on: audit <date> + research <date>

## Before anything else
<only if the audit's profile check said "profile problem": one line telling the user to fix the profile first, and why>

## Topics that work (both formats)
- <topic> — <evidence>

## Reels
### The 5 rules
1. <rule in one imperative sentence> — <evidence: n posts, links> — <level>
### Hooks that work here
| hook (H-id) | example first line (rewritten for your topic) | evidence |
### Formats and structures
### Length and pacing
### CTA
### Stop doing
### To test next
### Weak signals

## Carousels
### The 5 rules
### Slide 1 headlines that work here
| hook (H-id) | example headline (rewritten for your topic) | evidence |
### Formats and structures
### Slide count, words per slide, swipe drivers
### CTA (last slide / caption)
### Stop doing
### To test next
### Weak signals
```

Leave out a chapter for a format not in the profile. Fewer than 5 rules is fine when the evidence is thin; never pad.

Rules are written so a person who has never seen the data can follow them: concrete, with an example line, no jargon.

In chat: the rules per format, then: "השלב הבא: לכתוב תוכן לפי הכללים (viral-script). על איזה נושא, ורילז או קרוסלה?"

## Pre-flight before finishing

- [ ] Every rule has evidence with a count and links, from its own format
- [ ] No rule built on a weak signal
- [ ] Conflicts between audit and research resolved in favour of the audit, and noted
- [ ] Nothing in "Won't do" recommended
- [ ] Previous playbook kept
