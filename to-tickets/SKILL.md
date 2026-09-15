---
name: to-tickets
description: "Breaks a plan, spec, or the current conversation into tickets: thin vertical slices that run end to end, each declaring its blocking edges, published to the configured tracker."
disable-model-invocation: true
---

# To Tickets

Break a plan, spec, or conversation into a set of **tickets**: thin vertical slices that run end to end, each declaring the tickets that **block** it.

The issue tracker and triage label vocabulary should have been provided to you. If not, call the skill tool with name `setup-forge-skills` before continuing.

## Process

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes a reference (a spec path, an issue number or URL) as an argument, fetch it and read its full body and comments.

### 2. Explore the codebase

Explore the codebase enough to understand the current state of the code. Ticket titles and descriptions must use the project's domain glossary vocabulary and respect ADRs in the area you're touching. If you already explored the codebase in this conversation, skip this step.

Look for opportunities to prefactor the code to make the implementation easier. "Make the change easy, then make the easy change."

### 3. Draft vertical slices

Break the work into **tickets**: one vertical slice per ticket.

#### Vertical slice rules

- Each slice cuts a narrow but COMPLETE path through every layer (schema, API, UI, tests): vertical, NOT a horizontal slice of one layer
- A completed slice is demoable or verifiable on its own
- Each slice is sized to fit in a single fresh context window
- Prefactor tickets come first: the prefactoring you found in step 2 lands before any slice that depends on it

#### Wide refactors

**Wide refactors are the exception to vertical slicing.** A **wide refactor** is one mechanical change (rename a column, retype a shared symbol) whose **blast radius** fans across the whole codebase, so a single edit breaks thousands of call sites at once and no vertical slice can land green. Don't force it into a single slice. Sequence it as **expand-contract**:

1. **Expand**: add the new form beside the old so nothing breaks.
2. **Migrate**: move the call sites over in batches sized by blast radius (per package, per directory), one ticket per batch, each blocked by the expand. CI stays green batch to batch because the old form still exists.
3. **Contract**: delete the old form once no caller remains, in a ticket blocked by every migrate batch.

When even the batches can't stay green alone, keep the same sequence but route it through a `refactor/<name>` integration branch:

- Create the branch from main before the batches start.
- Each batch ticket branches from the integration branch and merges back into it.
- A final integrate-and-verify ticket, blocked by every batch, merges the branch into main and runs the full verification suite there. Green is promised only there.

#### Blocking edges

Give each ticket its **blocking edges**: the other tickets that must complete before it can start. A ticket with no blockers can start immediately.

Blocking edges must never form a cycle: no ticket may transitively block itself. Verify this before publishing.

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each ticket, show:

- **Title**: short descriptive name
- **Blocked by**: which other tickets (if any) must complete first
- **What it delivers**: the end-to-end behaviour this ticket makes work
- **Acceptance criteria**: the draft checks that prove the ticket is done

#### Acceptance criteria rules

- Each criterion must be objectively checkable, phrased as an observable result: a command and its output, an API response, or a UI state.
- Ban untestable wording. Good: "`GET /health` returns 200 with `{"status":"ok"}`." Bad: "The health endpoint works."
- The implementing agent verifies every criterion it can check on its own.
- When no automated check exists, phrase the criterion as a manual check and mark it for human verification. The implementing agent asks a human only for those.

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the blocking edges correct? does each ticket only depend on tickets that genuinely gate it?
- Should any tickets be merged or split further?

Iterate until the user approves the breakdown.

### 5. Publish the tickets to the configured tracker

Publish the approved tickets. Read `docs/agents/issue-tracker.md` for where they go and how their metadata is written.

#### Ticket content rules

Describe the end-to-end behaviour from the user's perspective, not as a layer-by-layer implementation list. Include a parent reference only when the work came from an existing spec or issue. Add a "Not in this ticket" note only when the slice could be confused with neighboring work; omit it otherwise. Avoid specific file paths or code snippets; they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it and note briefly that it came from a prototype. Trim to the decision-rich parts, not a working demo, just the important bits.

Every ticket body uses this template:

```markdown
## What to build

<end-to-end behaviour this ticket makes work>

## Acceptance criteria

- [ ] <acceptance criterion>
- [ ] <acceptance criterion>

## Not in this ticket

<work deliberately excluded>
```

The ticket body is the same for both. Follow the branch below that matches the tracker named in `docs/agents/issue-tracker.md`: the local markdown tracker, or a hosted issue tracker such as GitHub or GitLab. Only the shape of the blocking edges and where the metadata is written change.

#### Local markdown tracker

The file layout, numbering, slug, commit policy, and metadata header come from `docs/agents/issue-tracker.md`. Two workflow rules:

- Create the tickets blockers first, so the numbering reads in the order the work lands.
- List each blocker by its number, never by title.

Write `Status` using the label string that `docs/agents/triage-labels.md` maps to `ready-for-agent`. Omit the `Parent` line when the work has no parent.

#### Hosted issue tracker (GitHub, GitLab, …)

Publish one issue per ticket in dependency order (blockers first) so each ticket's blocking edges can reference real identifiers. Title each issue with the ticket title only; the tracker assigns its own identifier, so the `<NN>` numbering applies to the local markdown tracker only. Use the platform's native parent link when it has one; otherwise write the parent's ID plus title or short description as text. Use the platform's native blocking / sub-issue relationship where it has one; otherwise list each blocking issue by its stable identifier (`#123` on GitHub and GitLab, `ENG-123` on Linear), never by title. Apply the triage label that `docs/agents/triage-labels.md` maps to `ready-for-agent`, unless instructed otherwise.

When the platform has no native parent or blocking relationship, add the missing line at the top of the body, before `## What to build`:

```markdown
**Parent:** <parent ID plus title>

**Blocked by:** <blocking issue identifiers, or "None (can start immediately)">
```

After publishing, stop and hand the published tickets to the user. Do not close or modify the parent issue.
