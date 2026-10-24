# Post log template

`viral-instagram/log.md` holds one row per post that went through the kit. viral-script adds the row when a draft is chosen; my-content-audit fills the link and results. This is how the kit learns whether its own rules work.

```markdown
# Post log

| date | format | hook id | rules applied | first line / slide 1 | link | views | saves | shares | rel. | verdict |
|---|---|---|---|---|---|---|---|---|---|---|
```

- `rules applied`: playbook rule numbers, e.g. `R1,R3` / `C2`
- `link`, `views`, `saves`, `shares`, `rel.` (views ÷ the user's format median): filled by my-content-audit once the post is 7+ days old
- `verdict`: `above` (rel. ≥ 1.5) · `normal` · `below` (rel. ≤ 0.67)
