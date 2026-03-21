# Agent Instructions

## [PROJECT_NAME] Workspace

<!-- TODO: Replace [PROJECT_NAME] with your project name -->

This file defines the agent workflow, engineering standards, and operational protocols for the [PROJECT_NAME] project. All agents must read this file before commencing work.

---

## Project Overview

<!-- TODO: Fill in project description, repositories, and tech stack -->

[PROJECT_NAME] is [brief description of what the project does].

### Repository Structure

| Repository | Purpose | Tech Stack |
|------------|---------|------------|
| **[repo_1]** | [Brief description] | [Technologies] |
| **[repo_2]** | [Brief description] | [Technologies] |
| **[repo_docs]** | [Documentation] | [Technologies] |

### Architecture Overview

```
<!-- TODO: Add system architecture diagram -->
[Component A] → [Component B] → [Component C]
                ↓
         [Component D]
```

### Safety Classification

<!-- TODO: Select appropriate classification -->

**Classification:** [Non-critical / Business-critical / Safety-critical / Life-critical]

**Requirements:**
<!-- TODO: Adjust based on safety classification -->
- [ ] Validate all inputs before use
- [ ] Implement bounds checking on all outputs
- [ ] Provide fail-safe defaults
- [ ] Log all safety-relevant events
- [ ] Implement graceful degradation

---

## Agent Workflow

### High-Level Reasoning Pipeline (MANDATORY)

**ALL code changes must follow this pipeline. No exceptions.**

```
User Request
    |
    v
1. CONTEXT GATHERING
   - Read PROJECT_RESUMPTION.md (session context)
   - Read this INSTRUCTIONS.md (standards)
   - Read AGENTS.md from target repository
   - Read ERROR_LOG.md for past mistakes
   - Check for TODO.md files
   - Map current codebase state
    |
    v
2. REQUIREMENT ANALYSIS
   - What is the user asking?
   - Is it a bug, feature, refactor, or question?
   - What are implicit requirements?
   - What could go wrong?
    |
    v
3. DISCUSSION PROTOCOL (REQUIRED for all code changes)
   |
   ├─> @advocate (GLM-5 Brain) proposes best approach
   │      - Deep code analysis
   │      - Argues FOR the solution
   │
   ├─> @critic (GLM-5 Brain) stress-tests proposal
   │      - Finds weaknesses
   │      - Validates claims
   │      - Proposes mitigations
   │
   ├─> @debate-referee (GLM-5 Brain) makes final decision
   │      - Evaluates both sides
   │      - Renders definitive verdict
   │      - Provides implementation directive
   │
   └─> @synthesizer (MiniMax) relays decision to orchestrator
          - Pure pass-through
          - You (Auto/Kimi) receive <relayed-decision>
    |
    v
4. IMPLEMENTATION PLANNING
   - Break into discrete, verifiable steps
   - Identify files to modify/create
   - Plan validation strategy
   - Estimate complexity
   - DOCUMENT THE PLAN
    |
    v
5. USER AUTHORIZATION (REQUIRED)
   - Present: Problem → Approach → Trade-offs → Plan
   - Give recommendation
   - Wait for explicit confirmation
   - Never start without authorization

PHASE 2: IMPLEMENTATION
========================

6. PRE-IMPLEMENTATION BASELINE
   - Run existing tests → capture baseline
   - Verify build succeeds
   - Document current state
    |
    v
7. EXECUTE IMPLEMENTATION
   - Route to @executor with structured instructions
   - Use format: CONTEXT, STEPS, VALIDATION, DO_NOT
   - Changes must be traceable to debate decision
    |
    v
8. DOCUMENTATION UPDATE (CONCURRENT - NOT OPTIONAL)
   - Update README.md if behavior changed
   - Update .agents/ files if architecture/protocol changed
   - Update memory/project.md if config changed
   - Update ERROR_LOG.md if new error discovered
   - **Rule: No code change without doc update**
    |
    v
9. POST-IMPLEMENTATION VALIDATION
   - Run tests (same as baseline)
   - Compare results
   - Classify: CLEAN / IMPROVED / REGRESSION / NEW_FAILURE

PHASE 3: TESTING & ITERATION
=============================

10. UNIT TESTING
    - Write unit tests for new code
    - Ensure edge cases covered
    - Fix test failures
    |
    v
11. INTEGRATION TESTING
    - Test in target environment
    - Verify end-to-end functionality
    - Check for regressions
    |
    v
12. SELF-CORRECTION (if validation failed)
    - If REGRESSION or NEW_FAILURE:
        1. DIAGNOSE root cause
        2. RE-ENGINEER fix instructions
        3. RETRY with @executor
        4. RE-VALIDATE
        5. (One retry only - then escalate)
    |
    v
13. USER VALIDATION
    - Present completed work with evidence
    - Show: tests pass, no regressions, behavior correct
    - Get explicit acceptance

PHASE 4: KNOWLEDGE PRESERVATION
================================

14. UPDATE MEMORY
    - tasks/lessons.md (if user corrected or error found)
    - memory/project.md (architecture/config changes)
    - Agent scratchpads (mistakes to avoid)
    |
    v
15. COMPLETION REPORT
    - Summary of changes
    - Files modified
    - Tests run and results
    - Documentation updated
    - Next steps

ITERATION RULE
==============

If validation fails at Step 12:
    ┌───────────────────────────────────────────┐
    │  RETURN TO:                               │
    │  • Step 7 (Implementation) for minor fixes│
    │  • Step 3 (Discussion) if approach flawed │
    └───────────────────────────────────────────┘
    
Repeat until validation passes or user escalates.
```

