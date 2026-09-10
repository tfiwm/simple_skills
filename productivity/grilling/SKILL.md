---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

## Work in rounds

Work the tree in **rounds**. A ready question is a decision you can ask _now_. Its prerequisites are already settled. Place questions in one round only when one answer will not change the meaning or options of the others. Wait for answers before you start the next round.

Ask each round as one group. Keep each round to three or four ready questions. Feedback does not count toward that limit. Ask the group and wait for answers. Then recompute. Give each ready question a number. Start at 1 and never reuse a number for a different question. A question that reopens or becomes unblocked keeps its original number. Number only ready questions.

**Recompute** means re-derive the ready questions from the current design tree. Settled decisions are removed from the ready set. Parked decisions block their downstream branches. Reopened decisions go back in as unsettled. In-flight fact searches stay unsettled.

Send the group in one turn. With the `question` tool, make one tool call per round. The tool presents each question with options and a free-text field. With numbered text, send one numbered list per round.

## Round format

Send each round in this shape. Context block first, then the numbered questions, then the feedback line.

```
<context>
<common background that applies to more than one question in this round>

5. <background the user needs for question 5>
8. <background the user needs for question 8>
</context>

5. <question>?
6. <question>?

Feedback: <open feedback question>
```

Omit the `<common background>` part when there is no common context that applies to more than one question in the whole round. Omit a numbered line context when that question needs no context. With the `question` tool, send the context block as prose, then make one tool call. Prefix each ready question's text with its number. The feedback question goes last and gets no prefix.

## Before you ask

Before you ask, explain what the user must know. State key terms in plain words. State why the decision matters. Do this when the question uses jargon or assumes a concept. Do it also when it leans on a past decision the user may not recall. When context applies to the whole round, put it on the `All:` line in the context block. When it belongs to one question, put it under that question's number. Keep each note tight. Cover only what the user needs to answer. Add length only when the question stays unclear without it. Then send the round group.

## Question options

Give each question options for the likely answers. Make your recommended answer the first option. When text does not fit the question or its options, keep a short form with the question. Put the full text in the numbered context for that question number. Send the context block first, then the questions right after in the same turn. Do not split them across rounds.

If asking with the tool, make the recommended option the first in the list. Keep each option label a short summary of the answer. State the recommendation and its reason in that option's `description`, so the label keeps its full space. Rely on the tool free-text option for custom answers.

If asking as numbered text, mark the recommendation in plain words and let the user reply in plain words.

## Round feedback question

End every round with one extra, open question. It asks for feedback on the round. Keep it outside the numbered ready questions. Use it to catch steering, new direction, or a revised past answer. Put it last in the round's question group. Feed the result into the next recompute.

If asking with the tool, make "No additional feedback" the first option. Rely on the tool free-text option for custom feedback. Do not add a second typed option.

If asking as numbered text, ask it last as plain text.

## Respond to answers

Each user answer reshapes the tree. Settling a decision unblocks the questions that depended on it. The unblocked questions join the ready set. Recompute the ready questions and ask the next round. Sort each user message by effect. When it settles no new decision, explain and continue with what is still open. When it settles a new decision that fits past settled decisions, settle it and recompute forward. When it settles a new decision that clashes with a past settled decision, reopen that past decision. Mark all decisions that hung off it as unsettled again. State what you reopened, then recompute the ready questions.

When the user cannot answer a question, do not force it. Park that decision, mark its downstream questions as blocked, and recompute the rest. If a parked decision blocks everything left, ask the user how to proceed. List parked decisions as unsettled in the close-out note.

When the user says to stop before every branch is visited, end the session. Write the close-out note over the decisions settled so far. List what is still open as unsettled. Do not act until they confirm.

## Find facts yourself

Finding _facts_ is your job, never the user's. When a ready question needs a fact from tools or files, dispatch a sub-agent. Tell it the fact to find and where to look. Ask it to return the fact, the source, and trust level. Do not ask the user for what you can look up. Do not block the round on it. Treat a running search as unsettled. Ask other ready questions now. Ask downstream questions only after the fact returns. When the search fails or trust is low, state that and ask how to proceed. Never answer a decision yourself. Put each decision to the user and wait.

## Close-out

The session is done when no ready questions remain and no in-flight fact searches are pending. Every branch is visited. Nothing is left assumed. Then write a close-out note. List each settled decision and its answer in plain words. Ask the user to confirm shared understanding. Do not act until they confirm.

