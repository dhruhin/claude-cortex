---
name: qmd
description: Semantic search across the vault. Use for finding relevant context, notes, and information.
allowed-tools: Bash, Read
created: 2026-01-28T10:00
updated: 2026-01-28T23:14
---

# QMD - Semantic Vault Search

Search the vault semantically to find relevant context, notes, tasks, and information.

## When to Use

- **Use QMD** when searching for concepts, topics, or related content
- **Use Grep** when searching for exact strings or code patterns
- QMD is ideal for: "what do I know about X", "find context about Y", "related notes to Z"

## Collections

| Collection | Path | Content |
|------------|------|---------|
| projects | 1. Projects/ | Active projects, details, meetings |
| areas | 2. Areas/ | Ongoing life domains |
| journal | 3. Resources/Journal/ | Daily notes |
| notes | 3. Resources/Notes/ | Knowledge base |
| context | 4. Context/ | Personal reference files |
| people | 5. People/ | Person tracking |

## Commands

### Quick Keyword Search
```bash
qmd search "project timeline"
qmd search "API" -c notes    # Search specific collection
```

### Semantic Search (uses embeddings)
```bash
qmd vsearch "how to deploy"
```

### Best Quality (hybrid + reranking)
```bash
qmd query "quarterly planning process"
```

### Get Documents
```bash
qmd get "projects/ProjectName/Details/2026-01-20-W04.md"
qmd get "#abc123"                        # By docid from search results
qmd multi-get "journal/2025-05*.md"      # Glob pattern
```

### Export for Context Loading
```bash
qmd search "API" --all --files --min-score 0.3
```

## Workflow

1. Start with `qmd query "your question"` for best results
2. Review returned docids and scores
3. Use `qmd get "#docid"` or Read tool to load relevant files
4. If searching for exact strings, fall back to Grep

## Tips

- Use `qmd query` for complex questions (combines keyword + semantic + reranking)
- Use `qmd vsearch` for concept-based searches
- Use `qmd search` for fast keyword lookups
- Add `-c collection` to scope searches
- Results show relevance scores - focus on high-scoring matches
