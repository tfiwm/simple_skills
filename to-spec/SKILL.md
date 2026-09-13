---
name: to-spec
description: "Turn the current conversation into a spec and publish it to the project issue tracker: no interview by default; grill only for gaps you cannot safely assume."
disable-model-invocation: true
---

This skill takes the current conversation context and codebase understanding and produces a spec. Do NOT interview the user by default; synthesize what you already know. The one exception is the seam check in Process step 2.

Never invent details. If the conversation leaves a gap, assume only when the conversation or codebase supports the assumption and it is safe to assume. Otherwise, call the Skill tool twice, once with `grilling` and once with `domain-modeling`, to clarify the gap before writing the spec.

The issue tracker and triage label vocabulary should have been provided to you. If not, call the skill tool with name `setup-forge-skills` before continuing.

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary throughout the spec, and respect any ADRs in the area you're touching.

2. Sketch out the seams at which you're going to test the feature. Prefer existing seams to new ones. Use the highest seam possible, the one closest to the caller, so a single seam covers the most behavior. If you need a new seam, propose it as high as you can. The fewer seams across the codebase, the better; the ideal number is one.

Check with the user that these seams match their expectations.

3. Write the spec using the template below.

4. Publish it to the project issue tracker. If a spec for this feature already exists and no tickets have been cut from it, update that spec. Otherwise, open a new one.

5. Apply the triage label that `docs/agents/triage-labels.md` maps to `ready-for-agent`. No additional triage is needed.

```markdown
<!-- Local markdown tracker: keep the heading and the Status line below. Hosted issue tracker: the issue title field and the triage label carry them, so delete this comment, the heading, and the Status line. -->

# <one-line problem statement>

**Status:** <the label `docs/agents/triage-labels.md` maps to `ready-for-agent`>

## Problem Statement

<!-- The problem this change solves, from the perspective of whoever feels it: a user, a developer, an operator, or the maintainer. -->

<problem statement>

## Solution

<!-- The solution to the problem, from the same perspective. For behavior-preserving changes, state the technical result instead. -->

<solution>

## Assumptions

<!-- Only record an assumption when something supports it and assuming is safe. State the supporting evidence with each one. Example: Sessions expire after 24 hours, because the existing token flow already does this and this change does not touch it. -->

- <assumption>: <supporting evidence>

## User Stories

<!-- One story per distinct actor-visible behavior, including error and edge cases. Each story must be independently testable. Actors include end users, developers, operators, and downstream systems. Example: As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending. -->

1. As an <actor>, I want a <feature>, so that <benefit>

<!-- Behavior-preserving changes: replace the User Stories heading and list with the Invariants heading and list below. -->

## Invariants

<!-- Example: Given an empty cart, checkout still rejects the request. -->

1. Given <situation>, <observable behavior> stays the same.

## Implementation Decisions

<!-- The implementation decisions made. They can cover the modules built or modified, their interfaces, technical clarifications, architecture, schema changes, API contracts, and interactions. Do not include file paths or code snippets: they go stale. Exception: a prototype snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape) may be inlined and noted as coming from a prototype. -->

- <decision>

## Testing Decisions

<!-- The testing decisions made: the seams chosen in Process step 2 and the tests at each, what makes a good test (external behavior only), and prior art in the codebase. -->

- <testing decision>

## Out of Scope

<!-- What is out of scope for this spec. -->

- <out of scope item>

## Further Notes

<!-- Any further notes about the feature. -->

<notes>
```

Remove the comments before publishing.
