---
name: research
description: Investigate a question against high-trust primary sources and capture the findings as a Markdown file in the repo. Use when the user wants a topic researched, docs or API facts gathered, or reading legwork delegated to a background agent.
---

Spin up a **background agent** to do the research, so you keep working while it reads.

Its job:

1. Investigate the question against **primary sources** — official docs, source code, specs, first-party APIs — not a secondary write-up of them. Follow every claim back to the source that owns it.
2. Write the findings to a single Markdown file, citing each claim's source. Structure the file as follows:

   ```markdown
   # Research: <question>

   ## Summary

   <1–2 sentences answering the question outright, in plain English>

   ## Findings

   - <One finding per bullet. 1–3 sentences in plain English, with the source citation inline or in parentheses.>
   - <Next finding. Avoid implementation details unless the question is about code.>
   ```

   The Summary is the verdict — a reader should grasp the answer in seconds. The Findings are the evidence behind it.
3. Save it where the repo already keeps such notes; match the existing convention, and if there is none, put it somewhere sensible and say where.
