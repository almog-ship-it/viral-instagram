---
name: viral-setup
description: First-run setup for the viral-instagram kit. Checks that Composio and Instagram are connected (Gmail and Apify optional), walks the user through connecting what is missing, explains where niche data comes from, then asks the onboarding questions (business, formats, voice, positions, proof) and writes the profile every other viral-instagram skill reads. Use when the user says "תגדיר לי את ערכת האינסטגרם", "בוא נתחיל", "setup viral instagram", "connect my instagram", "תשנה את הפרופיל", or when any other viral-instagram skill finds no profile.
---

# viral-setup

Prepare everything the other five skills need: live connections and a filled profile. Run once per user; rerun to change answers.

Talk to the user in plain, friendly language, in Hebrew unless the user writes in another language. Never show tool names, file paths or JSON unless asked.

## Asking questions

When the `AskUserQuestion` tool is available, use it for every question below marked **[choice]**: 2-4 options, the recommended one first with "(מומלץ)" when there is a clear default, `multiSelect` where noted. The tool adds an "other" field on its own, so never add one. Batch independent choice questions into one call (up to 4). Questions marked **[open]** stay as normal chat messages, one per message. Without the tool, ask everything in chat, one question per message, and offer the same options as examples.


## Where things live

All kit data lives in a folder named `viral-instagram/` in the user's workspace folder:

```
viral-instagram/
  profile.md          ← this skill
  log.md              ← one row per planned/published post (viral-script, my-content-audit)
  audit/              ← my-content-audit
  research/           ← niche-research
  playbook.md         ← viral-playbook
  scripts/            ← viral-script
  trends/             ← trend-watch
```

Create the folder if it does not exist.

## Step 1 — Check Composio is installed

Look for the Composio tools (`COMPOSIO_SEARCH_TOOLS`, `COMPOSIO_MANAGE_CONNECTIONS`). If they are not available, stop and explain, using `references/composio.md` → "Installing the Composio connector". Resume once the user says it is done.

## Step 2 — Connections

`COMPOSIO_MANAGE_CONNECTIONS` with action `list` for `instagram`, `gmail`, `apify`.

| Toolkit | Needed for | If missing |
|---|---|---|
| instagram | everything (required) | connect now |
| gmail | trend alerts by email (optional) | ask in question 9 |
| apify | reel transcripts in research (optional) | mention once: "יש תוסף אופציונלי לתמלול רילז. בלי זה ננתח הוקים מהכיתוב. אפשר להוסיף אחר כך." Don't push it. |

Connect anything missing with `references/composio.md` → "Connecting a toolkit". Continue only when Instagram is ACTIVE.

## Step 3 — Check the Instagram account

`INSTAGRAM_GET_USER_INFO` (`ig_user_id: "me"`, fields `username,account_type,followers_count,media_count`), then one page of `INSTAGRAM_GET_IG_USER_MEDIA` (limit 100, fields `id,media_type,media_product_type`) to count reels and carousels.

- Personal account → explain the switch (`references/composio.md` → "Personal account"), then reconnect.
- Under 1,000 followers → say once: "יש לך פחות מ-1,000 עוקבים, אז אינסטגרם לא נותן את הנתונים העמוקים. ננתח לפי צפיות, שמירות, שיתופים ותגובות. זה מספיק כדי למצוא דפוסים." Record `deep_insights: no`.
- Under 8 posts in a format → record it; the audit will lean on niche research for that format.

## Step 4 — Niche data disclosure

Read the disclosure in `references/composio.md` → "Disclosure" to the user, in their language, and ask if they want niche research and trend alerts **[choice]**: "כן, גם מחקר נישה וטרנדים" / "רק הנתונים שלי". Record `niche_data: yes | no`. Respect a no without arguing.

## Step 5 — Onboarding questions

Open questions one per message, with examples so a non-technical user can answer fast. Choice questions in batches (see "Asking questions"): after question 2, ask 3 and 4 together; after question 8, ask 9 and 10 together.

