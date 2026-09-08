---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. A ready question is a decision you can ask _now_. Its prerequisites are already settled. Place questions in one round only when one answer will not change the meaning or options of the others. Wait for answers before you start the next round.

Ask each round as one group. With the tool, make one tool call per round. With numbered text, send one numbered list per round. Keep each round to three or four ready questions. Feedback does not count toward that limit. Ask the group and wait for answers. Then recompute. Keep one number run across rounds. Start at 1 and never reuse a number. Number only ready questions.

Before you ask, explain what the user must know. State key terms in plain words. State why the decision matters. Do this when the question uses jargon or assumes a concept. Do it also when it leans on a past decision the user may not recall. When context belongs to one question, put it under that question number. Keep each note tight. Cover only what the user needs to answer. Add length only when the question stays unclear without it. Then send the round group.

Give each question options for the likely answers. Make your recommended answer the first option. With the tool, label it `Recommended: <answer>`. Put the reason in its `description`. Rely on the tool free-text option for custom answers. With numbered text, mark the recommendation in plain words and let the user reply in plain words. When text does not fit the question or its options, keep a short form with the question. Put the full text in the numbered context for that question number. Send the context first, then the questions right after in the same turn. Do not split them across rounds.

End every round with one extra, open question. It asks for feedback on the round. Keep it outside the numbered ready questions. Use it to catch steering, new direction, or a revised past answer. Put it last in the round's question group. With the tool, label the first option `Recommended: No additional feedback`. Rely on the tool free-text option for custom feedback. Do not add a second typed option. With numbered text, ask it last as plain text. Feed the result into the next recompute.

Each round the user answers reshapes the tree, settled decisions push the ready questions outward and unblock questions that depended on them. Recompute the ready questions and ask the next round. Sort each user message by effect. When it settles no new decision, explain and continue with what is still open. When it settles a new decision that fits past settled decisions, settle it and recompute forward. When it settles a new decision that clashes with a past settled decision, reopen that past decision. Mark all decisions that hung off it as unsettled again. State what you reopened, then recompute the ready questions.

Finding _facts_ is your job, never the user's. When a ready question needs a fact from tools or files, dispatch a sub-agent. Tell it the fact to find and where to look. Ask it to return the fact, the source, and trust level. Do not ask the user for what you can look up. Do not block the round on it. Treat a running search as unsettled. Ask other ready questions now. Ask downstream questions only after the fact returns. When the search fails or trust is low, state that and ask how to proceed. Never answer a decision yourself. Put each decision to the user and wait.

The session is done when no ready questions remain. Every branch is visited. Nothing is left assumed. Then write a close-out note. List each settled decision and its answer in plain words. Ask the user to confirm shared understanding. Do not act until they confirm.

