# 01 — "Decisions so far" context pointer is hard to read

## File

`skills/engineering/wayfinder/SKILL.md` — line 44

## Instruction

```markdown
- [<closed ticket title>](link) — <one-line gist of the answer>
```

## Problem

The template tells the model to write a "one-line gist" but gives no guidance on *how* to write it. The model can produce:
- Long, multi-clause sentences
- Jargon-heavy technical descriptions
- Sentences that bury the verdict in background context
- Entries that span far more than one visual line in rendered markdown

This makes the "Decisions so far" section hard to scan, especially as it grows with many resolved tickets.

## Plan

Add an instruction beneath the template specifying that the gist must:
- Use plain, simple English
- Start with the verdict/decision itself (front-load the answer)
- Stay under ~15 words
- Avoid jargon, acronyms, or implementation details

Example of a readable entry:
```
- [Cache provider decision](https://...) — Use Redis; Redis Cluster handles the multi-region requirement without client-side sharding.
```

Example to avoid:
```
- [Cache provider decision](https://...) — After evaluating Redis, Memcached, and Hazelcast against our multi-region requirement, considering operational overhead and cost, the decision was made to proceed with Redis Cluster due to its native sharding support and team familiarity.
```
