# 05 — Research findings have no output template

## File

`skills/engineering/research/SKILL.md` — lines 10–12

## Instruction

1. Investigate the question against **primary sources** — official docs, source code, specs, first-party APIs — not a secondary write-up of them. Follow every claim back to the source that owns it.
2. Write the findings to a single Markdown file, citing each claim's source.
3. Save it where the repo already keeps such notes; match the existing convention, and if there is none, put it somewhere sensible and say where.

## Problem

The research skill is invoked as a subagent by wayfinder for AFK research tickets. It has zero guidance on:
- How to structure the findings document
- How concise each finding should be
- Whether to use plain English or technical language
- What heading hierarchy to use
- How many findings belong in the file

This can produce long, unstructured documents that are hard to scan — especially problematic when wayfinder's resolution step needs to extract a one-line gist from it.

## Plan

Add structure guidance after step 2:

- One `## Findings` section with bullet points, one finding per bullet
- Each finding: 1–3 sentences in plain English, concluding with the source citation in parentheses or as an inline link
- A `## Summary` section at the top (1–2 sentences) answering the original question outright
- Avoid implementation details or code snippets unless the question is about code

Example structure:

```markdown
# Research: Should we use Redis or Memcached for caching?

## Summary

Redis is the better fit: it supports native clustering, TTLs, and data structures beyond key-value. Memcached is simpler but lacks these features.

## Findings

- Redis supports cluster mode natively, which handles multi-region replication without client-side sharding. (https://redis.io/docs/management/scaling/)
- Memcached has no built-in clustering — it relies on consistent-hashing at the client level. (https://memcached.org/documentation)
- Redis supports TTLs per key and multiple data types (strings, hashes, lists). Memcached is key-value only with TTLs. (https://redis.io/docs/data-types/)

## Status

Not yet implemented.
```
