# Workflow and Decision Protocol

## The Complete Development Workflow

All code changes MUST follow this workflow. Each phase gates the next.

```
PHASE 1: PLANNING & DECISION
==============================

User Request
    |
    v
[Step 1] Context Gathering
    - Read PROJECT_RESUMPTION.md (session context)
    - Read INSTRUCTIONS.md (standards)
    - Read ERROR_LOG.md (past mistakes)
    - Examine relevant code
    - Check TODOs
    |
    v
[Step 2] Multi-Perspective Analysis
    - Identify constraints
    - List assumptions
    - Find edge cases
    - Consider safety implications
    |
    v
[Step 3] Discussion Protocol (REQUIRED for code changes)
    |
    ├─> @advocate (GLM-5) proposes best approach
    │      └─> Analyzes code, argues FOR solution
    │
    ├─> @critic (GLM-5) stress-tests proposal
    │      └─> Finds weaknesses, validates claims
    │
    ├─> @debate-referee (GLM-5) makes final decision
    │      └─> Evaluates both, renders verdict
    │
    └─> @synthesizer relays decision to orchestrator
           └─> You (Auto/Kimi) receive <relayed-decision>
    |
    v
[Step 4] Implementation Planning
    - Break into discrete, verifiable steps
    - Identify files to modify/create
    - Plan validation strategy
    - Document the plan (in .agents/ or as comments)
    |
    v
[Step 5] User Authorization
    - Present plan with trade-offs
    - Wait for explicit confirmation
    - Never start without authorization

PHASE 2: IMPLEMENTATION
========================

[Step 6] Pre-Implementation Baseline
    - Run existing tests
    - Verify build succeeds
    - Document baseline state
    |
    v
[Step 7] Execute Implementation
    - Route to @executor with structured instructions
    - Include: CONTEXT, STEPS, VALIDATION, DO_NOT
    - Make changes incrementally
    |
    v
[Step 8] Documentation Update (CONCURRENT)
    - Update README.md if behavior changed
    - Update .agents/ files if architecture/protocol changed
    - Update memory/project.md if config changed
    - Update ERROR_LOG.md if new error discovered
    |
    v
[Step 9] Post-Implementation Validation
    - Run same tests as baseline
    - Compare results
    - Classify: CLEAN / IMPROVED / REGRESSION / NEW_FAILURE

PHASE 3: TESTING & ITERATION
=============================

[Step 10] Unit Testing
    - Write unit tests for new code
    - Ensure edge cases covered
    - Run tests, fix failures
    |
    v
[Step 11] Integration Testing
    - Test in target environment
    - Verify end-to-end functionality
    - Check for regressions
    |
    v
[Step 12] Self-Correction (if needed)
    - If REGRESSION or NEW_FAILURE detected:
        1. DIAGNOSE: Identify root cause
        2. RE-ENGINEER: Create fix instructions
        3. RETRY: Route to @executor
        4. RE-VALIDATE: Confirm fix
        5. (One retry only, then escalate)
    |
    v
[Step 13] User Validation
    - Present completed work
    - Show evidence of correctness
    - Get explicit acceptance

PHASE 4: KNOWLEDGE PRESERVATION
================================

[Step 14] Update Memory
    - tasks/lessons.md (if user corrected or error found)
    - memory/project.md (architecture/config changes)
    - Agent scratchpads (mistakes to avoid)
    |
    v
[Step 15] Completion Report
    - Summary of changes
    - Files modified
    - Tests run and results
    - Documentation updated
    - Next steps (if any)

ITERATION RULE
==============

If validation fails after Step 12:
    ┌─────────────────────────────────────┐
    │  Return to Step 7 (Implementation)  │
    │  OR                                 │
    │  Return to Step 3 (Discussion)      │
    │  if approach was fundamentally      │
    │  flawed                             │
    └─────────────────────────────────────┘
    
Repeat until validation passes or user escalates.
```

### Workflow Principles

1. **No skipping phases.** Each phase validates readiness for the next.
2. **Discussion Protocol is mandatory** for any code modification.
3. **Documentation is concurrent**, not an afterthought.
4. **One self-correction attempt** before escalating to user.
5. **User authorization required** before implementation starts.
6. **Validation proves correctness**, not just "it compiles."

---

## When to Consult the User

### MANDATORY Consultation

Consult the user before proceeding when any of these apply:

1. **Public API changes** - Message types, service definitions, function signatures
2. **Safety-critical modifications** - Control systems, emergency stops, bounds checking
3. **New dependencies** - Adding packages, libraries, tools
4. **Large refactors** - Modifying more than 200 lines across files
5. **Performance trade-offs** - When optimization affects readability or safety
6. **Multiple valid approaches** - When trade-off analysis yields no clear winner
7. **Breaking changes** - Any change that alters existing behavior
8. **Configuration changes** - Environment variables, infrastructure changes

### OPTIONAL Consultation

These are judgment calls based on complexity and impact:

- Implementation details with no external impact
- Documentation improvements
- Test additions
- Bug fixes with obvious solutions
- Routine dependency updates

### CONSULTATION REQUIRED Examples

**Example 1: Changing API Protocol**
```
**Decision Required: Add new message type to protocol**

**Context:**
Need to add status reporting from [Component B] to [Component A].

**Option A: Add new message type (0x0C)**
- Pros: Clean, explicit, backward compatible
- Cons: Requires protocol version bump, both sides must update
- Complexity: Low

**Option B: Extend existing status message (0x0B)**
- Pros: No new message type, minimal changes
- Cons: Bloats existing message, mixing concerns
- Complexity: Low

**Recommendation:** Option A - explicit messages are clearer and easier to debug

Proceed with recommendation, or prefer alternative?
```

