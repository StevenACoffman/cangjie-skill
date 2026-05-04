# Phase 2 — RIA++ construct skill

## Target

Each methodological unit passed in Phase 1.5 is constructed into a SKILL.md file that conforms to the Claude Code skill specification.

Use the template: `templates/SKILL.md.template`

## RIA++ Six-segment

### R — Reading (Original Text)

- Direct quotes ≤ 150 words
- Source must be indicated (chapter/page number/paragraph marker)
- If the original book is in English, quote the original English text plus your own Chinese translation. **Do not use existing translations** (to avoid translator copyright issues and potential distortion in the translation).

### I — Interpretation (Self-narration)

- Rewrite the core framework of the methodology in your own words.
- Lines 5–15
- Check: After reading this section, can someone who hasn't read the original book understand what this methodology is about? If not, rewrite it.
- Prohibited: Copying sentences verbatim / Piling up rhetoric

### A1 — Past Application (Case Study from the Book)

- Specific cases in the book where the author personally used this methodology.
- At least 1 item, ≤ 3 items
- Each point should specify: What problem was encountered → How was this methodology used → What conclusions were reached → What were the actual results?

The purpose of this section is to provide the agent with specific analogies when the skill is invoked.

### A2 — Future Trigger ★ (Most Crucial)

This determines whether the skill will actually be used.

It must be made clear:
1. **In what situations would users encounter this type of problem?** (Scenario description, 3–5 items)
2. **What are the linguistic cues in these situations?** (What kind of words would the user say?)
3. **Which adjacent skills are different from it?** (To avoid competing with other skills for access)

The output of A2 is directly written to the `description` field of the skill frontmatter—Claude uses this to decide whether to activate the skill.

**Good A2 example** (from the "reverse thinking" skill):
This approach is not suitable for purely information-retrieving questions when users are struggling with a decision, listing positive reasons but unable to make sense of them, or asking "How can X be done to succeed?"

**Bad A2 Example**:
> When users need to think. ← Too broad a definition may lead to accidental activation.

### E — Execution (Executable Steps)

- Transform the methodology into a 1-2-3 step process
- Each step has a **measurable completion standard**.
- If there is a stopping point (if X is true after step 2, then jump to step 5), write it explicitly.

The purpose of E is to give the agent a clear execution path when calling this skill, rather than allowing it to "play freely".

### B — Boundary

- When should you **not** use this skill (anti-scenario)?
- Failure patterns warned about by the author in the book
- Author blind spots from stage 0, the critical stage
- Other methodologies that are adjacent but easily confused with it

The purpose of skill B is to **prevent misuse**. Without skill B, it might be used when it shouldn't, which would actually do more harm than good.

## Frontmatter Design

```yaml
---
name: <skill-slug> # kebab-case, unique
Description: | A condensed version of A2, ≤300 words
  <When to use + When not to use + Key trigger>
Source book: *Poor Charlie's Almanack* by Charlie Munger
source_chapter: Lecture 3
tags: [decision, mental-model, cognitive-bias]
related_skills: [] # Stage 3 Fill
---
```

## Common Failure Modes

1. **Write paragraph I as a book excerpt** — If it reads like "The author said X in this chapter," you are copying from the book, not explaining it. Rewrite it.
2. **A2 is too broad** — The "when a decision is needed" trigger will never be invoked precisely. A **recognizable linguistic signal** must be provided.
3. **Section E only contains philosophy and no action** — "Maintaining objectivity" is not a step; "listing three least desirable outcomes" is.
4. **Missing B segment** — Unbounded skills will be overused, leading to user disappointment.
5. **Jumping directly from I to E, skipping A1** — This loses the evidence that the author personally used it, and the skill loses its authority.
