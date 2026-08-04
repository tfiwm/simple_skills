# 07 — "Not yet specified" fog entries lack format guidance

## File

`skills/engineering/wayfinder/SKILL.md` — lines 86, 126

## Instruction

Line 86: "The map's **Not yet specified** section is where that dim view is written down: the suspected question, the area to revisit later."
Line 126: "graduate any fog the answer has made specifiable, clearing each graduated patch from **Not yet specified** so it lives only as its new ticket"

## Problem

The "Not yet specified" section has no format specification. Without one, the model can write:
- Long paragraphs describing fuzzy areas
- Inconsistent formatting across sessions
- Entries that mix multiple unresolved questions into one blob
- Vague language that makes it hard to know when something is resolved enough to graduate

## Plan

Add a format instruction:

```markdown
## Not yet specified

- <One unresolved area or suspected question per bullet, phrased as plainly as possible>
- <Another unresolved area — loose is fine, but keep it one question per line>
```

And note: each bullet should be a single question or area that the model can later check against resolutions (can this be ticketed now?). If an answer makes a bullet specifiable, graduate the *entire bullet* — don't partially clear it.
