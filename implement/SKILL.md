---
name: implement
description: "Implements a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, use /code-review to review the work.

Commit your work to the current branch.

Then mark the ticket done using the close workflow in `docs/agents/issue-tracker.md`: set `Status: resolved` on a local ticket file, or close it on a hosted issue tracker.
