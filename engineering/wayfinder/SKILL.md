---
name: wayfinder
description: Plan a huge chunk of work, more than one agent session can hold, as a shared map of decision tickets on your issue tracker, and resolve them one at a time until the way to the destination is clear.
disable-model-invocation: true
---

A loose idea has arrived, too big for one agent session, and wrapped in fog, the way from here to the **destination** isn't visible yet. Wayfinding is about finding that way, not charging at the destination. This skill charts the way as a **shared map** on the repo's issue tracker. Then it works its tickets one at a time until the route is clear. Most tickets are **decision tickets**, questions whose resolution is a decision, not slices of a build to execute. One type, Task, is enabling work a decision waits on.

The destination varies per effort, and naming it is the first act of charting, it shapes every ticket. It might be a spec to hand off and iterate on, or a decision to lock before planning starts. The map is domain-agnostic: engineering work, course content, whatever fits the shape.

## Plan, don't do

Wayfinder is **planning** by default. Most tickets resolve a decision. Task completes enabling work a decision waits on. The map is done when no open tickets remain and **Not yet specified** holds no bullets: nothing left to decide before someone goes and does the thing. The pull to just do the work is usually the signal you've reached the edge of the map. Then it's time to hand off. Produce decisions, not deliverables.

## Refer by name

