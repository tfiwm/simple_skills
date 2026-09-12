---
name: to-tickets
description: Break a plan, spec, or the current conversation into tickets: thin vertical slices that run end to end, each declaring its blocking edges, published to the configured tracker (edges as text in one file per ticket locally, or native blocking links on a real tracker).
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

Publish the approved tickets. **How** depends on the tracker `/setup-forge-skills` configured; the tickets are the same either way, only the shape of the blocking edges changes:

- **Local files** → write one file per ticket under `scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01` in dependency order (blockers first). Each file's "Blocked by" lists only the numbers it depends on, e.g. `01, 03`. A blocker's number is always lower than the ticket's own number, so never reference a title. Use this template, one ticket per file, never a single combined file:

  **Feature slug**: reuse the slug when a source spec or issue already lives under `scratch/<slug>/`. Otherwise derive it from the feature name: lowercase ASCII, words joined with hyphens, filler words dropped, 2-5 words. List `scratch/` before creating a directory. Reuse a directory for the same feature. Ask the user only on a real collision or ambiguity.

  **Commit policy**: leave ticket files untracked. Do not commit them unless the user asks. A fresh clone or separate worktree will not see uncommitted tickets, so the user commits and pushes when that sharing is needed.

  <local-ticket-template>

  # <NN>: <Ticket title>

  **Parent:** <parent issue number and title>

  **Blocked by:** <blocker numbers, e.g. 01, 03, or "None (can start immediately)">

  **Status:** ready-for-agent

  ## What to build

  <end-to-end behaviour this ticket makes work>

  ## Acceptance criteria

  - [ ] <acceptance criterion>
  - [ ] <acceptance criterion>

  ## Not in this ticket

  <work deliberately excluded>

  </local-ticket-template>

- **A real issue tracker (GitHub, GitLab, …)** → publish one issue per ticket in dependency order (blockers first) so each ticket's blocking edges can reference real identifiers. Title each issue with the ticket title only; the tracker assigns its own identifier, so the `<NN>` numbering applies to local files only. Use the platform's native parent link when it has one; otherwise write the parent's ID plus title or short description as text. Use the platform's native blocking / sub-issue relationship where it has one; otherwise list each blocking issue by its stable identifier (`#123` on GitHub and GitLab, `ENG-123` on Linear), never by title. Apply the `ready-for-agent` triage label unless instructed otherwise. Use this template, one issue per ticket:

  <issue-template>

  **Parent:** <parent ID plus title>

  **Blocked by:** <blocking issue identifiers, or "None (can start immediately)">

  ## What to build

  <end-to-end behaviour this ticket makes work>

  ## Acceptance criteria

  - [ ] <acceptance criterion>
  - [ ] <acceptance criterion>

  ## Not in this ticket

  <work deliberately excluded>

  </issue-template>

In either form, describe the end-to-end behaviour from the user's perspective, not as a layer-by-layer implementation list. Include a parent reference only when the work came from an existing issue. Add a "Not in this ticket" note only when the slice could be confused with neighboring work; omit it otherwise. Avoid specific file paths or code snippets; they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it and note briefly that it came from a prototype. Trim to the decision-rich parts, not a working demo, just the important bits.

After publishing, stop and hand the backlog to the user. Do not close or modify the parent issue.
