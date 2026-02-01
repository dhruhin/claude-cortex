---
name: route-note
description: Route an idea or note to the knowledge base. Use for thoughts, research, and reference material.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
created: 2026-01-24T17:05
updated: 2026-02-01T01:52
---

# Route Note

Routes ideas and notes to the knowledge base organized by Johnny.Decimal.

## Usage

```
/route-note "Microservices work best with clear domain boundaries"
/route-note Inbox/2026-01-24T10:30:00Z.md
```

## Steps

1. **Classify by Johnny.Decimal** (ask if unsure)
2. **Determine type**: fleeting, literature, or permanent
3. **Generate title** from content (sentence case)
4. **Route to**: `3. Resources/Notes/NN-NN Category/Title.md`
5. **Suggest backlinks** from related notes
6. **Clean up**: Delete inbox file if applicable

**Johnny.Decimal categories**: Check vault's `CLAUDE.md` or scan `3. Resources/Notes/` directory structure.

## Templates

### Fleeting Note
```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [fleeting]
type: fleeting
related: []
---

# Title

Content.

---
## To Process
- [ ] Develop further
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

Standalone explanation.

## Related Ideas
- [[Related Note]]
```

## Note Type Classification

**Fleeting**: Quick captures, unprocessed thoughts. "Microservices seem useful" → needs development.

**Literature**: From external sources with attribution. Book notes, article summaries, research papers.

**Permanent**: Fully developed, standalone ideas in your own words. "Microservices work best with clear domain boundaries because..." with explanations.

## Johnny.Decimal Guidance

**Finding Categories**: Scan `3. Resources/Notes/` for existing `NN-NN Category/` folders. Match content to category (e.g., "10-19 Software Engineering", "20-29 Business").

**Creating Categories**: Avoid creating new categories unless existing ones clearly don't fit. Ask user to confirm new category structure.

**No Match**: When genuinely cross-category, choose primary focus or ask user. Don't create "Miscellaneous".
