---
name: route-note
description: Route an idea or note to the knowledge base. Use for thoughts, research, and reference material.
allowed-tools: Read, Write, Edit, Glob, Grep
created: 2026-01-24T17:05
updated: 2026-01-24T17:06
---

# Route Note

Routes ideas and notes to the appropriate location in the knowledge base.

## Usage

```
/route-note "Interesting pattern: microservices work best with clear domain boundaries"
/route-note Inbox/2026-01-24T10:30:00Z.md
```

## Input

- **Text**: Idea, thought, or note content
- **File path**: Path to an inbox file containing note content

## Processing Steps

1. **Classify by Johnny.Decimal category**:
   - Analyze content to determine best category
   - If unsure → Ask user to select category

2. **Determine note type**:
   - **Fleeting**: Quick thought, needs processing later
   - **Literature**: Notes from reading/research with source
   - **Permanent**: Refined, standalone insight

3. **Generate title**:
   - Create concise, descriptive title from content
   - Use sentence case

4. **Determine destination**:
   - `Resources/Notes/NN-NN Category/Title.md`

5. **Format the note**:
   - Add frontmatter with tags and type
   - Structure content appropriately
   - Suggest related notes/backlinks

6. **Route the note**:
   - Create new note file in appropriate category
   - Update frontmatter with `related` field for connections

7. **Clean up**:
   - If input was an inbox file, delete it after routing

## Johnny.Decimal Categories

```
00-09 Relationships
10-19 Career
20-29 Technology
30-39 Money
40-49 Health
50-59 Life Logistics
60-69 Business
70-79 Learning
80-89 History
90-99 Media
```

## Note Templates

### Fleeting Note

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [fleeting]
type: fleeting
source: inbox
related: []
---

# Title

Content of the thought or idea.

---

## To Process

- [ ] Develop this idea further
- [ ] Find related concepts
```

### Literature Note

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [literature]
type: literature
source: "Book/Article/URL"
related: []
---

# Title

## Key Points

## Quotes

## My Thoughts
```

### Permanent Note

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [permanent]
type: permanent
related: []
---

# Title

Clear, standalone explanation of the concept.

## Related Ideas

- [[Related Note 1]]
- [[Related Note 2]]
```

## Suggesting Backlinks

After creating the note, search for:
- Notes with similar tags
- Notes mentioning similar concepts
- Notes in the same category

Suggest adding these to the `related` field.
