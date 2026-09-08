---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **ready questions** are every decision whose prerequisites are already settled, the questions you can ask _now_ without guessing at answers you haven't heard yet. Place questions in one round only when one answer will not change the meaning or options of the others. Wait for answers before you start the next round.

Ask with the **question tool** when the harness provides one; else use numbered text. Keep each call to three or four questions. Ask one call at a time and wait for its answers. Keep numbers continuous across calls. A round ends when all its calls are answered. Then recompute.

Before you ask, explain what the user must know. State key terms in plain words. State why the choice matters. Do this when the question uses jargon or assumes a concept. Do it also when it leans on a past choice the user may not recall. Keep it short. Then ask.

In the tool, give each question options for the likely answers. Make your recommended answer the first option. Label it `Recommended: <answer>`. Put the reason in its `description`. The tool adds a free-text option. The user picks your recommendation or writes another answer. When a recommendation cannot fit in a short label, ask that question first as numbered text, then make the tool call for the rest.

End every round with one extra, open question - the user's feedback on the round. Keep it separate from the numbered ready questions: feedback can touch several decisions at once, steer a direction the round never asked about, or revise an answer already given this round. Make it the last question in the round's final call. Include  `No additional feedback` as the first, recommended option, followed by second option where the user can freely-type feedback for anything else. Feed it into the recomputed ready questions before the next round.

Each round the user answers reshapes the tree, settled decisions push the ready questions outward and unblock questions that depended on them. Recompute the ready questions and ask the next round. When the user asks to clarify or discuss, explain and keep going. Do not reopen. Reopen only when the user settles a new decision that clashes with a past settled decision. Then reopen that past decision. Mark all decisions that hung off it as unsettled again. State what you reopened, then recompute the ready questions.

Finding _facts_ is your job, never the user's. When a ready question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it, don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report, ask the rest of the ready questions now. The _decisions_ are the user's, put each to them and wait.

The session is done when no ready questions remain: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