1. **[open]** מה העסק שלך ובמה אתה עוזר? (משפט אחד)
2. **[open]** מי הקהל שאתה רוצה שיראה את התוכן? (מי הם, מה כואב להם)
3. **[choice]** על אילו פורמטים לעבוד? Options: רילז / קרוסלות / שניהם. Put the counts in the option descriptions and mark the one the counts suggest as recommended ("ראיתי 34 רילז ו-12 קרוסלות").
4. **[choice, multiSelect]** מה המטרה? Options: עוקבים / לידים / מכירות. Then, only if their posts don't already show it, **[choice]** מה הצופה עושה בסוף? Options: מגיב מילה ומקבל משהו בפרטי / שומר את הפוסט / לוחץ על הלינק בביו / עוקב. If their captions already show one ending (for example a trigger word in most of them), state it and skip the question.
5. **[open] הקול שלך:** "תבחר 3 פוסטים שלך שהכי מרגישים כמוך, ותשלח לי קישור או תכתוב את המספר שלהם מהרשימה." Read their captions (and slides / transcript if available), infer how the user talks: pace, words they use, words they would never say, emoji habit, face or faceless. Show the inference in 3 lines, then **[choice]** "זה אתה?": נכון / נכון, אבל אני רוצה לתקן משהו / לא, בוא נעשה את זה בשאלות. If the user wants to fix or redo it, ask in chat.
6. **[open] עמדות:** "2-3 דברים שאתה מאמין בהם ושחלק מהקהל שלך לא מסכים איתם." These are where the best hooks come from. Fine to skip.
7. **[open] הוכחות:** "מספרים, תוצאות או סיפורים אמיתיים שמותר לי להשתמש בהם בתסריטים. אם אין, נשאיר מקום ריק והמספר יגיע ממך." Never invent one later; only what is written here may appear in a draft.
8. **[open]** If `niche_data: yes`: 10-20 חשבונות שדומים לך או שאתה מעריך. אם אין לך רשימה, אציע. Propose a mix: about 4 direct (same niche, a bit ahead), 4 adjacent (same audience, different niche), 2-4 much larger accounts for format only. Prefer accounts within about 10x of the user's follower count. The user confirms each one; never add an account they did not approve. When you propose accounts, use **[choice, multiSelect]** (up to 4 accounts per question, follower count in each description). Include carousel-heavy accounts when carousels are in scope.
9. If `niche_data: yes`: **[choice]** התראה על טרנדים: כל שבוע / כל שבועיים / בלי. And **[choice]** איך לקבל אותה: מייל / רק קובץ בתיקייה. (Connect Gmail now if they chose email.)
10. **[choice, multiSelect]** מה אתה **לא** מוכן לעשות בתוכן? Options: לרקוד / לצלם פנים / ליפסינק / סאונדים טרנדיים. The free-text field covers anything else.

## Step 6 — Write the profile

Write `viral-instagram/profile.md` from `references/profile-template.md`. Create an empty `viral-instagram/log.md` with the header from `references/log-template.md`. Read the profile back in 5 short lines and ask if anything is wrong **[choice]**: הכל נכון / יש מה לתקן.

## Step 7 — Hand off

End with one next action **[choice]**: "השלב הבא: לנתח את התוכן שלך. להתחיל?" כן, להתחיל / אחר כך. Yes → my-content-audit.

## Pre-flight before finishing

- [ ] Instagram ACTIVE and the account is professional
- [ ] Disclosure read; `niche_data` recorded
- [ ] Every question answered or marked skipped; at least one format chosen
- [ ] Choice questions asked through `AskUserQuestion` when it was available
- [ ] Voice inference confirmed by the user
- [ ] "Proof I can use" contains only what the user stated
- [ ] Reference accounts confirmed one by one, bare usernames without @
- [ ] profile.md and log.md written; profile read back
