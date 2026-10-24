---
name: trend-watch
description: Weekly scan for new Instagram trends in the user's niche, for reels and carousels. Pulls the last 7 days from the reference accounts through Composio (just_one_api), finds formats, hooks, slide structures or audio that broke out on 3+ accounts at once, keeps only what fits the user, and sends a short alert with a ready adaptation by email or as a file. Also sets up the recurring schedule. Use when the user says "מה הטרנדים השבוע", "תשלח לי התראה על טרנדים", "תגדיר סריקה שבועית", "what's trending in my niche", "set up trend alerts", or when the scheduled run fires.
---

# trend-watch

A trend is a pattern that breaks out on **3 or more accounts in the same week, in the same format**. One viral post is a fluke; the same move working across accounts is a trend.

## Preflight

1. Read `viral-instagram/profile.md` and `viral-instagram/playbook.md` (playbook optional; without it, adapt from the profile only). Scan only the formats under `## Formats`.
2. `niche_data: no` → trend alerts are off by the user's choice; say so and stop. Gmail ACTIVE only if `delivery: email`.

## Mode A — set up the schedule (first run or when asked)

Use the app's scheduled-task capability to create a recurring task named `viral-instagram trend-watch`, on the profile's frequency (weekly → Sunday 08:00 in the user's timezone; ask the timezone if unknown). Task prompt: "הרץ את trend-watch על הפרופיל שלי ושלח את ההתראה." Confirm the time with the user before creating it. If the app has no scheduling, tell the user to run "מה הטרנדים השבוע" once a week.

## Mode B — the scan

### 1. Pull the week
Per account, one `JUST_ONE_API_INSTAGRAM_GET_USER_POSTS` call (12 newest posts); keep posts from the last 7 days, skip pinned ones (`references/composio.md` → "Other accounts"). Cost: about $0.015 per account. No confirmation needed on scheduled runs once the user approved the schedule.

Baselines: use the account table in the latest `viral-instagram/research/*.md` (reel median views, carousel median engagement). Accounts missing there: pull 3 pages once and save the baseline to `viral-instagram/trends/baselines.md`.

### 2. Weekly outliers
Per format, the same ratio as niche-research (reels: views; carousels: likes + 3 × comments). Weekly outlier = ratio ≥ 2.5 (lower than research because the posts are young).

### 3. Group into trends
Tag outliers with `references/tagging.md` from captions; hooks get an id from `references/hooks.md`. Group within each format:
- Reels: same non-original audio track · same hook formula (H-id) + structure · same format with a recognisable twist · same topic or news event
- Carousels: same hook formula (H-id) + structure · same visual format (e.g. tweet-style slides, screenshots) · same slide-count/swipe pattern · same topic or news event

A group is a trend when it spans ≥ 3 accounts. A topic trending in both formats counts once, noted as "בשני הפורמטים".

Compare with the last 3 files in `viral-instagram/trends/`: mark each trend `new` or `continuing (week N)`. Week 3+ is late; say so.

### 4. Filter for fit
Keep a trend only if it fits the profile's topic and audience, doesn't hit "Won't do", and is in a format the user makes. Keep at most 3 in total. If none survive, the alert says so in one line; that is a valid result.

### 5. Adapt
For each kept trend, read 2 examples: slides for carousels (`references/composio.md` → "Reading slides"); for reels, transcripts if the Apify add-on is active, else captions. Then write, in the user's topic and following the playbook:
- reel: a hook line and a 3-beat outline
- carousel: a slide 1 headline and a 4-6 slide outline

Never invent numbers or stories; leave slots. Delete the slide cache at the end.

### 6. Deliver
Write the full report to `viral-instagram/trends/YYYY-MM-DD.md`:

```markdown
# Trends — week of <date>
accounts scanned: N · reels: N · carousels: N · weekly outliers: N

## <trend name> — <reel | carousel | both> — new | continuing (week N)
what: <one sentence>
seen on: <accounts> · examples: <links>
for you: <hook line or slide 1 headline>
outline: 1. ... 2. ... 3. ...
act by: <date — trends fade in 1-2 weeks>
```

Alert, **5 lines max**, sent by `GMAIL_SEND_EMAIL` to `alert_email` (subject: "טרנדים השבוע: <top trend>"), or shown in chat if `delivery: file`:

```
🔥 ריל: <trend> — <hook line for you>
🔥 קרוסלה: <trend> — <slide 1 headline for you>
(עד 3 טרנדים)
הפירוט המלא בתיקיית viral-instagram/trends
רוצה גרסה מלאה? כתוב "תסריט לטרנד 1"
```

"תסריט לטרנד N" → hand to viral-script with the trend as the topic and its format.

## Pre-flight before sending

- [ ] Every trend spans ≥ 3 accounts in one format, with links
- [ ] Nothing from "Won't do"; only formats the user makes; at most 3 trends
- [ ] Alert ≤ 5 lines; detail only in the file
- [ ] No invented facts in the adaptations
- [ ] Email goes only to the address in the profile
