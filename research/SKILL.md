---
name: research
description: "Researches a question against primary sources when the user wants a topic researched, docs or API facts gathered, or reading legwork done. Trivial lookups get an inline answer; larger questions get a delegated run and a saved findings file."
---

Run this research yourself, or delegate it to a background sub-agent and hand it these instructions as its job. Delegate when the work is large, long-running, should proceed in parallel with other work, or the caller explicitly asks to run this in parallel or as a sub-agent. Otherwise do it inline. Trivial lookups get a short inline answer under the rules below. When unsure, do it inline and offer to escalate. On a delegated run, relay only its Verdict-File-Confidence-Gaps block.

Research the main question lightly first. Map it into sub-questions when the first pass shows more than one part. Then research thoroughly: the single question when it shows one, each sub-question when it shows more. Keep one Findings section; separate unrelated domains with headings. Stop when the main question is answered, or when every open sub-question is blocked. A sub-question is blocked when a source is unreachable, sources genuinely conflict, or it is out of scope. Record each blocked sub-question and its cause in Gaps.

Use primary sources first: official docs, source code, specs, first-party APIs. Pick them to fit the question's domain. Skip secondary sources by default. Use them only to supplement: real-world behavior, limits, or performance the primary sources do not cover. Never treat them as authoritative.

Ground every claim in evidence you fetched or observed. Fetched means you opened the page or file. Observed means you ran a command or read local code; name what you ran or read. Cite a source only after you fetch it. Never cite from memory. Mark what you could not verify next to the claim, for example: "unverified: the changelog would not load".

When sources disagree, find the cause first: different versions, different variants (cloud or self-hosted, free or paid tier), or a real dispute. Report each side with its version or date. Never merge the two into one claim. Prefer the current official source when one side is outdated. A disagreement explained by version or variant is resolved, not a conflict. For example, the docs say a flag exists and a tutorial says it does not. The cause is version skew: the flag shipped in v2.4, the tutorial covers v2.1. Good report: "the flag shipped in v2.4 (official docs); the tutorial targets v2.1, which predates it". Bad report: "the flag exists in some versions".

Check that each source is current. Use the signal its domain offers: changelogs for software, revision status for standards, effective dates for policies. Note the version or date each finding rests on. Prefer live pages over archived copies. When only an archived copy exists, say so next to the claim.

Deliver the answer inline or as a file. Use a short inline answer for trivial lookups. Otherwise write a findings file at `docs/research/<id>_<name>.md`, relative to the working directory, creating the folder if needed. Take `<id>` as one more than the largest numeric prefix among files already there, zero-padded to two digits. Start at `01` when no numbered file exists yet. Keep counting past `99` with three digits (`100`, `101`, and so on). `<name>` is short kebab-case, for example `flag-compat`, giving `07_flag-compat.md`. Honor an explicit save location from the user instead. Use this format:

```markdown
# Research: <the research question>

## Summary

<2-4 sentences a reader can grasp in seconds. Answer outright when the evidence settles it. Otherwise start with "Undetermined:" plus the main blocker.>

## Findings

<One claim per paragraph, in plain English. Cite each claim as `[title](url)` right after it. For observed evidence, name the command or file instead of a link. Group claims under headings. Put basics before what builds on them. Include code only when the question needs it. Keep anything that affects an implementation decision.>

## Gaps

<One bullet per question left open, with the cause: a source was unreachable, sources genuinely conflict, or the sub-question was out of scope. Drop this section when empty.>
```

A delegated run does the research itself; never delegate twice. Upon completion, whatever the outcome, return this block to your caller (the orchestrator). When you ran the research yourself, answer the user directly instead.

- Verdict: <the Summary, verbatim>
- File: <path of the findings file, or "none">
- Confidence: <high, medium, or low>
- Gaps: <"none", or "see Gaps in the findings file">

Grade confidence as high when independent primary sources agree, and medium when one fetched source carries the verdict. Grade it low when the verdict is undetermined, rests on an unverified claim, or sources genuinely conflict.
