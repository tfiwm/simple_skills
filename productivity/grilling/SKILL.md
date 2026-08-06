---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled — the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Ask each round with the **question tool** when the harness provides one; ask as numbered text only when it does not. Match a round to one question-tool call when it is small. When the frontier holds more than about four decisions, split it into several calls — three or four questions each. Ask one call at a time; wait for its answers before the next call. Keep the numbers continuous across the calls and group related decisions together. The round ends only when every call is answered.

In the tool, give each question options for the likely answers. Make your recommended answer the first option, labeled `Recommended: <short answer>` — one to five words — with the one-line rationale in its `description`. The tool adds a free-text option, so the user picks your recommendation or writes another answer. When a recommendation resists a short label, list that question as numbered text alongside the tool call instead of forcing it into a label.

End every round with one extra, open question — the user's feedback on the round. Keep it separate from the numbered frontier: feedback can touch several decisions at once, steer a direction the round never asked about, or revise an answer already given this round. Make it the last question in the round's final call — `No additional feedback` as the recommended option, freely-typed feedback for anything else — or the last text question when you are not using the tool. Count the feedback as settled answers; feed it into the recomputed frontier before the next round.

Each round the user answers reshapes the tree — settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it — don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report — ask the rest of the frontier now. The _decisions_ are the user's — put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
