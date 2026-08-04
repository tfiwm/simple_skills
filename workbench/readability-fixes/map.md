# Readability Fixes — Map

## What we are trying to do

Make all outputs from the skills/engineering/wayfinder/ and skills/engineering/grill-with-docs/ skills and their dependent skills clear and easy to read. The goal is that anyone — the repo owner, a collaborator, or an LLM in a future session — can glance at any artifact (map, ticket, resolution comment, research file, glossary) and understand it in seconds without re-reading or parsing through jargon. The writing guidelines in `skills/engineering/AGENTS.md` define the shared standard; the items below fix specific template gaps that still let verbose or jargon-heavy output through.

## Why this matters

The wayfinder and grill-with-docs produce planning artifacts that persist on the issue tracker. If those artifacts are verbose, jargon-heavy, or inconsistently formatted, they fail at their job: communicating the state of the effort at a glance. Hard-to-read outputs compound over time as more entries accumulate, making the map harder to navigate with each resolved ticket.

## Items

| # | File | Summary |
|---|------|---------|
| 01 | [Decisions so far context pointer](01-decisions-so-far-context-pointer.md) | The one-line gist template has no plain English or conciseness guidance, producing hard-to-scan entries. |
| 02 | [Resolution comments](02-resolution-comments.md) | No format template exists for resolution comments, allowing verbose or unstructured dumping. |
| 03 | [Map body HTML comments](03-map-body-html-comments.md) | HTML comments used as instructions may leak into output or produce verbose filler instead of structured content. |
| 04 | [Out of scope entries](04-out-of-scope-entries.md) | "Out of scope" entries have no specific format, leading to inconsistent and wordy lines. |
| 05 | [Research findings template](05-research-findings-template.md) | The research subagent has zero output structure guidance, producing long, unstructured findings files. |
| 06 | [Ticket body question phrasing](06-ticket-body-question-phrasing.md) | Ticket questions lack clarity guidance — they become descriptions rather than clear, single questions. |
| 07 | [Not yet specified fog format](07-not-yet-specified-fog-format.md) | Fog entries in "Not yet specified" have no format, producing paragraphs instead of scannable bullets. |
| 08 | [Context definition language](08-context-definition-language.md) | Glossary definitions lack explicit plain English guidance, allowing jargon-on-jargon definitions. |
