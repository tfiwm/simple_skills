# 06 — Ticket body question phrasing has no clarity guidance

## File

`skills/engineering/wayfinder/SKILL.md` — lines 59–63

## Instruction

```markdown
## Question

<the decision or investigation this ticket resolves>
```

## Problem

The template says "the decision or investigation this ticket resolves" — this encourages a descriptive statement rather than a clear question. A model can write:

- Long compound descriptions: "Investigate the feasibility of implementing a webhook-based event notification system for the ordering context's outbound data synchronization requirements"
- Vague statements that don't frame a specific decision: "Look into caching options"
- Jargon-heavy phrasing that's hard to scan

A well-formed question is the most readable form — it tells the reader exactly what will be decided.

## Plan

Replace the instruction with an explicit format requirement:

```markdown
## Question

<Phrase as a single, clear question in plain English. What is the one thing this ticket will decide or find out?>
```

And add guidance: use "Should...", "Which...", "Does...", "How should..." — question words that frame a decision. Avoid descriptions masquerading as questions.

## Status

Applied. Replaced the ticket question placeholder with phrasing guidance and a comment listing example question words.
