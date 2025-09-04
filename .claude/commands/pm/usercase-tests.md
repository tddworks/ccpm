---
allowed-tools: Bash, Read, Write, LS, Task
---

# User Case Tests

Convert user cases into concrete unit test cases following Chicago School (state-based) or London School (mock-based) TDD patterns.

## Usage
```
/pm:usercase-tests <epic_name>
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

2. **Check for user case files:**
   - Check if usercase files (*-usercases.md) exist in `.claude/epics/$ARGUMENTS/`
   - If no user cases found, tell user: "❌ No user cases found. First run: /pm:task-usercases $ARGUMENTS"
   - Stop execution if no user cases exist

3. **Check for existing test files:**
   - Check if any test files (*-tests.md) already exist
   - If they exist, ask user: "⚠️ Found existing test files. Overwrite? (yes/no)"
   - Only proceed with explicit 'yes' confirmation
   - If user says no, suggest: "View existing tests with file browser"

## Instructions

You are converting user cases into unit test cases for: **$ARGUMENTS**

### 1. Read User Case Files
- Find all usercase files (*-usercases.md) in `.claude/epics/$ARGUMENTS/`
- Load each file and understand:
  - User scenarios and goals
  - Step-by-step user journeys
  - Success criteria and performance requirements

### 2. Determine TDD School Approach
- **Chicago School (State-based)**: Test final state/values after operations
- **London School (Mock-based)**: Test interactions/behavior between objects
- **Both**: Use appropriate approach based on scenario (recommended)

### 3. Generate Unit Test Cases
For each user case file, create corresponding test file: `.claude/epics/$ARGUMENTS/{task_number}-tests.md`

#### Test File Format
```markdown
---
task: {task_number}
task_name: {task name}
user_cases_source: {task_number}-usercases.md
created: [Current ISO date/time]
tdd_school: both  # chicago, london, or both
test_cases: {count}
---

# Unit Tests: {Task Name}

## From UC-{task}-{#}: {User Case Title}

### Test Suite: {LogicalGrouping}

#### Test Case: TC-{task}-{usercase}-{#} (Chicago School - State-based)
**Given**: {Test setup conditions}
**When**: {Action being tested}
**Then**: {Expected outcomes - focus on final state}

```javascript
// Chicago School - Test final state/values
test('should {behavior description}', async () => {
  // Arrange
  {setup code with real objects}
  
  // Act
  {action code}
  
  // Assert - Check final state
  {state assertions}
});
```

#### Test Case: TC-{task}-{usercase}-{#} (London School - Interaction-based) 
**Given**: {Test setup with mocks}
**When**: {Action triggering interactions}
**Then**: {Expected interaction patterns}

```javascript
// London School - Test interactions/behavior
test('should {interaction description}', async () => {
  // Arrange
  {mock setup}
  
  // Act
  {action code}
  
  // Assert - Check interactions
  {interaction assertions with mocks}
});
```

[Continue for each user case...]
```

### 4. Test Implementation Guidelines

#### Framework Selection
Choose appropriate test syntax based on detected/specified tech stack:
- **JavaScript/TypeScript**: Jest, Mocha, Jasmine
- **Java/Kotlin**: JUnit, TestNG, MockK
- **Python**: pytest, unittest, mock
- **C#/.NET**: xUnit, NUnit, Moq
- **Others**: Match project's existing test framework

#### Test Coverage
For each user scenario, generate 2-4 test cases covering:
- **Happy path**: Core functionality working as expected
- **Edge cases**: Boundary conditions, unusual inputs
- **Error conditions**: Validation failures, exceptions
- **Performance**: Speed/memory requirements from user case

#### TDD School Implementation
- **Chicago School**: Focus on state verification
  - Assert final values, object states, return values
  - Use real objects when possible
  - Test the "what" (outcomes)

- **London School**: Focus on behavior verification
  - Assert method calls, interaction patterns
  - Use mocks/spies extensively
  - Test the "how" (collaborations)

### 5. Parallel Processing
For epics with many user cases (>6), use parallel agents:

```yaml
Task:
  description: "Generate unit tests batch {X}"
  subagent_type: "general-purpose" 
  prompt: |
    Generate test files for user cases {list}
    Use both Chicago and London TDD patterns
    [Include detailed instructions]
```

### 6. Quality Validation

Before finalizing test cases, verify:
- [ ] Tests trace back to specific user scenarios
- [ ] Both TDD schools represented appropriately
- [ ] Framework-specific syntax is correct
- [ ] Performance requirements are tested
- [ ] Error conditions are covered
- [ ] Test cases reflect real user mental models

### 7. Post-Creation

After successfully creating test cases:
1. Confirm: "✅ Created unit tests for {count} user case files in epic: $ARGUMENTS"
2. Show summary:
   - Total test cases generated
   - TDD school distribution (Chicago vs London)
   - Framework coverage
   - Traceability from user cases
3. Suggest next step: "Tests are ready for implementation! Use them to guide TDD development."

## Error Recovery

If any step fails:
- List which user case files were processed successfully
- Identify specific failures and reasons
- Provide clear recovery steps
- Never leave malformed test files

Focus on creating executable unit tests that developers can actually implement, following TDD principles and reflecting real user scenarios from "$ARGUMENTS".