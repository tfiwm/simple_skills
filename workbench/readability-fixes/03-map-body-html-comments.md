# 03 — Map body HTML comments may leak or produce verbose filler

## File

`skills/engineering/wayfinder/SKILL.md` — lines 31–53

## Instruction

The map body template uses HTML comments (`<!-- ... -->`) as instructions to the model:

```markdown
## Decisions so far

<!-- the index — one line per closed ticket: enough to judge relevance, then zoom the link for the detail the ticket holds -->

## Not yet specified

<!-- see "Fog of war": in-scope fog you can't ticket yet; graduates as the frontier advances -->
```

## Problem

- Models may accidentally leave `<!-- ... -->` comments in the final artifact, which renders as invisible clutter that feels broken
- The instructions inside comments are phrased as general guidance rather than explicit format constraints, so the model can produce verbose, unstructured prose for each section
- Different sessions may produce wildly different formats for "Not yet specified" and "Notes" content

## Plan

Replace the HTML comments with one of:
- **Option A:** Short inline format descriptions (not HTML comments) that show the expected structure. For example:

  ```markdown
  ## Not yet specified

  <!-- One bullet per unresolved fog area, plain English, one question each. Remove this comment. -->
  ```

- **Option B:** Explicit `> Remove this comment before saving.` appended to each instruction comment, making it clear the comment is meta-instruction.

- **Option C:** Move all structural instructions out of the template and into the surrounding prose, leaving only format markers and placeholder text in the template body.
