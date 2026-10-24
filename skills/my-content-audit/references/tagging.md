# Tagging scheme

Every post analysed by the kit, the user's or anyone else's, gets the same tags so the studies can be compared. Fields marked (R) are reels only, (C) carousels only.

## hook: what stops the scroll
For a reel: the first 1-3 seconds. For a carousel: slide 1 (headline + image).
Tag with a formula id from `hooks.md` (H1-H22), or `unclassified` with the line quoted.

## format
- Reel: `talking_head` · `screen_demo` · `b_roll_voiceover` · `text_on_screen` · `skit` · `interview` · `before_after` · `slideshow`
- Carousel: `text_slides` · `screenshots` · `photo_story` · `infographic` · `quote` · `before_after` · `meme` · `tweet_style`

## structure
`problem_solution` · `step_by_step` · `story_lesson` · `list` · `myth_truth` · `reaction` · `tutorial` · `behind_the_scenes` · `checklist` (C) · `framework` (C)

## other fields
- `topic`: 2-4 words
- `length_s` (R): duration in seconds
- `slides` (C): number of slides
- `words_per_slide` (C): rough average
- `swipe_driver` (C): what makes the viewer swipe from slide 1 to 2 (`open_loop` · `numbered` · `cut_sentence` · `arrow_cue` · `none`)
- `recap_slide` (C): yes / no
- `cta_type`: `none` · `follow` · `comment_word` · `save` · `share` · `link`
- `cta_location` (C): `last_slide` · `caption` · `both`
- `audio` (R): `original` · `trending`
- `first_line`: the exact first spoken sentence (R) or slide 1 headline (C)
- `tagged_from`: `transcript` · `slides` · `caption`

## How to tag
- Reels: from the transcript when there is one (Apify add-on), else from the caption and the on-screen text if visible in the thumbnail.
- Carousels: from the slide images themselves (see "Reading slides" in composio.md), plus the caption.
- Never guess the hook from the topic alone; if there is not enough, write `unknown`.
