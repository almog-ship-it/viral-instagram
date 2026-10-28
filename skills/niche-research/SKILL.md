---
name: niche-research
description: Find the reels and carousels that broke out in the user's niche and take them apart. Pulls recent posts of the reference accounts through Composio (just_one_api), flags posts that beat their own account's median by 3x or more within the same format, reads carousel slides as images and (with the optional Apify add-on) reel transcripts, and tags hook formula, format and structure. Use when the user says "מה עובד בנישה שלי", "תמצא לי סרטונים ויראליים", "תמצא לי קרוסלות שעבדו", "תחקור את המתחרים", "find viral reels in my niche", "what carousels work in my niche", or after my-content-audit.
---

# niche-research

Find posts that broke out **relative to their own account and format**, not posts with big numbers. A million views on a huge account is a normal day; 5x the median on a small account is a signal.

Run it monthly, not weekly: formulas last a season. Weekly movement is trend-watch's job.

## Preflight

1. Read `viral-instagram/profile.md`. If missing → viral-setup. Research only the formats under `## Formats`.
2. `niche_data: no` → say niche research is off by the user's choice, offer to turn it on (re-read the disclosure from `references/composio.md`; the choice through `AskUserQuestion` when available: "כן, להפעיל" / "לא עכשיו"), and stop.
3. At least 8 reference accounts. Fewer → ask for more, or suggest some and get approval. When suggesting, use `AskUserQuestion` when available with `multiSelect` (up to 4 accounts per question, follower count in each description); never add an account the user did not pick.
4. First run only: repeat the disclosure in one short line.

## Step 1 — Estimate and confirm cost

Accounts × 3 calls × $0.015 (≈ 30 posts each). If `apify: active`, add transcripts for up to 15 reel outliers (~$0.05 × average minutes). Tell the user the total in one line and wait for yes, through `AskUserQuestion` when available ("כן, להריץ" first; with Apify, also "בלי תמלולים" and its lower total), otherwise in chat.

## Step 2 — Pull posts

Per account: `JUST_ONE_API_INSTAGRAM_GET_USER_DETAIL` once (follower count), then `JUST_ONE_API_INSTAGRAM_GET_USER_POSTS` up to 3 pages (`pagination_token`), stopping early when posts get older than 90 days. Parse in `COMPOSIO_REMOTE_WORKBENCH` when the result is saved to a file. Field mapping: `references/composio.md` → "Other accounts".

- Skip pinned posts.
- Keep reels (`product_type = clips`) and carousels (`carousel_container`), per the formats in scope.
- An account that errors or is private: note it and move on.

## Step 3 — Find outliers (per account, per format)

Account baseline needs ≥ 8 posts in that format; otherwise skip the account for that format and say so.
- **Reels**: `ratio` = `play_count` / the account's median reel `play_count`.
- **Carousels**: no public views, so `ratio` = (likes + 3 × comments) / the account's median of the same. Weaker signal than views; the report says so.

Outlier = ratio ≥ 3. Drop posts under 3 days old. Keep the top 15 outliers per format, at most 3 per account.

## Step 4 — Read and tag

- **Carousels**: download and view each outlier's slides (`references/composio.md` → "Reading slides").
- **Reels**: with the Apify add-on, transcripts for the outlier URLs; without it, tag from the caption (`tagged_from: caption`).

Tag each with `references/tagging.md`; hooks get an id from `references/hooks.md`, or `unclassified` with the line quoted.

## Step 5 — Patterns

Within each format, count tags across the outliers. A pattern needs at least 3 outliers from at least 2 different accounts. For each pattern:
- what it is, in one sentence
- example posts (links)
- the first line or slide 1 headline of each, verbatim
- why it might work for this audience (one line, labelled as interpretation)
- **your version**: one hook line in the user's topic and voice, using only their approved proof (`[המספר שלך]` where a number is missing)

Read the `unclassified` hooks by hand: two of the same shape may be a formula missing from the library. Name it in the report.

Check fit against the profile: drop or flag anything in "Won't do".

## Step 6 — Write the report

`viral-instagram/research/YYYY-MM-DD.md`:

```markdown
# Niche research — <date>
source: public data via just_one_api · hooks tagged from: transcript | caption

## Reels
accounts: N scraped, N skipped · reels: N · outliers (≥3x views): N
### Patterns
#### <name> (H<id>)
seen in: N outliers, N accounts
examples: <links + first lines>
interpretation: ...
your version: "..."
fits you: yes | no (<reason>)
### The outliers
| account | ratio | views | hook | format | structure | length | first line | link |
### Unclassified hooks
...

## Carousels
accounts: N · carousels: N · outliers (≥3x engagement): N
note: ratio uses likes and comments; public carousels show no views or saves.
### Patterns
### The outliers
| account | ratio | likes | comments | hook | format | slides | swipe driver | recap | slide 1 headline | link |

## Accounts
| account | followers | reels median views | carousels median engagement | outliers R | outliers C |

## Confidence
<N posts across N accounts. Say plainly whether that supports the patterns or only hints at them.>
```

Leave out a format section that is not in the profile.

In chat: the 3 strongest patterns per format, one line each with "your version", then: "השלב הבא: לאחד את זה עם הניתוח שלך לספר כללים (viral-playbook). להתחיל?"

## Rules

- Analyse technique, never copy content. First lines and headlines are quoted only as evidence in the private report; "your version" is always rewritten from the user's own story.
- Never report a "viral" post without its ratio.
- Never compare reel and carousel numbers.

## Pre-flight before finishing

- [ ] User approved the cost estimate before the first run
- [ ] Pinned posts excluded
- [ ] Every outlier has a ratio and its account baseline, within its own format
- [ ] Every pattern has ≥ 3 outliers from ≥ 2 accounts, and a "your version" line with no invented facts
- [ ] Carousel section states that its ratio uses likes and comments
- [ ] Confidence section filled
- [ ] Slide cache deleted
