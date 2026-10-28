# Finding reference accounts

The user does not have to know who their competitors are. Read their own account first, confirm what you understood, then find similar accounts on the web and bring a short list for approval.

## 1. Read the user's own account (Composio, official)

- `INSTAGRAM_GET_USER_INFO` with `ig_user_id: "me"` and fields `username,name,biography,website,followers_count,media_count,account_type`.
- Captions of the last 30 posts (`INSTAGRAM_GET_IG_USER_MEDIA`, fields from `composio.md`).
- If `website` is set, open it with the web fetch tool and read the headline and the offer.

From these, infer:
- **niche:** what the account is about, in the user's words where possible
- **audience:** who it speaks to, and what problem of theirs it solves
- **market:** country and language of the content
- **offer:** what the user sells, if anything is visible

Each inference needs a source: a line from the bio, a repeated caption theme, the website headline. If the account has fewer than 6 posts with captions and no clear bio, say the signal is thin and fall back to the two open questions in setup.

## 2. Confirm before searching

Tell the user in 2-3 short lines what you saw and where, for example: "קראתי את הפרופיל שלך. לפי הביו והפוסטים האחרונים, אתה מלמד בעלי עסקים קטנים בישראל לעבוד עם כלי AI, ומוכר סדנאות." Then ask **[choice]**: נכון / נכון, עם תיקון / לא, אסביר בעצמי. A correction goes in chat. The search itself is offered separately, once the user agreed to niche data (setup question 8, or niche-research preflight).

## 3. Search the web for creators, then their handles

Use the web search tool, not Composio, for discovery. Search engines index few Instagram pages, so `site:instagram.com` queries usually come back empty (tested 28/10/2026). Search for the **people** first, then get their handle from their own pages.

**Find creators** (6-10 searches, in the market's language and in English, built from the confirmed niche, audience and market):
- "<niche> <market>" with words like מדריך, קורס, סדנה, פודקאסט, course, coach, creator
- lists and roundups: "<niche> instagram accounts <market>", "<נישה> משפיענים", "who to follow <niche>"
- LinkedIn, Gumroad, YouTube, podcast and course pages of people who teach or sell to the same audience
- people, brands or tools the user's own captions and website mention

**Get the handle:** open the creator's own page with the web fetch tool (website, Linktree, Gumroad, YouTube about, LinkedIn) and take the Instagram link from it (`instagram.com/<handle>`). A search result URL of the form `instagram.com/<handle>/` also counts. Skip `/p/`, `/reel/`, `/explore/` and hashtag URLs. Never guess a handle from a name: no link on a page → leave the person out.

## 4. Vet each candidate (web only)

From the creator's pages and any search snippet:
- **Same audience?** They speak to the user's audience, not just the same topic.
- **Active?** Something published this year (posts, videos, a course page, an article).
- **Size:** follower count if a snippet or page shows it, marked `~`; otherwise "לא ידוע". Prefer accounts within about 10x of the user's followers; keep 2-4 much larger ones for format only.
- **Format and market:** they make short video or carousels, in the user's language and market (format-only accounts may be foreign).

Drop anything that fails two checks. niche-research's first call on each approved account confirms it exists and gives the real follower count; an account that errors there is reported and dropped.

## 5. Bring the list for approval

Aim for 10-16 candidates in three groups: direct (same niche, a bit ahead), adjacent (same audience, different niche), format only (much larger). Present them through **[choice, multiSelect]**, up to 4 accounts per question and up to 4 questions per call. Each option: the handle as the label; the description holds the group, `~followers`, and one line on why it fits. After the picks, show the source link for any account the user asks about.

Expect small, non-English markets to yield fewer confirmed handles: many creators never link their Instagram from their own pages. Say how many people were found and how many handles were confirmed, so a short list reads as a web limit, not as an empty niche.

Save only what the user picked, as bare usernames without @. Under 8 picked → offer one more search round on the weakest group, or continue with what there is and say the research will be thinner.

## When there is no web search

Say so in one line and ask the user for 8-20 accounts in chat, as before.
