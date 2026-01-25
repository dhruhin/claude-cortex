---
name: internet-research
description: Research a topic on the internet using a subagent with internet mode. Saves results to internet/ directory and returns filename for the coordinator to read.
allowed-tools: Bash, Read
created: 2026-01-25T01:50
updated: 2026-01-25T01:54
---

# Internet Research

Spawns a subagent with internet access to research a topic and saves the results as a markdown file.

## Usage

```
/internet-research "what is the current state of AI regulation in the EU"
/internet-research "latest TypeScript 5.x features"
```

## Input

- **Query** (required): The research topic or question to investigate

## Processing Steps

1. **Sanitize query for filename**:
   - Convert query to lowercase
   - Replace spaces with hyphens
   - Remove special characters
   - Truncate to ~50 characters for readability
   - Format: `YYYY-MM-DD-sanitized-query.md`

2. **Run internet subagent**:
   ```bash
   cd internet && ccid -p "Research this topic and provide a comprehensive summary: [QUERY]

   Save your findings as a markdown file with:
   - A clear title as H1
   - Key findings as bullet points
   - Sources/references where applicable
   - Date researched

   Be thorough but concise. Focus on actionable information."
   ```

3. **Wait for subagent to complete**:
   - The ccid command runs synchronously
   - Subagent saves results to `internet/YYYY-MM-DD-query.md`

4. **Verify file creation**:
   - Check that the expected file was created
   - If not found, check for similarly named files created today

5. **Return result**:
   - Return the filename so caller can read the research
   - Format: `Research saved to: internet/YYYY-MM-DD-query.md`

## Output

Returns the path to the created research file, allowing the coordinator to:
- Read the file contents
- Reference it in other tasks
- Link it in notes or projects

## Example Workflow

```
User: Research the latest Next.js features
Skill: Running internet research subagent...
Skill: Research saved to: internet/2026-01-25-latest-nextjs-features.md
User: Great, now summarize that for my notes
[Coordinator reads internet/2026-01-25-latest-nextjs-features.md]
```

## Notes

- The `ccid` command runs Claude Code in internet mode with a prompt
- Subagent has full internet access via WebSearch and WebFetch
- Results persist in internet/ for future reference
- Use descriptive queries for better research results
- The internet/ directory should be gitignored if containing ephemeral research
