---
name: internet-research
description: Research a topic on the internet using a subagent with internet mode. Saves results to internet/ directory and returns filename for the coordinator to read.
allowed-tools: Bash, Read
created: 2026-01-25T01:50
updated: 2026-02-01T01:49
---

# Internet Research

Uses a third-party LLM with internet access to research topics. Leverages clipboard workflow for seamless integration.

## Usage

```
/internet-research "what is the current state of AI regulation in the EU"
/internet-research "latest TypeScript 5.x features" --depth 2
/internet-research "comprehensive analysis of Next.js vs Remix" --depth 3
```

## Input

- **Query** (required): Research topic or question
- **Depth** (optional): 1 (quick facts) | 2 (overview, default) | 3 (comprehensive)

## Processing Steps

1. Parse `--depth N` flag (default: 2), extract clean query
2. Copy system prompt + query to clipboard: `echo "[SYSTEM_PROMPT]\n\nQuery: [USER_QUERY]" | pbcopy`
3. Instruct user to paste into 3P LLM (ChatGPT, Perplexity) and return results
4. Save to `internet/YYYY-MM-DD-sanitized-query.md` with frontmatter

System prompts: Level 1 (brief factual answers) | Level 2 (structured overview with 3-5 sources) | Level 3 (comprehensive analysis with 5-10 sources)

## Output Format

Example: `---` frontmatter (created, query, depth, source) `---` `# [Title]` `[Content]`

## Example Workflow

User requests research → Skill copies prompt to clipboard → User pastes to 3P LLM → User pastes results back → Skill saves to `internet/YYYY-MM-DD-query.md`

## Notes

- Uses 3P LLM with internet access (not ccid subagent)
- Results persist in `internet/` for future reference
