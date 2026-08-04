# 02 — Resolution comments have no format template

## File

`skills/engineering/wayfinder/SKILL.md` — line 125

## Instruction

"post the answer as a **resolution comment**, **close** the issue"

No template or format is specified for the resolution comment's content.

## Problem

Without a template, the model can dump:
- Full session transcripts or reasoning chains
- Multiple paragraphs of analysis
- Technical implementation details irrelevant to the decision
- Unstructured freeform text

A resolution comment should be a crisp record of the decision — something a reader can grasp in seconds.

## Plan

Add a template for resolution comments:

```markdown
## Answer

<1–2 sentence verdict in plain English: what was decided or found, and why in brief.>
```

Keep it minimal. The detail lives in linked artifacts (research file, prototype branch, ticket comments). The comment itself is a signpost, not a store.
