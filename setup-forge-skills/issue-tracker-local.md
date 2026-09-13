# Issue tracker: Local Markdown

Issues and specs for this repo live as markdown files in `scratch/`.

## Conventions

- One feature per directory: `scratch/<feature-slug>/`
- The spec is `scratch/<feature-slug>/spec.md`
- Implementation issues are one file per ticket at `scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01` in creation order, never a single combined tickets file
- A ticket file starts with a `# <NN>: <title>` heading, followed by metadata lines near the top: `Parent:` (only when the work has a parent), `Blocked by:`, and `Status:`
- Feature slug: reuse an existing slug when a spec or issue for the feature already lives under `scratch/<feature-slug>/`. Otherwise derive it from the feature name: lowercase ASCII, words joined with hyphens, filler words dropped, 2-5 words. List `scratch/` before creating a directory and ask the user only on a real collision or ambiguity.
- Commit policy: leave these files untracked; do not commit them unless the user asks. A fresh clone or separate worktree will not see uncommitted issues, so the user commits and pushes when sharing is needed.
- `Status:` records the ticket's state. Active implementation tickets use the triage role strings mapped in `docs/agents/triage-labels.md`, and the implementing agent sets `resolved` once the work lands. Wayfinder tickets use `claimed`/`resolved` (see Wayfinding operations below).
- `Blocked by:` lists the numbers of the tickets that must complete first, for example `Blocked by: 01, 03`. A blocker clears when its file is terminal (`resolved` or `wontfix`).
- `Parent:` names the parent when the work has one. On this tracker the parent is usually the spec, so write its path plus title, for example `scratch/<feature-slug>/spec.md` plus the spec title. Omit the line when there is no parent.
- Comments and conversation history append to the bottom of the file under a `## Comments` heading

## When a skill says "publish to the issue tracker"

Create a new file, creating directories as needed. The path depends on the artifact kind:

- An implementation ticket goes at `scratch/<feature-slug>/issues/<NN>-<slug>.md`.
- A spec goes at `scratch/<feature-slug>/spec.md`.

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.

## When a skill says "close the ticket"

Set the ticket's `Status:` line to `resolved`. Use `wontfix` when the work is rejected rather than done.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket. Child tickets live in a `decisions/` folder, separate from implementation tickets.

- **Map**: `scratch/<effort>/map.md` (the Notes / Decisions-so-far / Fog body).
- **Child ticket**: `scratch/<effort>/decisions/NN-<slug>.md`, numbered from `01` within `decisions/`, with the question in the body. A `Type:` line records the ticket type (`research`/`prototype`/`grilling`/`task`); a `Status:` line records `claimed`/`resolved`.
- **Research findings**: each research ticket's findings live at `scratch/<effort>/research/NN-<slug>.md`, mirroring the `decisions/` numbering; the file links back to its ticket.
- **Blocking**: a `Blocked by: NN, NN` line near the top. A ticket is unblocked when every file it lists is terminal (`resolved` or `wontfix`).
- **Frontier**: scan `scratch/<effort>/decisions/` for files that are open, unblocked, and unclaimed; first by number wins.
- **Claim**: set `Status: claimed` and save before any work.
- **Resolve**: append the answer under an `## Answer` heading, set `Status: resolved`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.md`. The `map.md` append is not atomic under concurrent sessions. Fetch the map fresh right before appending.