### Critical Gates

| Gate | Requirement | Cannot Proceed If |
|------|-------------|-------------------|
| G1 | Discussion Protocol complete | No @debate-referee decision |
| G2 | User authorization obtained | No explicit "yes" from user |
| G3 | Baseline captured | Tests failing before changes |
| G4 | Validation passed | REGRESSION or NEW_FAILURE |
| G5 | User acceptance | User rejects or requests changes |

---

## Code Standards

### Simplicity and Readability

| Metric | Limit |
|--------|-------|
| Function lines | 30 |
| File lines | 300 |
| Function parameters | 5 |
| Nested conditional depth | 2 |
| Class methods | 10 |
| Cyclomatic complexity | 10 |

**Guidelines:**
- Code should be readable by a junior engineer
- One responsibility per function
- Early returns over deep nesting
- Self-documenting variable names (avoid `tmp`, `x`, `i` except loops)
- No magic numbers - use named constants

### Complexity Management

- Target O(1) or O(n) algorithms
- Avoid O(n^2) unless explicitly justified
- No premature optimization
- No over-engineering - simplest correct solution wins
- If approaching complexity limits, refactor rather than extend

### Safety-Critical Standards

<!-- TODO: Adjust based on your project's safety requirements -->

**For safety-critical systems:**

1. **Input Validation:** All inputs must be validated before use
2. **Bounds Checking:** All outputs must be bounded before execution
3. **Fail-Safe Defaults:** When in doubt, apply safe state
4. **No Unbounded Operations:** No unbounded loops or recursion in real-time paths
5. **Error Logging:** All errors must be logged with context (function, line, values)
6. **Defensive Programming:** Assume inputs may be malicious or out of range

### Documentation Standards

| Element | Requirement |
|---------|-------------|
| Public functions | Docstrings required |
| Complex logic | Comments explain WHY, not WHAT |
| Architecture decisions | Architecture Decision Records (ADRs) |
| Behavior changes | Update README.md |

### Testing Standards

| Test Type | Coverage Target | Framework |
|-----------|------------------|-----------|
| Unit tests | 80%+ general, 100% critical | <!-- TODO: your framework --> |
| Integration tests | All critical paths | <!-- TODO: your framework --> |
| E2E tests | Critical user journeys | <!-- TODO: your framework --> |

**Rules:**
- Unit tests must pass before commit
- Integration tests must pass before commit
- No test coverage exemption without explicit user approval

---

## Repository-Specific Context

<!-- TODO: Fill in repository-specific details -->

### [repo_1]

**Before any changes, read:**
- `.agents/architecture.md` - System architecture
- `.agents/error_log.md` - Known issues

