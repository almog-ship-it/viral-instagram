# Composio — connection guide

All external access in this kit goes through the Composio connector. Every Composio call takes a session: start with `session: {generate_id: true}` on the first `COMPOSIO_SEARCH_TOOLS` call and reuse the returned id. Tools run through `COMPOSIO_MULTI_EXECUTE_TOOL`. Large results are saved to a remote file; parse them with `COMPOSIO_REMOTE_WORKBENCH` rather than reading them inline.

## Installing the Composio connector

Tell the user, in plain words:

1. נכנסים ל-composio.dev ופותחים חשבון חינמי.
2. ב-Claude: הגדרות → Connectors → מוסיפים את Composio (או מדביקים את כתובת ה-MCP שקיבלת בדשבורד של Composio).
3. מאשרים, ופותחים שיחה חדשה.
4. כותבים לי "חיברתי את קומפוזיו".

## Connecting a toolkit

1. `COMPOSIO_MANAGE_CONNECTIONS` → `{toolkits: [{name: "<toolkit>", action: "add"}]}`.
2. Show the returned `redirect_url` as a markdown link: `[לחץ כאן לחיבור <name>](url)`.
3. Tell the user what they will see:
   - **instagram**: מתחברים עם חשבון האינסטגרם המקצועי ומאשרים את ההרשאות.
   - **gmail** (optional): מתחברים עם חשבון הגוגל שאליו יגיעו ההתראות.
   - **apify** (optional, transcripts only): נרשמים ל-Apify בחינם, ובחיבור מדביקים את ה-API token מ-Settings → API & Integrations.
4. Wait for the user. Then `action: "list"` and continue only when status is `ACTIVE`.

`just_one_api` needs no connection of its own: it runs on the user's Composio account and is billed there (about $0.015 per call).

Never ask the user to paste a token into the chat.

## Personal account

The Instagram API works only with Business or Creator accounts. Tell the user:
באפליקציה: פרופיל → ☰ → הגדרות → סוג חשבון וכלים → מעבר לחשבון מקצועי → Creator. זה חינם, לוקח דקה, ולא משנה כלום לעוקבים. אחר כך מחברים שוב.

## Disclosure (say once, in setup, and in every first research run)

הנתונים על החשבונות שלך מגיעים מהחיבור הרשמי של אינסטגרם. הנתונים על חשבונות של אחרים מגיעים מכלי חיצוני לא רשמי שקורא רק מידע ציבורי: הוא לא מתחבר לחשבון שלך, לא עושה שום פעולה בשמך, ולא רואה שום דבר שלא גלוי לכל מי שנכנס לפרופיל. אינסטגרם לא מאשרת רשמית איסוף כזה. אם זה לא מתאים לך, אפשר לדלג על מחקר הנישה והטרנדים ולעבוד רק עם הנתונים שלך.

Record the answer as `niche_data: yes | no` in the profile. If `no`, niche-research and trend-watch explain that they are off and stop.

## Own account — Instagram toolkit

| Slug | Used by | Notes |
|---|---|---|
| `INSTAGRAM_GET_USER_INFO` | setup | `ig_user_id: "me"` |
| `INSTAGRAM_GET_IG_USER_MEDIA` | audit | `ig_user_id: "me"`, `limit: 100`; rows under `data.data`, cursor under `data.paging.cursors.after` |
| `INSTAGRAM_GET_IG_MEDIA_INSIGHTS` | audit | `metric` must be an array; requires 1,000+ followers |
| `INSTAGRAM_GET_IG_MEDIA_CHILDREN` | audit | slides of a carousel: `media_url` per slide |
| `INSTAGRAM_GET_USER_INSIGHTS` | audit (profile check) | account-level, see below |
| `GMAIL_SEND_EMAIL` | trend-watch | `recipient_email` = the address in the profile |

Media fields: `id,caption,media_type,media_product_type,permalink,timestamp,like_count,comments_count`. Keep the list this short: asking for the extra count fields (`view_count`, `saved_count`, `shares_count`, ...) can make the response come back without `caption`. Views, saves and shares come from `INSTAGRAM_GET_IG_MEDIA_INSIGHTS` instead. Reels: `media_product_type = REELS`. Carousels: `media_type = CAROUSEL_ALBUM`.

Insight metrics:
- Reels: `["views","reach","saved","shares","likes","comments","total_interactions","ig_reels_avg_watch_time","reels_skip_rate"]`
- Carousels: `["views","reach","saved","shares","likes","comments","total_interactions"]`

If a call returns 400, retry once with a shorter list (drop the last two); if it still fails, keep the row and mark insights unavailable.

Account-level, for the profile check (last 30 days, `metric_type: "total_value"`):
- `reach` with `breakdown: "follow_type"` → share of reach from non-followers
- `profile_views`, `follows_and_unfollows` (or `follower_count` trend), `website_clicks`

Each may come back empty for small accounts; empty is valid, not an error.

## Other accounts — just_one_api

| Slug | Input | Notes |
|---|---|---|
| `JUST_ONE_API_INSTAGRAM_GET_USER_DETAIL` | `username` | profile, follower count |
| `JUST_ONE_API_INSTAGRAM_GET_USER_POSTS` | `username`, `pagination_token` | 12 posts per call; pass the returned `pagination_token` for the next page |

Items are under `data.data.data.items` (three `data` levels) and the next-page token under `data.data.pagination_token`. Check the item count on the first page before paging: a wrong path reads as an empty account and still costs the call. Fields (confirm on the first run; log any that are missing):
- `product_type`: `clips` = reel · `carousel_container` = carousel · `feed` = single image
- `play_count` (reel views; `ig_play_count` as fallback), `like_count`, `comment_count`
- `caption.text`, `taken_at` (unix) or `taken_at_date`, `code` → link `https://www.instagram.com/p/<code>/`
- `video_duration` (reels), `carousel_media_count` (carousels)
- slide images: `carousel_media[i].image_versions.items[0].url`
- **Skip pinned posts**: `is_pinned` true or `timeline_pinned_user_ids` non-empty. They are old and distort the baseline.

Carousels expose no views here either: only likes and comments.

Cost: about $0.015 per call. 30 posts per account ≈ 3 calls ≈ $0.045. Tell the user the estimate before the first run of any study.

## Transcripts — Apify add-on (optional)

Only if `apify: active` in the profile. `APIFY_RUN_ACTOR_SYNC_GET_DATASET_ITEMS`, actor `apify/instagram-reel-scraper`, `{"username": [<reel URLs>], "includeTranscript": true}`. About $0.05 per minute of audio. Use only on short lists of outlier reels. Without it, reel hooks are tagged from captions (`tagged_from: caption`).

## Reading slides

Download each slide image (from `INSTAGRAM_GET_IG_MEDIA_CHILDREN` or `carousel_media`) into `viral-instagram/.cache/<post id>/<n>.jpg` and view the files. Instagram URLs expire within hours: download in the same run that fetched them. Delete the cache folder at the end of the run.
