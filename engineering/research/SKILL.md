---
name: research
description: Investigate a question against primary sources and capture the findings as a Markdown file in the repo. Use when the user wants a topic researched, docs or API facts gathered, or reading legwork delegated to a background agent.
---

Spin up a research agent in the **background** whose job is as follows:

Investigate the question against **primary sources** such as official docs, source code, specs, first-party APIs etc.
Decide primary sources based on the nature and the domain of the question. Do not use a secondary write-up of these 
primary sources by default. Use secondary reports when primary sources alone give an incomplete answer: the question 
asks how the capability behaves in real-world use, the primary sources are silent or thin on the point, or the 
question itself requires different perspectives. For example, the official documentation is the authoritative source to 
determine if a software library has a specific capability. While the compatibility or performance metrics of this 
capability can also be determined from the official documentation, it is also worth considering reports on the 
compatibility, limitations, performance metrics etc., from other reliable sources based on the real world usage of 
the capability. Never use these secondary sources as the primary source of truth, always use them as supplementary 
information.

Ground every claim in fetched or observed evidence. Cite a source only after you fetch and read it. Never cite from
memory. Label any claim you could not verify, right beside the claim, for example: "unverified: the official
changelog could not be fetched".

When two trusted sources disagree, first find the cause: different versions, different variants (cloud or
self-hosted, free or paid tier), or a genuinely contested fact. Report each side with its date or version, and never
blend the two into one claim. Prefer the current official source when one side is outdated. When the disagreement
survives after ruling out version and variant differences, treat the fact as contested. Present both sides in the
Findings, or record the conflict in Gaps when it blocks the answer. For example, the current docs say a flag exists
and a tutorial says it does not. The cause is version skew: the flag shipped in v2.4, the tutorial covers v2.1. Good
report: "the flag shipped in v2.4 (official docs); the tutorial targets v2.1, which predates it". Bad report: "the
flag exists in some versions".

Check every source for staleness: a source is stale when the thing it describes has moved past it. Confirm currency
with the signal the domain offers: changelogs and release notes for software, revision status for standards, effective
dates for policies and terms. Note which version, revision, or date each finding applies to, so a future reader can
judge it later. Prefer a live page over an archived copy, and say so when only an archived copy exists.

The trunk is the core question the research must answer. A branch is one distinct topic that splits off the
trunk or off another branch. A branch that splits off a branch is a sub-branch. A tree is one independent
structure of trunk and branches.

First do a breadth pass to understand the trunk of the research question and its immediate branches. Then do a 
depth pass to explore the trunk, its branches and sub-branches. The depth pass ends when the question is
answered, or when every branch still open is blocked. Record each blocked branch and its blocker in the Gaps
section of the findings file. If the question itself or the breadth pass uncovers distinct groups or domains that are not 
connected, treat each of those as different trees.

Prepare research findings in the following markdown format. Cite the source of each claim. The text within `<>` is 
embedded instructions within the template format. Replace `<>` according to the instruction it represents.

If the caller gives specific instructions on where to save the research findings, follow that. If not, save the 
research findings as `docs/research/<id>_<meaningful_name>.md`. `<id>` is a two-digit number, one greater than the 
largest `<id>` among files in `docs/research/`. Use `01` when the folder has no numbered files. Zero-pad it: `07`, 
not `7`. If the next number is taken, or the largest is `99`, keep counting (three digits allowed).

Upon research completion (regardless of success or failure), return this block to your caller:

- Verdict: <the Summary sentence, verbatim>
- File: <path of the saved findings file>
- Confidence: <high, medium, or low>
- Gaps: <"none", or "see the Gaps section of the findings file">

Grade confidence as high when fetched primary sources agree, medium when a single fetched source carries the
verdict, low when the verdict is undetermined, an unverified claim carries it, or sources conflict.

```markdown
# Research: <well defined question that needs to be researched>

## Summary

<The Summary is the verdict that helps a reader grasp the answer in seconds. Keep it to 2-4 sentences, in plain
English. Use more than 4 only when the question has more distinct parts; give each part its own sentence. Answer the
question outright when the evidence settles it. When it does not, state "undetermined" plus the
main blocker, for example: "Undetermined: the official docs do not document this flag, and no reliable independent
source tests it.">

## Findings

<The Findings are the evidence behind the summary.>

<Categorize findings into groups with the help of markdown headings. Use the trees, trunks and branches as scaffolding 
to build the Markdown structure. Organize the content into further markdown heading levels as needed based on the 
complexity. When ordering groups or content within groups, prefer ordering that helps progressive understanding of the 
question. For example, define fundamentals first, followed by concepts that depend on the fundamentals.>

<Use markdown formatting as required to represent content for readability, ease of understanding and efficiency. 
For example, use code fences when including code snippets.>

<One key concept or claim per paragraph, in plain English, with the source cited as `[title](url)` right after the claim. For 
complex concepts, split the content into different paragraphs. Avoid implementation details unless the question needs 
technical details for a complete and accurate answer. Keep key information that can affect implementation decisions.>

## Gaps

<One bullet per branch or sub-question that could not be resolved, with the reason: source unreachable, sources
contradict, or out of scope. Remove this section when nothing is blocked.>
```

