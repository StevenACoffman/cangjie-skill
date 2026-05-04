---
name: book2skill
description: Distill a book into a coherent set of executable skills. Use when the user asks to "Disassemble a book" / "Distill a book" / "Make a book skill" / "turn a book into skills" — i.e. wants a book's frameworks, principles, and methodologies extracted into atomic, reusable Claude skills that an agent can invoke in real-world situations. NOT for simple summarization, book reviews, or role-playing as the author (that is nuwa-skill's job).
---

# book2skill — Distills a book into a set of executable skills, which are then called meta-skills.

## Mission

The methodologies distilled from a book are broken down into a set of **atomic** skills that can be invoked by agents in real-world scenarios, allowing readers to actually use them.

**boundary**:
- ✅ Do: Distillation of methodology/decision framework/checklist/principles/conceptual system
- ❌ Do not include: book excerpts/reading notes/author persona role-playing (for the latter, please use nuwa-skill).

## Core Methodology: RIA-TV++

A pipeline with four phases, parallel extraction, triple validation, and Darwin compatibility testing. See `methodology/00-overview.md` for details.

```
Phase 0: Understanding the entire Adler book → BOOK_OVERVIEW.md
Phase 1: Parallel extraction of 5 agents → Candidate methodology unit pool
Phase 1.5: Triple Validation Screening → Units that Pass
Phase 2: RIA++ constructs skills → SKILL.md for each skill
Phase 3: Zettelkasten Link → INDEX.md
Phase 4: Stress Testing (Darwin Compatible) → test-prompts.json + Rework and Elimination
```

## When to invoke this skill

Users said something similar:
- "Help me disassemble 'Poor Charlie's Almanack'"
- "Distilling Mao's Selected Works into a skill"
- "distill this book into skills: <path>"
- "I want to turn the methodology in this book into a usable skill."

## Input Requirements

**Confirmation must be obtained from the user before proceeding:**
1. **The text source of the book:** PDF / EPUB / TXT file path, or accessible plain text. **Do not** dissect the book "from memory" without the text—it's better to stop and ask the user.
2. **Title + Author + Publication Year**: Used for catalog naming and auditing.
3. **Is this the first time piloting?**: If this is the first time the user is using book2skill, it is recommended to split the verification process into one book first and then proceed with the batch.

## Output Structure

```
books/<book-slug>/
├── BOOK_OVERVIEW.md # Phase 0 Outputs: Theme/Skeleton/Terminology/Criticism
├── INDEX.md # Stage 3 Output: Skill Overview + Reference Image
├── candidates/ # Phase 1 Output: Original candidate pool (for auditing)
├── rejected/ # Phase 1.5 Unit rejected + Reason (for auditing purposes)
├── <skill-slug-1>/
│   ├── SKILL.md
│ └── test-prompts.json # darwin-skill compatible format
├── <skill-slug-2>/
│   └── ...
```

## Execution Flow (Strictly in Sequence)

### Stage 0 — Understanding the Whole Book

1. Read user-provided book text. Large files are read in chunks.
2. Perform the four Adler steps (structure/interpretation/critique/application) in `methodology/01-stage0-adler.md`.
3. Fill in `templates/BOOK_OVERVIEW.md.template` and write `books/<slug>/BOOK_OVERVIEW.md`.
4. Show the output to the user for confirmation: "Did I understand the framework correctly? Are there any key areas you want to emphasize?" Only proceed to stage 1 after receiving confirmation.

### Phase 1 — Parallel extraction of 5 sub-agents

**Parallelism** spawns 5 Task sub-agents (using the Agent tool, 5 are launched in a single call):

| sub-agent | prompt read | output |
|---|---|---|
| Framework Extractor | `extractors/framework-extractor.md` | Decision Framework / Mental Model |
| Principle Extractor | `extractors/principle-extractor.md` | Principles/Lists/Rules |
| Case Extractor | `extractors/case-extractor.md` | Examples personally used by the author in the book |
| Counterexample Extractor | `extractors/counter-example-extractor.md` | Failure patterns warned about in the book |
| Terminology Extractor | `extractors/glossary-extractor.md` | Key Concept Dictionary |

Each sub-agent independently reads, extracts, and outputs data to `books/<slug>/candidates/<type>.md`.

### Phase 1.5 — Triple Validation Screening

Read `methodology/03-stage1.5-triple-verify.md` and execute the following for each candidate unit:

- **V1 Cross-Domain:** Does the book contain at least two independent paragraphs providing supporting evidence?
- **V2 Predictive Power**: Can it be used to answer a new question that isn't explicitly addressed in the book?
- **V3 Uniqueness**: Isn't this common sense that any intelligent person would say?

Successful entries proceed to Phase 2. Unsuccessful entries are written to `books/<slug>/rejected/` with the reason—this preserves the audit trail and allows users to retrieve the results later.

### Phase 2 — RIA++ construct skill

For each passed cell, populate `templates/SKILL.md.template`:

- **R (Reading)**: Original text citation ≤ 150 words/paragraph
- **I (Interpretation)**: Rewrite the methodology framework in your own words (avoiding simply copying the translation).
- **A1 (Past Application)**: Case studies used by the author in the book
- **A2 (Future Trigger)** ★: In what situations would a user need this → the `description` field of a skill.
- **E (Execution)**: 1-2-3 Executable Steps
- **B (Boundary)**: When is it inapplicable/Blind spots of the author from Stage 0, the critical stage.

See `methodology/04-stage2-ria-plus.md` for details.

### Phase 3 — Zettelkasten Link

按 `methodology/05-stage3-zettelkasten.md`:
1. Identify the reference relationships between skills (A depends on B / A compares to B / A combines with B).
2. Add a "Related skills" section to the end of each SKILL.md file.
3. Generate `INDEX.md` (including the referenced image mermaid) by clicking `templates/INDEX.md.template`.

### Phase 4 — Stress Testing (Darwin Compatible)

For each skill, press `methodology/06-stage4-pressure-test.md`:
1. Design 5–10 test prompts and write them into `test-prompts.json` using `templates/test-prompts.json.template`.
2. Includes at least 3 categories: **Should be invoked** / **Should not be invoked (decoy)** / **Blurred boundaries**
3. Run it locally once; **stage 2: Rework if it fails** — No "surface repairs" are allowed.
4. After all steps are completed, notify the user: "Completed. You can feed it to darwin-skill for automatic evolution with one click."

## Quality Red Line (Output will be stopped if violated)

1. Each skill must pass **all** triple verifications.
2. Each skill must have a complete set of six segments: R, I, A1, A2, E, and B.
3. Original text quotations ≤ 150 words/paragraph
4. Each skill must have a `test-prompts.json` file, which includes decoy tests (scenarios that should not be invoked).
5. The `description` field must explicitly specify the trigger condition; it cannot simply be "a skill about X".

## Ecosystem positioning compared to nuwa-skill / darwin-skill

- **nuwa-skill**: Distilled person (mindset/expressive DNA)
- **book2skill** (This skill): Distilling books (methodology/framework/principles)
- **darwin-skill**: Evolve any skill

The three elements work together: The `test-prompts.json` output by this skill strictly follows the darwin-skill format so that the generated skill can be directly connected to darwin for automatic evolution.

## Calling Conventions

- **Always test with 1 unit first** — unless the user explicitly says "bulk".
- **Proactively report progress between stages** — Don't silently run the program and then dump the results.
- **Unpacking books without relying on memory** — Stop and ask questions if there's no text.
- **Preserve audit trails** — Both candidates and rejected candidates should be recorded.
