---
name: route-note
description: Route an idea or note to the knowledge base. Use for thoughts, research, and reference material.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
created: 2026-01-24T17:05
updated: 2026-01-31T22:40
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

## Johnny.Decimal Categories

```
00-09 Relationships    50-59 Life Logistics
10-19 Career           60-69 Business
20-29 Technology       70-79 Learning
30-39 Money            80-89 History
40-49 Health           90-99 Media
```

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
