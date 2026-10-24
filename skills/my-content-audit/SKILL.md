---
name: my-content-audit
description: Analyse the user's own Instagram reels and carousels through Composio: pull every post with its numbers, tag each one by hook, format and topic (reading carousel slides as images), and compare the top performers to the bottom ones within each format to show what works for this account and what doesn't. Use when the user says "מה עבד לי באינסטגרם", "תנתח את הרילז שלי", "תנתח את הקרוסלות שלי", "למה הפוסט הזה נכשל", "analyse my reels", "analyse my carousels", "what worked on my instagram", or after viral-setup.
---

# my-content-audit

Find what separates this user's best posts from the worst ones, **separately for each format**. Instagram distributes reels and carousels differently, so never rank a carousel against a reel. The user's own data is the only proof of what works on their audience, so it outweighs anything found in other accounts.

Talk to the user in the profile language. Keep the chat short: findings go in the report file.

## Preflight

1. Read `viral-instagram/profile.md`. If missing → run viral-setup first. Analyse only the formats listed under `## Formats`.
2. Confirm the Instagram connection is ACTIVE (`COMPOSIO_MANAGE_CONNECTIONS` list). If not → follow `references/composio.md`.

## Step 1 — Pull the posts

`INSTAGRAM_GET_IG_USER_MEDIA` with `ig_user_id: "me"`, `limit: 100` and the fields in `references/composio.md`. Paginate until no cursor. Split into reels (`media_product_type = REELS`) and carousels (`media_type = CAROUSEL_ALBUM`). Drop posts younger than 7 days: their numbers are still moving.

If `deep_insights: yes`, call `INSTAGRAM_GET_IG_MEDIA_INSIGHTS` per post with the metric list for its format. Normalize each metric list into name → value.

If a format has fewer than 8 posts, say so: "יש רק X קרוסלות, זה מעט מדי למסקנות. אנתח מה שיש ואסמן את זה כחלש." Continue anyway.

## Step 2 — Score (within each format)

Per post:
- `views` = `views` insight, else `total_views_count`, else `view_count`
- `share_rate` = shares / views, `save_rate` = saved / views, `comment_rate` = comments / views
- `relative_views` = views / median views of the user's posts **in the same format**

Composite score:
- **Reels**: 0.4 × relative_views (capped at 5) + 0.3 × share_rate percentile + 0.2 × save_rate percentile + 0.1 × comment_rate percentile. When `ig_reels_avg_watch_time` exists, add avg watch time ÷ length as a separate column; it is the best single signal of a good hook.
- **Carousels**: 0.4 × save_rate percentile + 0.3 × share_rate percentile + 0.2 × relative_views (capped at 5) + 0.1 × comment_rate percentile. Saves lead because a carousel is built to be kept.

Rank, then take the top 25% and the bottom 25% of each format.

## Step 3 — Read the content of the top and bottom groups

Only the top and bottom groups, to keep cost and time down.

- **Reels**: if `apify: active`, get transcripts with the Apify add-on (`references/composio.md` → "Transcripts"); tell the user the estimate first. Otherwise tag from captions and say so once in the report.
- **Carousels**: `INSTAGRAM_GET_IG_MEDIA_CHILDREN` per carousel, download and view the slides (`references/composio.md` → "Reading slides"). No Apify needed.

## Step 4 — Tag

Tag each top and bottom post with `references/tagging.md`, using the fields for its format. Hooks get an id from `references/hooks.md`.

## Step 4b — Profile check (hook problem or profile problem?)

`INSTAGRAM_GET_USER_INSIGHTS` for the last 30 days (`references/composio.md` → account-level metrics):
- `non_follower_share` = non-follower reach ÷ total reach
- `profile_conversion` = new follows ÷ profile views (use `follows_and_unfollows`; if empty, the `follower_count` change over the window)

Read it like this:
- Posts reach non-followers well (`non_follower_share` ≥ 50%) but `profile_conversion` is low (under ~5%) → **the content travels, the profile doesn't convert.** Say it first in the report: fix the bio, the pinned posts and the link before writing more hooks. Point at what to look at; don't rewrite the bio here.
- Low non-follower reach → the hooks aren't travelling; the rest of the audit applies.
- Metrics empty (small account) → skip this step and say so in one line.

## Step 4c — Close the loop with log.md

Read `viral-instagram/log.md`. For every row without results that is 7+ days old, match it to a post (by date and first line or caption), fill `link`, `views`, `saves`, `shares`, `rel.` and `verdict`. Then report in one short section: how did posts built on playbook rules do compared with the rest? With fewer than 5 logged posts, list them and don't conclude.

## Step 5 — Compare

Within each format, look for differences between the top and bottom groups, with counts: hook types, formats, topics, CTA, first line; plus length for reels and slide count / swipe driver / words per slide for carousels. A finding needs at least 3 posts behind it; anything with fewer is written as "אות חלש".

Outliers: any post with relative_views ≥ 3 gets its own short paragraph on why it may have broken out.

If both formats are in scope, add one cross-format note **about topics only** (e.g. "הנושא X עבד גם ברילז וגם בקרוסלות"). Never compare raw numbers across formats.

## Step 6 — Write the report

Write `viral-instagram/audit/YYYY-MM-DD.md`:

```markdown
# Content audit — <date>
data: deep | basic · hooks tagged from: transcript | caption

## Profile check
non-follower share: N% · profile views → follows: N% · verdict: <hook problem | profile problem | not enough data>

## Did the playbook work? (from log.md)
| first line | rules | rel. | verdict |

## Reels
posts analysed: N · window: <first> – <last>
### Top 5 / Bottom 5
| # | first line | hook | format | views | rel. | share% | save% | link |
### What the top reels share
- <finding> — <n of m top vs n of m bottom>
### What the bottom reels share
### Outliers
### Weak signals

## Carousels
posts analysed: N · window: ...
### Top 5 / Bottom 5
| # | slide 1 headline | hook | slides | swipe driver | views | save% | share% | link |
### What the top carousels share
### What the bottom carousels share
### Outliers
### Weak signals

## Across formats (topics only)

## Data table
<every post: format, date, link, views, rates, tags>
```

Leave out a format section that is not in the profile.

In chat: up to 3 findings per format, each one line, then one next action: "השלב הבא: לחפש מה עובד בחשבונות שבחרת (niche-research). להתחיל?"

## Pre-flight before finishing

- [ ] No ranking or comparison mixes reels and carousels
- [ ] No finding rests on fewer than 3 posts without the "אות חלש" label
- [ ] Every number comes from the API, none estimated
- [ ] Posts under 7 days old excluded
- [ ] Profile check done (or skipped with a reason) and stated before the hook findings when it says "profile problem"
- [ ] log.md rows older than 7 days filled
- [ ] Slide cache deleted
- [ ] Chat summary ≤ 3 findings per format + one next action
