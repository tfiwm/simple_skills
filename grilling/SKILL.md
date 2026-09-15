---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user says 'grill me', wants to stress-test their thinking, or uses any other 'grill' trigger phrase.
---

Interview the user until every decision behind their plan is settled. Treat the
plan as a tree: each decision has sub-decisions hanging off it.

## The loop

1. List the decisions the plan depends on. A question is ready when everything
   it depends on is settled.
2. Compose a round of up to 4 ready questions. Keep two questions in the same
   round only when neither answer could change the other's meaning or its
   options. Prefer questions from one branch of the tree; mix branches only
   when that branch cannot fill the round.
3. Ask the round in one turn. Give each question the context the user needs
   to answer it well (see "Give the context needed to answer"). Wait for all
   answers.
4. Apply the answers. A settled decision unblocks its children. An answer that
   contradicts a settled decision reopens that decision and everything under
   it.
5. Repeat until no ready questions remain.

## Write clear questions

- One decision per question. If answering needs two decisions, split it into
  two questions.
- Each question stands alone. A reader who sees only the question knows what
  is being asked and about what.
- Apply the two-reader test. If two reasonable readers could read the wording
  differently, rewrite it. Prefer the plainest word: "pin the version", not
  "fix the dependency"; "delete the file", not "remove it".
- Expand noun stacks into full phrases. Write "the mix of calls the benchmark
  should make", not "the representative benchmark call mix".
- Name each thing in the question, or define it there. Say "the mix" only
  after stating what the mix contains.

## Give the context needed to answer

The user answers well only what they understand. Before a question, explain
what they need: define terms, state what hangs on the decision, and state the
trade-off the options weigh. Background shared by several questions in the
round goes once in a short context block, not repeated per question. Scope
context to the question, but never cut it so short that the user must guess
why you are asking.

## Write options that carry the detail

- Give each question 2 to 4 options.
- The label restates the answer. A label that makes sense only with its
  description is too vague; rewrite it.
- Use the option description for the detail: what the answer implies, its
  trade-offs, and, for the recommended option, why it is recommended.
- If asking as plain text, put the detail on the same line after the label.

## Handle answers

- If an answer is vague, restate it as a concrete decision and ask to confirm.
- If the user cannot answer, park it, skip its sub-decisions, move on.
- Look up facts yourself (files, tools, sub-agents). Never ask the user for a
  fact you can find.

## How to ask

Use the `question` tool when available: one call per round, recommended option
first, feedback question last with "No additional feedback" as the first
option. Otherwise send one numbered list per round. End every round with one
open question: "Anything to correct or steer?"

## Close-out

Write a summary: each decision and its answer, one line each, plus parked
decisions as unresolved. Ask the user to confirm. Do not act until they confirm.
