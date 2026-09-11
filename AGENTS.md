This is **forge-skills**, a collection of skills: tools for daily workflow and coding work. The following README.md files give a map of different groups of skills in this repo.
 
- engineering/README.md
- productivity/README.md

Most skills in this directory produces written artifacts (maps, tickets, resolution comments, research files, glossary entries) that must be clear, scannable, and accurate. The rules below apply to all output from any skill in this directory.

## Writing guidelines

### Sentence structure

- One sentence, one idea. Keep sentences under 20 words.
- Active voice. Put the doer before the action.
- Simple tenses: present, past, or future. Avoid "-ing" forms and perfect tenses when a simple tense works.
- For steps, use the imperative mood. One instruction per line.

### Word choice

- One term per concept. Do not switch between synonyms for the same thing.
- Pick the simplest word that stays correct: "start" not "initiate", "use" not "utilize", "end" not "terminate", "show" not "display".
- Remove filler words. Use the minimum set of words that keeps the meaning correct.
- Use American English spelling.

### Domain vocabulary

This skill set uses deliberate terms — "seam", "tracer bullet", "deep module", "fog", "ticket", "map" — because each compresses a known concept into one word. Use them. But:

- **Define every domain term on first use** in the artifact, in plain English, without using other domain terms in the definition. Write it for someone seeing it for the first time.
- **The definition must stand alone.** If a reader reads only that sentence, they must understand the term.
- **If the definition sounds like a textbook, simplify it.**
- **Include a good and a bad example with every rule.** A concrete contrast clarifies the rule faster than more prose does.

  Good: "A **seam** is the public boundary where you observe a module's behavior without reaching inside."
  Avoid: "A **seam** is the interface boundary at which deep modules expose their behavior to tracer-bullet testing." (defines vocabulary with vocabulary)

### Artifact structure

- **Use the specified template.** If a section has a format, fill it. Do not invent a new structure.
- **Every output needs a format.** If no template exists for an artifact, define one. An artifact without a format produces inconsistent, hard-to-scan output.
- **Specify templates as code blocks.** Show the exact format the model should produce. A code-block template constrains output more tightly than prose instructions.
- **Front-load the verdict.** The decision, answer, or finding comes first. Context and rationale follow.
- **Bullets over paragraphs.** One point per bullet. Give enough context to judge relevance; link to the detail.
- **Write for the next reader.** A future session (human or LLM) must understand this artifact without the conversation that produced it.

### Behavior

- If a question is ambiguous, make a reasonable assumption and answer. Ask for clarification only when a correct answer is impossible.
- Reference code as `file_path:line_number`.
- Brevity must not sacrifice accuracy. If you are not sure, say what you do not know.

## Skill design

Rules for writing skill instructions, not for writing skill output.

- **Subagents need their own output templates.** When a parent skill invokes a subagent, specify the format the subagent should produce. Without one, the subagent returns unstructured results the parent cannot efficiently consume.
