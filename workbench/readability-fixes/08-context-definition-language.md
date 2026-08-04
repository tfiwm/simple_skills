# 08 — CONTEXT.md term definitions could use plain English guidance

## File

`skills/engineering/domain-modeling/CONTEXT-FORMAT.md` — lines 27–29

## Instruction

- **Keep definitions tight.** One or two sentences max. Define what it IS, not what it does.
- **Only include terms specific to this project's context.**

## Problem

The existing rules are good, but they lack explicit "use plain English" guidance. The model can still produce:
- Definitions that use jargon to define jargon ("An Order is an aggregate root that coordinates Bounded Context interactions")
- Passive-voice or circuitous phrasing
- Definitions that describe implementation behavior rather than the concept's meaning

"Define what it IS, not what it does" is somewhat abstract — a concrete example would help.

## Plan

Add a rule:

- **Use plain English.** Define the term as you would to a new team member. Avoid jargon, implementation patterns ("aggregate root", "event-sourced"), and multi-clause sentences. Read the definition aloud — if it sounds like a textbook, simplify it.

And add a contrasting example to the `## Rules` section:

Good: "**Order**: A request from a customer to purchase one or more products."
Avoid: "**Order**: An aggregate root within the ordering context that orchestrates the lifecycle of a purchase transaction via domain events."

## Status

Applied. Added a "Use plain English" rule with contrasting examples to `engineering/domain-modeling/CONTEXT-FORMAT.md`.