**Example 2: Configuration Change**
```
**Decision Required: Modify default timeout values**

**Context:**
Current timeout (500ms) causes false disconnects under high load.

**Option A: Increase to 1000ms**
- Pros: Should eliminate false disconnects
- Cons: Slower failure detection
- Risk: Low

**Option B: Implement adaptive timeout**
- Pros: Dynamic based on load
- Cons: Adds complexity, harder to debug
- Risk: Medium - more parameters to tune

**Recommendation:** Option A first (safe improvement), then Option B if needed

Proceed with recommendation, or prefer alternative?
```

---

## Consultation Format

When presenting decisions to the user, use this structure:

```
**Decision Required: [Title]**

**Context:**
[What's the situation? Why is this decision needed?]

**Options:**

**Option A: [Name]**
[Description of the approach]
Pros:
- [Pro 1]
- [Pro 2]
Cons:
- [Con 1]
- [Con 2]

**Option B: [Name]**
[Description]
Pros:
- [Pro 1]
Cons:
- [Con 1]

**Recommendation:** [A or B] because [reasoning]

**Proceed with recommendation, or prefer alternative?**
```

---

## Code Review Checklist

Before presenting completed work:

### Correctness

- [ ] Implements requirements correctly
- [ ] Handles edge cases
- [ ] No logic errors
- [ ] Thread-safe (if concurrent code)

### Safety

- [ ] Inputs validated at function boundaries
- [ ] Bounds checked on all outputs
- [ ] Fail-safes in place
- [ ] No undefined behavior
- [ ] Errors logged with context

### Quality

- [ ] Follows style guide
- [ ] Functions are small and focused (max 30 lines)
- [ ] Names are descriptive
- [ ] No dead code
- [ ] No magic numbers

### Testing

- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Edge cases covered
- [ ] No regressions in existing tests

### Documentation (REQUIRED for every change)

- [ ] **Code comments explain WHY** (not what)
- [ ] **README.md updated** if behavior changed or new feature added
- [ ] **.agents/ documentation updated** if architecture, protocol, or workflow changed
- [ ] **memory/project.md updated** if configuration, environment, or setup changed
- [ ] **Architecture decisions recorded** in relevant .agents/ file
- [ ] **Error log updated** if new error pattern discovered

**Rule: No code change is complete without documentation update. If the change affects how something works, the docs must reflect it.**

---

## Simplicity Standards

### Prefer

| Principle | Example |
|-----------|---------|
| Explicit over implicit | `steering_angle_degrees` vs `angle` |
| Simple over clever | Clear loop vs one-liner with side effects |
| Readable over optimized | Maintainable code vs micro-optimized |
| Composition over inheritance | Function calls vs class hierarchies |
| Pure functions over side effects | `calculate_pid(target, current)` vs global state |
| Early returns over nesting | `if (!valid) return error;` vs deeply nested ifs |

### Avoid

| Anti-pattern | Why | Alternative |
|--------------|-----|-------------|
| Premature abstraction | YAGNI - you aren't gonna need it | Wait until pattern emerges |
| Complex design patterns | Over-engineering | Simple class/function |
| "Smart" code | Hard to read, hard to debug | Clear, obvious code |
| Deep inheritance | Fragile, hard to follow | Composition |
| Global state | Hidden dependencies | Explicit parameters |
| Magic numbers | Unclear meaning | Named constants |
| Premature optimization | Complexity for no gain | Profile first |

---

## Complexity Limits

| Metric | Limit | Action if Exceeded |
|--------|-------|-------------------|
| Function lines | 30 | Split into smaller functions |
| File lines | 300 | Split into multiple files |
| Function parameters | 5 | Use configuration object |
| Nested depth | 2 | Extract to helper function |
| Class methods | 10 | Split class responsibility |
| Cyclomatic complexity | 10 | Simplify logic flow |

**Rule:** If approaching limits, refactor. Do not extend.

---

## Edge Case Handling

### Common Edge Cases

| Scenario | Required Behavior |
|----------|-------------------|
| Invalid input | Log error, use default or fail-safe |
| Communication timeout | Log error, retry with backoff |
| Resource unavailable | Queue request or return error |
| State overflow | Implement clamping or reset |
| Message out of sequence | Log error, discard or reorder |
| Configuration invalid | Fail fast with clear error message |

### Safety Edge Cases

| Scenario | Required Behavior |
|----------|-------------------|
| Out-of-range command | Cap to valid range, log error |
| Missing heartbeat | Apply safe state after timeout |
| Watchdog trigger | Log error, attempt recovery |
| Unexpected state | Transition to safe state |

---

## Validation Protocol

### For Bug Fixes

1. Write test that reproduces the bug
2. Verify test fails
3. Fix the bug
4. Verify test passes
5. Verify no regressions in related tests

### For New Features

1. Write tests for all acceptance criteria
2. Verify tests fail (nothing implemented)
3. Implement feature
4. Verify tests pass
5. Add integration tests
6. Verify all tests pass

### For Refactors

1. Ensure tests exist for code under test
2. Verify all tests pass before refactor
3. Make changes
4. Verify all tests pass after refactor
5. Verify performance unchanged

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| <!-- TODO: Date --> | Initial creation | <!-- TODO: Author --> |
