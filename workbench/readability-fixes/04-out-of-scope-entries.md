# 04 — "Out of scope" entries have no format

## File

`skills/engineering/wayfinder/SKILL.md` — line 101

## Instruction

"leave one line in the **Out of scope** section: the gist plus why it's out of scope, linking the closed ticket"

## Problem

The instruction is vague — "the gist plus why" gives the model too much freedom. Without a format constraint, entries can become:
- Multi-clause sentences that are hard to parse
- Inconsistent with the style of "Decisions so far" entries
- Missing a clear one-line verdict

## Plan

Add a template:

```markdown
- [<closed ticket title>](link) — <one-line explanation of why this sits beyond the destination>
```

And note: entries here are scope boundaries, not decisions — the language should reflect that ("ruled out because...", "beyond destination because...").
