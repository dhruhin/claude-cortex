---
name: execute-task
description: Autonomously execute tasks from the vault. Use when user wants to make progress on tasks, work on todos, or execute pending items.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task, AskUserQuestion, WebFetch, WebSearch
created: 2026-01-25T01:35
updated: 2026-01-25T01:37
---

# Execute Task

Picks tasks from the vault, makes autonomous progress, and stores execution results with status tracking.

## Usage

```
/execute-task                           # Show menu of tasks + pending executions
/execute-task path/to/file.md "substring"  # Execute specific task
```

## Workflow

```
/execute-task
    ↓
Check executions/pending/ for resumable work
    ↓
If pending exists → Show menu: resume pending OR pick new tasks
If no pending → Show checklist of incomplete tasks
    ↓
User selects one or more tasks (multi-select)
    ↓
Spawn parallel subagents for each selected task
    ↓
Each subagent:
    ├── Analyze what's needed
    ├── Execute or identify blockers
    ├── Write execution file with results
    └── Link execution file to original task
    ↓
Collect results and report
```

---

## Step 1: Check for Pending Executions

```bash
ls executions/pending/
```

If pending executions exist, present options:
1. Resume pending execution(s) - list them
2. Pick new tasks to execute
3. Show all (pending + new tasks)

---

## Step 2: Gather Tasks for Selection

Scan for incomplete tasks across the vault:

### Sources (in priority order)
1. `Projects/*/Details/*.md` - Project tasks
2. `People/*/Details/current.md` - Person tasks
3. `Resources/Journal/YYYY/MM/DD.md` - Daily tasks
4. `Areas/*/Details/*.md` - Area tasks

### Task Pattern
```
- [ ] task text (with optional 🔺⏫🔼🔽 priority and 📅 due date)
```

### Priority Sorting
Display tasks sorted by:
1. Priority emoji (🔺 > ⏫ > 🔼 > 🔽 > none)
2. Due date (sooner first)
3. Source file (projects > people > journal > areas)

---

## Step 3: Present Task Menu

Use `AskUserQuestion` with `multiSelect: true`:

```
Which tasks would you like to execute?

□ 🔺 Review PR for auth feature [[Auth_Migration]]
□ ⏫ Write blog post about productivity [[Content]]
□ 🔼 Research vacation destinations
□ Call Sarah about lunch [[Sarah]]
```

Options should include:
- Task text (truncated if long)
- Priority emoji
- Source context (project/person name)

---

## Step 4: Execute Tasks in Parallel

For each selected task, spawn a subagent:

```
Task tool:
  subagent_type: general-purpose
  prompt: |
    Execute this task autonomously:

    Task: [task text]
    Source file: [path]
    Source line: [line number]

    Instructions:
    1. Analyze what's needed to complete this task
    2. If you can complete it fully, do so
    3. If blocked by questions, document them
    4. Write execution file to executions/[status]/
    5. Add execution link to original task

    Execution scope: Full system access (code, terminal, web)
    Question handling: Store in execution file, don't ask interactively
```

### Parallel Execution

Launch all subagents in a **single message with multiple Task tool calls** for parallel execution. Each writes to its own execution file, so no conflicts.

---

## Step 5: Execution File Format

**Location**: `executions/{status}/YYYY-MM-DD-task-slug.md`

**Slug**: First 3-5 words of task, kebab-case (e.g., `review-pr-auth-feature`)

### File Template

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
status: planned | pending | completed
source_file: path/to/original/task/file.md
source_line: 42
task_text: "exact task text from source"
---

## Open Questions

<!-- Keep at top for visibility -->
- [ ] Question 1?
- [ ] Question 2?

## Task

[Original task with surrounding context from source file]

## Execution Log

### YYYY-MM-DD

[What was done, decisions made, files changed, commands run]

## Outcome

<!-- Only populated when status=completed -->
[Summary of what was accomplished]
```

### Status Definitions

| Status | Meaning | Location |
|--------|---------|----------|
| planned | Questions must be answered before work begins | `executions/planned/` |
| pending | Work started but blocked on questions | `executions/pending/` |
| completed | Task fully completed | `executions/completed/` |

---

## Step 6: Link Execution to Original Task

After creating execution file, update the original task line:

**Before:**
```markdown
- [ ] review PR for auth feature 🔺 📅 2026-01-25
```

**After:**
```markdown
- [ ] review PR for auth feature 🔺 📅 2026-01-25 [[2026-01-25-review-pr-auth-feature]]
```

Use the Edit tool to append the execution file link.

---

## Step 7: Status Transitions

### planned → pending
When some work is done but questions remain:
```bash
mv executions/planned/file.md executions/pending/file.md
```
Update frontmatter: `status: pending`

### pending → completed
When all questions answered and task done:
```bash
mv executions/pending/file.md executions/completed/file.md
```
Update frontmatter: `status: completed`

Also mark original task as complete:
```markdown
- [x] review PR for auth feature 🔺 📅 2026-01-25 ✅ 2026-01-25 [[2026-01-25-review-pr-auth-feature]]
```

---

## Step 8: Resumption

When `/execute-task` is run with pending executions:

### Auto-Resume Last Pending
If only one pending execution exists, offer to resume it automatically.

### Menu for Multiple Pending
If multiple pending executions exist, show menu:
```
Pending executions found:

1. 🔄 review-pr-auth-feature (2 open questions)
2. 🔄 write-blog-post (1 open question)
3. ➕ Pick new tasks instead
```

### Resuming an Execution
1. Read the execution file
2. Check if open questions have been answered (user may have edited file)
3. Continue where left off
4. Update execution log with new progress

---

## Execution Scope

The subagent has full access to:
- **Bash**: Run commands, scripts, build tools
- **Read/Write/Edit**: Modify any files
- **WebFetch/WebSearch**: Research information
- **Glob/Grep**: Search codebase

### Example Executions

**Task**: "Review PR #123 for auth feature"
- Fetch PR details via `gh pr view 123`
- Read changed files
- Write review comments
- Log actions in execution file

**Task**: "Research vacation destinations in Japan"
- Web search for travel info
- Compile findings in execution file
- List follow-up questions (budget? dates?)

**Task**: "Fix the login bug"
- Search codebase for login code
- Identify issue
- Implement fix
- Run tests
- Log all changes

---

## Question Handling

When blocked, **store questions in execution file** rather than asking interactively:

```markdown
## Open Questions

- [ ] What's the budget for this feature?
- [ ] Should we support OAuth or just email/password?
- [ ] Who should review the PR?
```

This enables async workflows where:
1. User runs `/execute-task`
2. Tasks are executed, questions stored
3. User reviews execution files at leisure
4. User answers questions by editing files
5. User runs `/execute-task` again to resume

---

## Processing Report

After all subagents complete:

```
Execution Complete
------------------
Tasks executed: 3

✅ review-pr-auth-feature → completed
   Reviewed PR, left 2 comments, approved

🔄 write-blog-post → pending (2 questions)
   → executions/pending/2026-01-25-write-blog-post.md

📋 research-vacation → planned (3 questions)
   → executions/planned/2026-01-25-research-vacation.md
```

---

## Error Handling

- If a subagent fails, log error in execution file
- Set status to `pending` with error noted
- Continue with other tasks
- Include failures in processing report
