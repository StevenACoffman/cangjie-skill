# Phase 3 — Zettelkasten Link + INDEX

## Target

Make the relationships between atomic skills explicit, forming a navigable network instead of a bunch of isolated files.

## Three Types of Relationships

1. **Dependency (depends-on):** The prerequisite for using A is understanding B.
   - Example: "Checklist decisions" rely on "multiple mental models" (because the checklist items come from the models).

2. **Contrasts-with**: A and B are two options; choose one based on the context.
   Example: "Forward reasoning" versus "reverse thinking"

3. **Composers-with:** A and B are often used together.
   Example: Combining "circle of competence assessment" with "margin of safety".

## Execution Steps

1. List all skills produced in Phase 2.
2. Perform pairwise scans to identify whether the above three types of relationships exist.
3. In the frontmatter `related_skills` field of each skill, enter:
   ```yaml
   related_skills:
     - slug: multi-mental-models
       relation: depends-on
     - slug: forward-reasoning
       relation: contrasts-with
   ```
4. Append a "Related Skills" section to the end of the SKILL.md file for each skill, explaining the relationship in natural language.
5. Generate `books/<slug>/INDEX.md` (template `templates/INDEX.md.template`)

## INDEX.md must contain

- Basic information about the book (author/year/one-sentence summary)
- A list of all skills, grouped by topic
- Reference diagram (mermaid flowchart or graph)
- Recommended learning order (derived from dependencies)

## The Principle of Moderation

**Avoid creating artificial relationships.** Don't write `related_skills` if there's no genuine dependency/comparison/composition relationship between two skills. It's better to have sparse links than fake ones.

A rule of thumb: If a book is broken down into 10 skills, a reasonable number of relationships is around 8-15. Fewer than 5 relationships indicates the breakdown is too independent (the unit selection may be incorrect), and more than 25 relationships indicate that the relationships are being artificially created.
