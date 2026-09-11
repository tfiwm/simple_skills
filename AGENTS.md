# Writing Skills

This repo is a flat collection of [Agent Skills](https://agentskills.io/). Each skill is one folder with a `SKILL.md`. This file tells you how to add or edit one.

Read this file before writing a skill, then run the checklist at the end.

Authoritative sources, when this file is not enough:

- [Agent Skills specification](https://agentskills.io/specification)
- [Best practices for skill creators](https://agentskills.io/skill-creation/best-practices)
- [Anthropic skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)

## How a skill loads

An agent sees only the `name` and `description` of every installed skill. It loads `SKILL.md` when the skill matches the task, then reads or runs bundled files as needed. Everything else on disk costs nothing until it is read.

Each level has one job:

- **Metadata** wins or loses the trigger.
- **`SKILL.md`** gives the agent what it needs on every run.
- **Reference files and scripts** carry detail that only some runs need.

## Context is a public good

Every token in `SKILL.md` competes with the conversation, the system prompt, and other skills. Add only what the agent would not know without you: project conventions, non-obvious edge cases, the exact tools to use. Omit what a capable model already knows.

For each paragraph ask: **would the agent get this wrong without it?** If no, cut it. If the agent already completes the task well with no skill, do not write the skill.

Keep `SKILL.md` under 500 lines and 5,000 tokens. Move overflow into reference files.

## Start from real work

A skill written from general knowledge comes out vague, for example "handle errors appropriately". A skill written from a real task carries the exact API patterns, edge cases, and corrections that make it useful.

1. **Do the task once without a skill.** Watch where the agent fails, stalls, or asks for context.
2. **Write down what you supplied:** corrections, input and output formats, and facts the agent did not have.
3. **Build evaluations first.** A few realistic requests with expected behavior, plus the baseline without the skill.
4. **Write the minimum instructions that pass the evaluations.**
5. **Refine from execution.** Run the skill on real requests, read the agent's trace, then cut what it ignored and add a gotcha for every mistake you had to correct.

## Scope

Cover one coherent unit of work, like a function. Too narrow, and one task loads several skills that may conflict. Too broad, and no trigger matches it cleanly.

Prefer extending an existing skill over adding a sibling. If the new material is reference detail for an existing skill, add a file beside its `SKILL.md` instead of a new skill.

## Layout

```
<skill-name>/
  SKILL.md            # required, exact name and case
  agents/
    openai.yaml       # Codex interface metadata
  <REFERENCE>.md      # optional, loaded on demand
  scripts/            # optional, executed or read
```

The collection is flat. Keep one skill per top-level folder, with no category folders. The folder name matches the frontmatter `name` and uses kebab-case.

## Frontmatter

```yaml
---
name: <skill-name>
description: <what it does, in third person, plus when to use it>
---
```

Rules:

- `name` is 1 to 64 characters of `a-z`, `0-9`, and single hyphens. It matches the folder name and passes `^[a-z0-9]+(-[a-z0-9]+)*$`.
- `description` is 1 to 1024 characters. Write it in third person, and name both the capability and the situations that trigger it.
- Do not use XML tags in either field. Do not use reserved words such as `claude` or `anthropic` in `name`.
- Some runtimes ignore unknown fields, so do not rely on them.

### User-invoked skills

A skill that must only run when the user types it needs three things:

1. `disable-model-invocation: true` in the frontmatter (Claude Code).
2. `policy.allow_implicit_invocation: false` in `agents/openai.yaml` (Codex).
3. A listing under "User-invoked" in [README.md](./README.md).

## Write the description to trigger

The description is the most important sentence in the skill. The agent reads it to choose among many skills.

- State what the skill does and when to use it.
- Include the words a user would actually say, for example "review since X", "red-green-refactor", or "handoff".
- Use third person. "Generates a report", not "I generate" or "you can use this".
- Name concrete artifacts and actions, not a vague area. "Analyze Excel workbooks and build pivot tables" beats "helps with spreadsheets".

Bad: `description: Helps with documents`

Good: `description: Extract text and tables from PDF files, fill forms, and merge documents. Use when the user mentions PDFs, forms, or document extraction.`

## Write the body

### Calibrate control to fragility

Give freedom when many approaches work, and explain why, so the agent can adapt. Be prescriptive when order matters or a step is dangerous, and give the exact command.

- **Open field:** "Review the diff for race conditions in concurrent code paths."
- **Narrow bridge:** "Run exactly `python scripts/migrate.py --verify --backup`. Do not add flags."

### Prefer procedures over answers

Teach the method, not one instance of it. A skill that says "join orders to customers on `customer_id` and filter EMEA" helps only that query. A skill that says "read the schema, join on the `_id` convention, apply user filters, then aggregate" generalizes.

### Offer a default, not a menu

Pick one recommended path and mention an alternative only as an escape hatch. "Use pdfplumber. For scanned files, use pdf2image with pytesseract." Do not list five equal options.

### Work in explicit steps

Break multi-step work into ordered steps. For anything complex or fragile, give a checklist the agent can copy and mark off. Name each step, and put its command or decision inside it.

### Close the loop

Add a validation step for anything fragile: run the validator, fix errors, repeat. For batch or destructive work, use plan, validate, execute. Write the plan to a structured file, check it against a source of truth with a script, then apply it.

### Record gotchas

The highest-value content is often a short list of environment facts that defy reasonable assumptions. Write concrete corrections, not general advice.

```
## Gotchas

- The `users` table uses soft deletes. Every query needs `WHERE deleted_at IS NULL`.
- The same ID is `user_id` in the database, `uid` in auth, and `accountId` in billing.
```

Keep gotchas in `SKILL.md` where the agent meets them before the mistake. When you correct the agent, add the correction here.

### Show, do not only tell

Concrete input and output pairs teach format and tone better than description does. For output format, provide a template and say whether it is strict or a starting point.

### Define the handoff

Say what the agent reports back: the format, the length, and what to ask the human when blocked. A human reads that output, so make it short and specific.

## Write for the human

The agent is the reader, but a human maintains, reviews, and sometimes follows the skill. Serve both readers.

- Write complete, short sentences. Keep one idea per sentence, under 30 words.
- Use active voice and present tense. "Run the validator", not "the validator should be run".
- Use imperative steps, one action each.
- Define a term the first time it appears, then use that same term everywhere.
- Connect cause and result with "because", "so", or "but". Make the logic explicit.
- Prefer literal words. Do not explain a procedure with metaphor.
- Mark code, paths, commands, and field names with backticks.
- Never use em-dashes or en-dashes. Use a comma, parentheses, or two sentences.
- Do not write time-sensitive statements. State the version or date a fact depends on, or point the agent at where to check.
- Never invent facts. When a check fails, say what is unknown and why.
- Mark uncertainty as "Unverified:" plus the reason. That beats a confident guess.

## Reference files

- Link every reference file directly from `SKILL.md`. Do not chain file to file, because partial reads produce partial information.
- Say when to load each file: "Read `references/api-errors.md` only if the API returns a non-200 status."
- Give files descriptive names, and organize them by topic.
- Put a short table of contents at the top of any reference file over 100 lines.
- Use forward slashes in every path, on every platform.

## Scripts

Bundle a script when the agent would otherwise rewrite the same logic on every run, or when the operation must be consistent. A script is more reliable than generated code, and it costs nothing until it runs.

- Say whether to run the script or read it as reference. Execution is the default because it is the most reliable.
- Handle errors inside the script. Do not leave recovery to the agent.
- Justify every constant in a comment. An unexplained value invites the wrong change.
- List required packages and versions, and check they exist in the target environment.
- Test the script before you reference it from `SKILL.md`.

## Composing skills

A skill may load another skill with the Skill tool. Use this for a thin wrapper that chains skills, as `grill-with-docs` does. State the composition in one line, and keep the wrapper free of duplicated instructions.

## Checklist

Before you call the skill done:

- [ ] Name is kebab-case, matches the folder, and passes `^[a-z0-9]+(-[a-z0-9]+)*$`.
- [ ] Description says what the skill does and when to use it, in third person, with trigger words a user would say.
- [ ] `SKILL.md` is under 500 lines, and overflow lives in linked reference files.
- [ ] Every reference file links directly from `SKILL.md`, and each link says when to load it.
- [ ] Steps are ordered, and fragile work has a validation or feedback loop.
- [ ] A gotchas section records every mistake you had to correct.
- [ ] No time-sensitive claim lacks a version or date.
- [ ] Terminology is consistent from frontmatter to the last line.
- [ ] Scripts run, handle their own errors, and state run versus read.
- [ ] User-invoked skills set both invocation opt-outs.
- [ ] `agents/openai.yaml` exists with `display_name` and `short_description`.
- [ ] `README.md` lists the skill in the correct section.
- [ ] You tested the skill on a real request and watched the agent succeed.
- [ ] A fresh reader can follow the skill without asking what a term means.
