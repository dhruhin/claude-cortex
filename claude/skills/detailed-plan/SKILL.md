---
name: detailed-plan
description: Interview-driven planning for complex tasks. Asks questions to understand intent before proposing solutions. Use for non-trivial tasks (>1 step).
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, AskUserQuestion
created: 2026-01-31T16:59
updated: 2026-02-02T00:44
---

# Detailed Plan - Collaborative Planning Protocol

Interview-driven planning workflow that asks questions to understand user intent before proposing solutions. Plans iterate through multiple drafts with progressive refinement.

## When to Use

**Use this skill for:**
- Any non-trivial task (more than 1 step)
- Multiple files or components involved
- Unclear requirements or multiple valid approaches
- Architectural decisions or design questions
- When you detect ambiguity in the request
- Tasks mentioning: "refactor", "architecture", "design", "implement [feature]"

**Skip this skill for:**
- Simple, single-step tasks with clear intent
- User has given explicit detailed instructions
- Pure research/exploration (use Explore agent instead)

## Question Protocol

**Ask many questions** to understand intent before proposing solutions:
- Group questions by category (Requirements, Constraints, Preferences, etc.)
- Each question gets a **Recommendation** with your reasoning
- Include blank **User Input:** field (with space) for responses
- When presenting multiple options, list them and **bold the recommended option**

**Question format:**
```markdown
- **Q1**: Should we use approach A or B?

  **Recommendation**: Approach A because it's more maintainable and scalable.

  **User Input:**
```

**Option listing format** (when appropriate):
```markdown
- **Q2**: Which authentication method should we use?

  **Recommendation**: Option A is best for this use case.

  Options:
  - **Option A (Recommended)**: JWT tokens - stateless, scalable
  - Option B: Session cookies - easy to revoke, needs session store
  - Option C: OAuth 2.0 - third-party auth, more complex

  **User Input:**
```

## Iteration Triggers

**Default behavior: iterate**
- Any user response triggers next iteration (even just "next", " ", etc.)
- Move blank/approved User Input fields to Decisions section
- Incorporate Additional Input into new questions or implementation
- Ask new questions that emerged from feedback
- Clear Additional Input section for next round

**Execution:**
- User says "execute" or "do it" → stop iterating, implement the plan
- No new questions and implementation is clear → show "Ready to Execute" message

## Draft Progression

**Each iteration:**
1. Take user feedback from inline User Input fields and Additional Input section
2. Move approved items (blank User Input = approved) to Decisions section at bottom
3. Incorporate Additional Input into new questions or implementation plan
4. Ask new questions that emerged
5. Clear Additional Input section
6. Increment iteration number
7. Update Iteration Log
8. Output terminal summary: "Iteration N ready - moved X items to Decisions, asked Y new questions"

**Silence = approval:**
- Blank User Input field means approved
- Don't ask about it again in next iteration
- Summarize in Decisions section for context

**Progressive decluttering:**
- Over iterations, Questions section shrinks
- Decisions section grows
- Implementation plan becomes clearer

## Plan File Structure

**Save to:** `plans/YYYY-MM-DD-slug.md`

**Template for iterations with questions:**
```markdown
---
created: YYYY-MM-DDTHH:mm
status: draft | in-progress | ready-to-execute
iteration: N
---

# Plan: [Title]

## 🎯 Goal
[What we're achieving]

## ❓ Questions (Iteration N)

### [Category]
- **Q#**: [Question]?

  **Recommendation**: [My suggestion with reasoning]

  **User Input:**

## 💬 Additional Input
<!-- Add any ideas or feedback that don't fit questions above -->
-



---

## 📋 Implementation Plan
[Specific changes - only when ready]

## ⚠️ Risks
[Any concerns]

---

## ✅ Decisions (from previous iterations)
[Summaries for context]

## 🔄 Iteration Log
[Running notes]
```

**Template when ready to execute:**
```markdown
## ✅ Ready to Execute

No remaining questions. Implementation plan is complete.
Say "execute" or "do it" to proceed with changes.

## 💬 Additional Input
<!-- Any final thoughts before execution? -->
-



---

[Keep Implementation Plan, Risks, Decisions, Iteration Log sections]
```

## Workflow

1. **Initial invocation:**
   - Ask 10-20 context questions grouped by category
   - Format with inline User Input fields
   - Create plan file in `plans/`
   - Output terminal summary

2. **User responds:**
   - Fill in User Input fields (or leave blank for approval)
   - Add ideas to Additional Input section
   - Say "next" or just respond to trigger iteration

3. **Next iteration:**
   - Move approved items to Decisions
   - Incorporate Additional Input
   - Ask new questions if needed
   - Update plan file
   - Output terminal summary

4. **Ready to execute:**
   - No more questions OR user says "execute"
   - Show "Ready to Execute" message
   - Wait for user confirmation

5. **Execution:**
   - User says "execute" or "do it"
   - Implement the plan
   - Update plan status to completed

## Example Patterns

**Initial questions:**
```markdown
## ❓ Questions (Iteration 1)

### Requirements
- **Q1**: What data should this feature handle?

  **Recommendation**: Based on the existing schema, I'd suggest supporting user profiles and preferences.

  **User Input:**

- **Q2**: Should this support real-time updates or batch processing?

  **Recommendation**: Real-time updates using WebSockets for better UX.

  **User Input:**

### Constraints
- **Q3**: Are there performance requirements?

  **Recommendation**: Target <100ms response time for API calls.

  **User Input:**

### Preferences
- **Q4**: Which testing approach should we use?

  **Recommendation**: Jest for unit tests, Playwright for integration tests.

  Options:
  - **Jest + Playwright (Recommended)**: Full coverage, good DX
  - Vitest + Cypress: Faster but less ecosystem support
  - Pure Node assertions: Minimal but harder to maintain

  **User Input:**

## 💬 Additional Input
-
```

**Iteration progression:**
```markdown
## 🔄 Iteration Log

### Iteration 1 - 2026-02-02 00:45
Asked 12 initial questions about requirements, constraints, and preferences.

### Iteration 2 - 2026-02-02 01:00
- Moved Q1-Q8 to Decisions (approved)
- Incorporated Additional Input: user wants to support mobile apps
- Asked 5 new questions about mobile API design and authentication

### Iteration 3 - 2026-02-02 01:15
- Moved Q9-Q13 to Decisions
- No new questions - implementation plan is clear
- Status changed to ready-to-execute
```

## Key Behaviors

- **Ask before acting:** Don't make assumptions, ask questions
- **Recommend but don't decide:** Always suggest your preferred approach with reasoning
- **Default to iterate:** Unless user says "execute", keep refining
- **Progressive refinement:** Each iteration should get clearer
- **Keep user in driver's seat:** Present options, wait for approval
- **Flexibility:** User can "execute" even with Additional Input remaining if they're ready
- **Terminal summaries:** Brief output after each iteration for tracking progress