**Build Commands:**
```bash
<!-- TODO: Add build commands -->
```

**Requirements:**
<!-- TODO: Add requirements -->

### [repo_2]

**Before any changes, read:**
- `.agents/AGENTS.md` (in [repo_2]) - Critical rules

**CRITICAL RULES:**
<!-- TODO: Add critical rules -->

**Build Commands:**
```bash
<!-- TODO: Add build commands -->
```

---

## Development Environments

<!-- TODO: Fill in development environment details -->

### Target Hardware/Environments

| Environment | Connection | Workspace | Purpose |
|-------------|------------|-----------|---------|
| **[env_1]** | [connection method] | [path] | [purpose] |
| **[env_2]** | [connection method] | [path] | [purpose] |

### Environment Setup

```bash
<!-- TODO: Add environment setup commands -->
```

---

## When to Consult the User

### ALWAYS Consult Before

| Category | Examples |
|----------|----------|
| Public API changes | Message type changes, service definitions |
| Safety-critical code | Control systems, emergency stops, bounds checking |
| New dependencies | Adding packages, libraries, tools |
| Large refactors | Modifying >200 lines across multiple files |
| Configuration changes | Environment variables, config files |
| Deleting functionality | Removing existing features |
| Multiple valid approaches | When trade-off analysis yields no clear winner |

### Consider Consulting

- Performance vs. readability trade-offs
- Unclear or ambiguous requirements
- Breaking changes to existing behavior
- Test coverage exemptions

### Consultation Format

```
**Decision Required: [Brief title]**

**Context:** [What's happening and why]

**Option A: [Name]**
- Pros: [list]
- Cons: [list]

**Option B: [Name]**
- Pros: [list]
- Cons: [list]

**Recommendation:** [A or B] because [reasoning]

Proceed with recommendation, or prefer alternative?
```

---

## Task Management

### On Startup/Resume

1. Read this INSTRUCTIONS.md
2. Read ERROR_LOG.md
3. Search for TODO.md in each repository
4. Check git status in each repository
5. Ask user: "What would you like to work on?" or "Should I continue with [last task]?"

### Before Starting Work

1. Confirm understanding with user
2. Get explicit authorization
3. Verify baseline (tests pass, build succeeds)

### Definition of Done

- [ ] Code implements requirements correctly
- [ ] Unit tests pass (if applicable)
- [ ] Integration tests pass (if applicable)
- [ ] No regressions in existing tests
- [ ] **Documentation updated** (REQUIRED - see below)
- [ ] ERROR_LOG updated if new lesson learned
- [ ] User validated and accepted

#### Documentation Update Requirements (Every Change)

**Mandatory:** Every code change must include corresponding documentation updates.

| If you changed... | Then update... |
|-------------------|----------------|
| Code behavior or features | `README.md` in the repository |
| Architecture or design | `.agents/architecture.md` or relevant .agents/ file |
| Protocols or APIs | `.agents/architecture.md` protocol sections |
| Build/development process | `.agents/INSTRUCTIONS.md` or `memory/project.md` |
| Environment or setup | `memory/project.md` |
| API or message types | `.agents/architecture.md` and relevant READMEs |
| Configuration | `memory/project.md` |
| Discovered error pattern | `.agents/ERROR_LOG.md` (always) |

**Rule:** If the change affects how future developers or agents will work with the code, document it immediately. Undocumented changes create technical debt.

---

## Git Protocol

### Before Any Commit

1. `git status` - see what's staged
2. `git diff --cached` - verify changes line-by-line
3. `git diff` - see unstaged changes
4. Build and test
5. Write descriptive commit message

### Commit Message Format

```
<type>: <short summary>

<body explaining what and why>

Refs: #issue-number
```

**Types:** feat, fix, docs, refactor, test, chore

### Never Commit

- Secrets, passwords, API keys
- Build artifacts
- Temporary/debug files
- Breaking changes without user approval

---

## Error Handling Standards

### All Code Must

- Validate inputs at function boundaries
- Return errors, do not panic/crash
- Log errors with context (function, line, values)
- Provide actionable error messages
- Implement graceful degradation

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| <!-- TODO: Date --> | Initial creation | <!-- TODO: Author --> |