Every map and ticket is an issue, so it has a **name**: its title. In everything the human reads (narration, the map's Decisions-so-far), refer to it by that name, never by a bare id, number, or slug. A wall of `#42, #43, #44` is illegible; names read at a glance. The id and URL don't vanish, a name wraps its link, but they ride *inside* the name, never stand in for it.

## The Map

The map is a single issue on this repo's issue tracker, labelled `wayfinder:map`, the canonical artifact. Its tickets are child issues of the map.

The map is an **index**, not a store. Each ticket records its decision once in its `## Answer` comment. The map holds only a gist plus a link.

**Where the map, its child tickets, blocking, and frontier queries physically live is tracker-specific.** The **frontier** is the open, unblocked, unclaimed child tickets, the edge of the known. The issue tracker should have been provided to you, if not, run `/setup-matt-pocock-skills`. Read `docs/agents/issue-tracker.md` for the tracker in use. Consult its "Wayfinding operations" section for how _this_ repo expresses them. If the tracker doc has no "Wayfinding operations" section, work from its prose and mirror the local-markdown conventions it describes. If no tracker has been provided, default to the local-markdown tracker.

### The map body

The whole map at low resolution, loaded once per session. Open tickets are **not** listed. They are open child issues, found by query.

```markdown
## Destination

<what reaching the end of this map looks like. The spec, decision, or change this effort is finding its way to. One or two lines; every session orients to it before choosing a ticket.>

## Notes

<domain; skills every session should consult; standing preferences for this effort>

## Decisions so far

<!-- the index, one entry per closed in-scope ticket, enough to judge relevance, then fetch the full body of the linked ticket for the detail it holds. Front-load the verdict, use plain English, no jargon, under ~50 words. Remove this comment before saving. -->

- [<closed ticket title>](link): <verdict-first gist, under ~50 words>

## Not yet specified

<!-- see "Fog of war": in-scope fog you can't ticket yet. It graduates, turns an area of fog into one or more concrete tickets, as the frontier advances. Remove this comment before saving. -->

- <One unresolved area or suspected question per bullet, plain English>
- <Another area, one question per line>

## Out of scope

<!-- see "Out of scope": work ruled beyond the destination; closed, never graduates. Remove this comment before saving. -->

- [<closed ticket title>](link): <why this sits beyond the destination>
- Ruled out: <why, in plain English>
```

### Tickets

Each ticket is a **child issue** of the map; the tracker's native identifier is its identity. Its body is the question, sized to one 100K token agent session:

```markdown
## Question

<Phrase clearly in plain English - What is the one thing this ticket will decide or investigate? Why is this needed and/or what is the impact of this decision or investigation?>

<!-- Use question words ("Should", "Which", "Does", "How should", etc.) that frame a decision. Avoid descriptions masquerading as questions. Remove this comment before saving. -->
```

Each ticket carries a `wayfinder:<type>` label, one of `research`, `prototype`, `grilling`, `task` (see [Ticket Types](#ticket-types)).

A session **claims** a ticket **first**, before any work, with the tracker's claim operation, so concurrent sessions skip it. An open, unclaimed ticket is takeable. If you claim a ticket and cannot resolve it in this session, say so plainly. Release the claim with the tracker's reverse operation, so the frontier can pick it up.

Blocking uses the tracker's **native** dependency relationship because it renders the frontier _visually_ in the tracker's own UI, so the human sees what's takeable without opening the map. Only a tracker that lacks native blocking falls back to a body convention. A ticket is **unblocked** when every ticket blocking it is closed.

The answer isn't part of the body, it's recorded on resolution (see [Work through the map](#work-through-the-map)). Assets created while resolving a ticket are linked from the `## Answer` comment, not pasted in. No branch naming rule: use whatever link the skill returns.

## Ticket Types

Every ticket is either (1) **HITL** (human in the loop), worked *with* a human who speaks for themselves or (2) **AFK**, driven by the agent alone. A HITL ticket only resolves through that live exchange; the agent never stands in for the human's side of it (a grilling agent that answers its own questions has broken this).

- **Research** (AFK): Reading documentation, third-party APIs, or local resources like knowledge bases to surface a fact a decision waits on. Resolved by a subagent that calls the Skill tool with `research`. Use when knowledge outside the current working directory is required.
- **Prototype** (HITL): Raise the fidelity of the discussion by making a cheap, rough, concrete artifact to react to an outline, a rough take, a stub, or UI/logic code by calling the Skill tool with `prototype`. Links the prototype from the Answer. Use when "how should it look" or "how should it behave" is the key question.
- **Grilling** (HITL): Conversation that always calls the Skill tool twice, once with `grilling` and once with `domain-modeling`.
- **Task** (HITL or AFK): Manual work that must happen before a *decision* can be made. For example, signing up for a service so its API can be judged, provisioning access, moving data so its shape can be seen. This is the one type that *does* rather than decides because some other tickets can't decide until this task is done. The agent drives it alone where it can (AFK); otherwise it hands the human a precise checklist (HITL). Resolved when the work is done; the answer records what was done and any resulting facts (credentials location, new URLs, row counts) later tickets depend on.

## Fog of war

The map is _deliberately_ incomplete: don't chart what you can't yet see. Beyond the live tickets lies the **fog of war**, the dim view of decisions and investigations you can tell are coming but can't yet pin down, because they hang on questions still open. Resolving a ticket clears the fog ahead of it, graduating whatever's now specifiable into fresh tickets, one at a time, until no open tickets remain and **Not yet specified** is empty.

The map's **Not yet specified** section is where that dim view is written down: one bullet per unresolved area or suspected question, in the format shown in the map template. It's the undiscovered frontier _toward_ the destination, everything here is in scope, just not sharp enough to ticket. When a resolution makes a bullet specifiable, graduate the entire bullet, don't partially clear it.

**Fog or ticket?** The test is whether you can state the question precisely now, _not_ whether you can answer it now.

- **Ticket when** the question is already sharp, even if it's blocked and you can't act on it yet.
- **Not yet specified when** you can't yet phrase it that sharply. Don't pre-slice the fog into ticket-sized pieces: it's coarser than a ticket, and one patch may graduate into several tickets, or none, once the frontier reaches it.

**Not yet specified** excludes what's already decided (Decisions so far), what's already a live ticket, and what's out of scope (the next section).

## Out of scope

Fog only ever gathers _toward_ the destination. The destination fixes the scope, so work beyond it is **out of scope**, it isn't fog, and it doesn't belong in **Not yet specified**. It gets its own **Out of scope** section on the map: work you've consciously ruled out of _this_ effort. Scope, not sharpness, lands it here.

Out-of-scope work never graduates. The frontier stops at the destination, so it returns only if the destination is redrawn, and then as a fresh effort, not a resumption.

Ruling something out of scope is a scoping act, not a step on the route. When a ticket that already exists turns out to sit past the destination, mis-scoped in while charting, or exposed by a resolution, **close it** (a closed ticket is unambiguously off the frontier) and leave one line in the **Out of scope** section (in the format shown in the map template): `- [<closed ticket title>](link): <why this sits beyond the destination>`. If you consciously rule something out of scope without a ticket, use: `- Ruled out: <why>`. These entries stay out of **Decisions so far**, which records only the route actually walked. The reason lives in the linked ticket's `## Answer`. When a closed ticket leaves other tickets unblocked, do not rule on them yourself: tell the user each affected ticket's question, what blocked it, and why the blocker was ruled out, and let them decide.

## Invocation

Work resolves at most one non-research ticket per session, or up to three research tickets in parallel.

Common rules apply to both modes below. A session claims a ticket first, before any work, with the tracker's claim operation. Treat a claimed ticket as untakeable. If you need a claimed ticket, ask the user before taking it. The `## Answer` format is:

```markdown
## Answer

<1–2 sentence verdict in plain English: what was decided or found, and why in brief.>
```

The Decisions-so-far append is the one shared write. Fetch the map fresh before appending; if another session appended since your last fetch, re-read and append after it. The user may run unblocked tickets in parallel, so expect other sessions to edit the tracker concurrently.

### Chart the map

User invokes with a loose idea.

1. **Name the destination.** Call the Skill tool twice, once with `grilling` and once with `domain-modeling`, to pin down what this map is finding its way to; the spec, decision, or change. The destination fixes the scope, so it's settled first.
2. **Map the frontier.** Grill again,  this time, fan out across the whole space rather than deep on any one thread, surfacing the open decisions and the first steps takeable now. **If this surfaces no fog**, the way to the destination is already clear, the whole journey small enough for one session, you don't need a map. Stop and ask the user how they'd like to proceed.
3. **Create the map** (label `wayfinder:map`): Destination and Notes filled in, Decisions-so-far empty, the fog sketched into **Not yet specified**.
4. **Create the tickets you can specify now** as child issues of the map. Then wire blocking edges in a **second pass** (issues need ids before they can reference each other). Wiring sorts them into the frontier and the blocked; everything you can't yet specify stays in the fog, the **Not yet specified** section.
5. Stop - charting builds the map and tickets; it resolves nothing. Tell the user how many research tickets await on the frontier.

### Work through the map

User invokes with a map (URL or number). Tickets are **optional**, without them, you pick, not the user.

1. Load the **map**, the low-res view, not every ticket body.
2. Choose tickets. If the user named tickets, use them: one ticket of any type, or up to three research tickets. Otherwise take the first frontier ticket the tracker's frontier query returns. If it is research, you may take up to two more unblocked research tickets to fill a batch of three. **Claim each** with the tracker operation before any work.
3. Resolve by type, after you **fetch full body of each ticket**. If needed, fetch the full body of any related or closed tickets as well.
   - Research (AFK) never resolves by your own hand. Run one subagent per ticket that calls the Skill tool with `research` on the ticket's question. Name the save path from the tracker's Wayfinding operations; if it names none, use the research default. The subagent runs the research itself and returns its Verdict-File-Confidence-Gaps block.
   - Other types: invoke the skills the `## Notes` block names, if any. If none of those skills fit the question either, ask the user how to proceed.
4. Record each resolution. Post the `## Answer` comment with verdict plus asset link (file link for research, prototype link for prototype). **Close** the ticket. Append a context pointer to the map's Decisions-so-far, with confidence for research. Keep it minimal. The detail lives in linked artifacts (research file, prototype link, ticket comments). The comment itself is a signpost, not a store. Carry gaps into fog and follow-up tickets.
5. Add newly-surfaced tickets (create-then-wire). Graduate any fog the answer has made specifiable. Clear each graduated patch from **Not yet specified** so it lives only as its new ticket. If this ticket or another sits beyond the destination, rule it out of scope. Do not resolve it on the route. If the decision invalidates other parts of the map, close those tickets with the reason in their `## Answer`; never delete them.
6. Close the map when done. If no open tickets remain and **Not yet specified** is empty, the way is clear: close the map and tell the user. If no open tickets remain but fog bullets remain, the map is stuck, not done: leave the map open and report what still blocks graduation.
