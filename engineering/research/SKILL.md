---
name: research
description: Investigate a question against high-trust primary sources and capture the findings as a Markdown file in the repo. Use when the user wants a topic researched, docs or API facts gathered, or reading legwork delegated to a background agent.
---

Spin up a research agent in the **background** whose job is as follows:

Investigate the question against **primary sources** such as official docs, source code, specs, first-party APIs etc.
Decide primary sources based on the nature and the domain of the question. Do not use a secondary write-up of these 
primary sources unless the question requires input from different perspectives. For example, the official documentation 
is the authoritative source to determine if a software library has a specific capability. While the compatibility or 
performance metrics of this capability can also be determined from the official documentation, it is also worth 
considering reports on the compatibility, limitations, performance metrics etc., from other reliable sources based on 
the real world usage of the capability. Never use these secondary sources as the primary source of truth, always use 
them as supplementary information.

First do a breadth first pass to understand the trunk of the research question and its immediate branches. Then do a 
depth pass to explore trunk, its branches and sub-branches as needed. If question itself or first pass uncovers distinct 
groups or domains that are not connected, treat each of those as different trees.

Prepare research findings in the following markdown format, citing each claim's source. The text within `<>` is 
embedded instructions within the template format. Replace `<>` according to the instruction it represents. If the caller 
gives specific instructions on where to save the research findings, follow that. If not, save the research findings as 
`docs/research/<id>_<meaningful_name>.md`.
`<id>` is a two-digit number, one greater than the largest `<id>` among files in `docs/research/`.
Use `01` when the folder has no numbered files. Zero-pad it: `07`, not `7`.
If the next number is taken, or the largest is `99`, keep counting (three digits allowed).
Upon research completion (regardless of success or failure), return 
(1) the outcome of the research work and (2) where to find research content to your caller. 

```markdown
# Research: <well defined question that needs to be researched>

# Summary

<The Summary is the verdict that helps a reader grasp the answer in seconds. Keep it short and clear, answering the 
question outright, in plain English>

# Findings

<The Findings are the evidence behind the summary.>

<Categorize findings into groups with the help of markdown headings. Use the trees, trunks and branches as scaffolding 
to build the Markdown structure. Organize the content into further markdown heading levels as needed based on the 
complexity. When ordering groups or content within groups, prefer ordering that helps progressive understanding of the 
question. For example, define fundamentals first, followed by concepts that depend on the fundamentals.>

<Use markdown formatting as required to represent content for readability, ease of understanding and efficiency. 
For example, use code fences when including code snippets.>

<One key concept or claim per paragraph, in plain English, with the source citation inline or in parentheses. For 
complex concepts, split the content into different paragraphs. Avoid implementation details unless the question needs 
technical details for a complete and accurate answer. Keep key information that can affect implementation decisions.>
```

