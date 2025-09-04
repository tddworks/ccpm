---
allowed-tools: Bash, Read, Write, LS, Task
---

# Task User Cases

Break down epic tasks into concrete user scenarios that reflect real user mental models.

## Usage
```
/pm:task-usercases <epic_name>
```

## Required Rules

**IMPORTANT:** Before executing this command, read and follow:
- `.claude/rules/datetime.md` - For getting real current date/time

## Preflight Checklist

Before proceeding, complete these validation steps.
Do not bother the user with preflight checks progress ("I'm not going to ..."). Just do them and move on.

### Validation Steps
1. **Verify epic exists:**
   - Check if `.claude/epics/<epic_name>/epic.md` exists
   - If not found, tell user: "❌ Epic not found: $ARGUMENTS. First create it with: /pm:prd-parse $ARGUMENTS"
   - Stop execution if epic doesn't exist

2. **Check for task files:**
   - Check if numbered task files (001.md, 002.md, etc.) exist in `.claude/epics/$ARGUMENTS/`
   - If no tasks found, tell user: "❌ No tasks found. First run: /pm:epic-decompose $ARGUMENTS"
   - Stop execution if no tasks exist

3. **Check for existing user cases:**
   - Check if any usercase files (*-usercases.md) already exist
   - If they exist, ask user: "⚠️ Found existing user case files. Overwrite? (yes/no)"
   - Only proceed with explicit 'yes' confirmation
   - If user says no, suggest: "View existing user cases with file browser"

## Instructions

You are breaking down epic tasks into user scenarios for: **$ARGUMENTS**

### 1. Read Task Files
- Find all numbered task files (001.md, 002.md, etc.) in `.claude/epics/$ARGUMENTS/`
- Load each task file and understand:
  - Task name and description
  - Acceptance criteria
  - Technical details

### 2. Generate User Cases for Each Task
For each task, create 3-5 realistic user scenarios that reflect real user mental models:

#### User Case File Format
Create file: `.claude/epics/$ARGUMENTS/{task_number}-usercases.md`

```markdown
---
task: {task_number}
task_name: {task name from file}
created: [Current ISO date/time]
user_cases: {count}
---

# User Cases: {Task Name}

## UC-{task}-1: {Scenario Title}
**Actor**: {User type - Junior Developer, Team Lead, Product Manager, etc.}
**Goal**: {What they want to achieve}
**Scenario**: 
- {Step by step user journey}
- {What they see/experience}
- {System responses}
- {Final outcome}

**Success Criteria**:
- {Measurable outcomes}
- {Performance expectations}
- {User experience goals}

[Repeat for 3-5 user cases per task]
```

### 3. Focus on Real User Mental Models
Generate scenarios like:
- **New developer** setting up TDD on unfamiliar project
- **Team lead** standardizing practices across multiple repositories
- **Developer** switching between different tech stacks
- **Product manager** creating tasks with proper test breakdown
- **Senior developer** handling edge cases and complex scenarios

### 4. Parallel Processing
If there are many tasks (>6), use parallel agents to generate user cases more efficiently:

```yaml
Task:
  description: "Generate user cases batch {X}"
  subagent_type: "general-purpose"
  prompt: |
    Generate user case files for tasks {list}
    [Include detailed instructions]
```

### 5. Quality Validation

Before finalizing user cases, verify:
- [ ] Each user case reflects real-world scenarios
- [ ] Scenarios cover different user types and experience levels
- [ ] Success criteria are measurable and realistic
- [ ] User journeys are step-by-step and actionable
- [ ] Edge cases and common problems are included

### 6. Post-Creation

After successfully creating user cases:
1. Confirm: "✅ Created user cases for {count} tasks in epic: $ARGUMENTS"
2. Show summary:
   - Total tasks processed
   - Total user scenarios created
   - Range of user types covered
3. Suggest next step: "Ready to generate unit tests? Run: /pm:usercase-tests $ARGUMENTS"

## Error Recovery

If any step fails:
- List which task files were processed successfully
- Identify specific tasks that failed and why
- Provide clear recovery steps
- Never leave partial user case files

Focus on creating user scenarios that reflect authentic developer experiences and mental models, not just technical requirements for "$ARGUMENTS".