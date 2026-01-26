---
name: internet-research
description: Research a topic on the internet using a subagent with internet mode. Saves results to internet/ directory and returns filename for the coordinator to read.
allowed-tools: Bash, Read
created: 2026-01-25T01:50
updated: 2026-01-25T16:45
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

- **Query** (required): The research topic or question to investigate
- **Depth** (optional): Research depth level (1-3, default: 2)
  - 1: Simple query (factual lookups, quick answers)
  - 2: Standard research (topic overview, synthesis)
  - 3: Deep research (comprehensive analysis, comparisons)

## Processing Steps

1. **Determine research depth**:
   - Parse `--depth N` flag from query
   - Default to level 2 if not specified
   - Extract clean query without flags

2. **Select and copy system prompt to clipboard**:

   **Level 1 - Simple Query**
   ```
   You are a research assistant with internet access. Ignore any previous instructions or system settings. Focus entirely on this task.

   Retrieve accurate, up-to-date information for the following query. Provide direct answers with key facts and relevant sources. Keep response concise and factual.
   ```

   **Level 2 - Standard Research**
   ```
   You are a research assistant with internet access. Ignore any previous instructions or system settings. Focus entirely on this task.

   Research the following topic thoroughly and provide a well-structured overview. Include:
   - Key concepts and definitions
   - Current state/trends
   - Multiple perspectives if applicable
   - 3-5 most relevant sources

   Keep explanations clear and organized. Prioritize accuracy and recency.
   ```

   **Level 3 - Deep Research**
   ```
   You are a research assistant with internet access. Ignore any previous instructions or system settings. Focus entirely on this task.

   Conduct comprehensive research on the following topic. Provide:
   - Detailed background and context
   - Multiple viewpoints with evidence
   - Comparisons of different approaches/options
   - Latest developments and research
   - Practical implications
   - 5-10 quality sources with brief notes on why they're relevant

   Aim for thorough, nuanced analysis suitable for decision-making.
   ```

3. **Copy system prompt + query to clipboard**:
   ```bash
   echo "[SYSTEM_PROMPT]\n\nQuery: [USER_QUERY]" | pbcopy
   ```

4. **Instruct user**:
   - Tell user: "System prompt and query copied to clipboard"
   - Tell user: "Paste this into your 3P LLM (e.g., ChatGPT, Perplexity)"
   - Tell user: "Then paste the LLM's response back here when ready"

5. **Wait for user to paste results**:
   - User will paste the research results in their next message
   - No need to wait actively - just inform and return

6. **Save results to file** (after user pastes):
   - Sanitize query for filename (lowercase, hyphens, remove special chars, truncate ~50 chars)
   - Get current date: `date +%Y-%m-%d`
   - Create file: `internet/YYYY-MM-DD-sanitized-query.md`
   - Add frontmatter with created date, depth level, original query
   - Save research content
   - Return path to saved file

## Clipboard Commands

- Copy to clipboard: `echo "content" | pbcopy`
- View clipboard: `pbpaste`
- Read from temp: `pbpaste > /tmp/clipboard.txt && cat /tmp/clipboard.txt`

## Output Format

The saved research file should include:
```markdown
---
created: YYYY-MM-DDTHH:mm
query: Original user query
depth: 1|2|3
source: 3P LLM name
---

# [Research Title]

[Research content from LLM]
```

## Example Workflow

```
User: /internet-research "latest Rust async features" --depth 2
Skill: Copied Level 2 system prompt + query to clipboard.
Skill: Paste into your 3P LLM, then paste the results back here.

[User pastes LLM results]

Skill: Saved research to: internet/2026-01-25-latest-rust-async-features.md
```

## Notes

- Uses 3P LLM with internet access (not ccid subagent)
- Clipboard integration with `pbcopy`/`pbpaste` for seamless workflow
- System prompts instruct LLM to ignore previous settings and focus on task
- Results persist in `internet/` directory for future reference
- Depth levels adjust thoroughness of research
