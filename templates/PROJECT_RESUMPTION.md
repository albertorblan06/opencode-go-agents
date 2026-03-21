# Project Resumption Guide

<!-- TODO: Replace [PROJECT_NAME] with your project name -->

**[PROJECT_NAME] - Session Startup Protocol**

Use this file at the start of every new session to quickly resume context. This document provides the essential state needed to continue work effectively.

---

## Quick Start (30-Second Summary)

<!-- TODO: Update this section with your project details -->

**What this is:** [Brief project description]
**Where to find things:** `[repo_1]/` ([purpose]), `[repo_2]/` ([purpose]), `[repo_docs]/` ([purpose])
**Current priority:** [Top priority task]
**Environments:** [List of development environments]

---

## Project Overview

### System Architecture

```
<!-- TODO: Add your system architecture -->
[Component A] → [Component B] → [Component C]
                ↓
         [Interface]
                ↓
         [Component D]
```

### Repository Structure

<!-- TODO: Update with your repositories -->

| Repo | Path | Purpose | Stack |
|------|------|---------|-------|
| **[repo_1]** | `[repo_1]/` | [Brief description] | [Technologies] |
| **[repo_2]** | `[repo_2]/` | [Brief description] | [Technologies] |
| **[repo_docs]** | `[repo_docs]/` | [Documentation] | [Technologies] |

**Git repos:** [Note about repository relationships]

---

## Current State (As of Last Update)

<!-- TODO: Update this section regularly -->

### Active TODOs (High Priority)

**[repo_1]:**
- [ ] [Task 1 - brief description]
- [ ] [Task 2 - brief description]
- [ ] [Task 3 - brief description]

**[repo_2]:**
- [ ] [Task 1 - brief description]
- [ ] [Task 2 - brief description]

**[repo_docs]:**
- [ ] [Task 1 - brief description]

### Recent Activity (Last 5 Commits)

<!-- TODO: Update with recent commits -->

**[repo_1]:**
- `[commit_hash]` - [Commit message]
- `[commit_hash]` - [Commit message]
- `[commit_hash]` - [Commit message]

**[repo_2]:**
- `[commit_hash]` - [Commit message]
- `[commit_hash]` - [Commit message]

**[repo_docs]:**
- `[commit_hash]` - [Commit message]
- `[commit_hash]` - [Commit message]

---

## Development Environment

<!-- TODO: Update with your environments -->

### Available Environments

| Environment | Access | Purpose |
|-------------|--------|---------|
| **[env_1]** | [How to access] | [Purpose] |
| **[env_2]** | [How to access] | [Purpose] |
| **[env_local]** | [How to access] | [Purpose] |

### Environment Setup

```bash
<!-- TODO: Add environment setup commands -->
```

---

## Build Commands

<!-- TODO: Update with your build commands -->

### [repo_1]
```bash
[build_command]                              # Build all
[test_command]                               # Run tests
```

**Key components:** [List of important components/modules]

### [repo_2]
```bash
[build_command]                              # Build
[test_command]                               # Test
```

### [repo_docs]
```bash
[build_command]    # Build (CI uses this)
[serve_command]    # Local preview
```

---

## Critical Architecture Details

<!-- TODO: Add your architecture details -->

### API/Interface Map

| Endpoint/Interface | Purpose | Direction |
|-------------------|---------|-----------|
| `[interface_1]` | [Purpose] | [Direction] |
| `[interface_2]` | [Purpose] | [Direction] |
| `[interface_3]` | [Purpose] | [Direction] |

### Communication Protocol

<!-- TODO: Add protocol details if applicable -->

**Format:** `[description]`
- **[Field 1]:** [Description]
- **[Field 2]:** [Description]

**Key Message Types:**
| Direction | Type | Purpose |
|-----------|------|---------|
| [A → B] | [Type] | [Purpose] |
| [B → A] | [Type] | [Purpose] |

### Configuration

<!-- TODO: Add configuration details -->

| Setting | Value | Notes |
|---------|-------|-------|
| [Setting 1] | [Value] | [Notes] |
| [Setting 2] | [Value] | [Notes] |

---

## Common Pitfalls (Read Before Coding)

### [repo_1]

<!-- TODO: Add repository-specific pitfalls -->

- **[Pitfall 1]:** [Explanation and solution]
- **[Pitfall 2]:** [Explanation and solution]
- **[Pitfall 3]:** [Explanation and solution]

### [repo_2]

<!-- TODO: Add repository-specific pitfalls -->

- **[Pitfall 1]:** [Explanation and solution]
- **[Pitfall 2]:** [Explanation and solution]

### General

- **[Pitfall 1]:** [Explanation and solution]
- **[Pitfall 2]:** [Explanation and solution]
- **Confirmation:** Never run destructive commands without explicit user confirmation.
- **Validation:** A change is NOT done until validated in the target environment.

---

## File Locations

### Critical Paths

<!-- TODO: Add your critical file paths -->

- **Config:** `[path/to/config]`
- **Source:** `[path/to/main/source]`
- **Tests:** `[path/to/tests]`
- **Agent instructions:** `.agents/INSTRUCTIONS.md`
- **Project state:** `memory/project.md`
- **Error log:** `.agents/ERROR_LOG.md`

### Important Files

<!-- TODO: Add important files -->

- `[file_path]` - [Description]
- `[file_path]` - [Description]

---

## Safety-Critical Rules (if applicable)

<!-- TODO: Adjust based on your safety requirements -->

These must never be violated:

1. **[Rule 1]:** [Description]
2. **[Rule 2]:** [Description]
3. **[Rule 3]:** [Description]
4. **[Rule 4]:** [Description]
5. **[Rule 5]:** [Description]

---

## Session Startup Checklist

When starting a new session, complete these steps:

- [ ] Read this PROJECT_RESUMPTION.md
- [ ] Read `.agents/INSTRUCTIONS.md`
- [ ] Read `.agents/ERROR_LOG.md`
- [ ] Check `[repo_1]/TODO.md` for priorities
- [ ] Run `git status` in all repos
- [ ] Ask user: "What would you like to work on?"

---

## Documentation Update Protocol (REQUIRED)

**Rule:** Every code change MUST include documentation updates.

### What to Update

| Change Type | Documentation to Update |
|-------------|------------------------|
| Code behavior or features | Repository `README.md` |
| Architecture or design decisions | `.agents/architecture.md` or relevant .agents/ file |
| Protocol changes | `.agents/architecture.md` protocol sections |
| Build/development process | `.agents/INSTRUCTIONS.md` or `memory/project.md` |
| Environment, setup, or config | `memory/project.md` |
| API or message type changes | `.agents/architecture.md` + relevant READMEs |
| Configuration | `memory/project.md` |
| New error pattern discovered | `.agents/ERROR_LOG.md` (always required) |
| Session learns new context | `memory/project.md` or agent scratchpads |

### Documentation Quality Standards

- Update docs **concurrently** with code, not after
- If docs are unclear, fix them even if you did not write the original code
- Cross-reference related documentation (do not create contradictions)
- Include specific details: file paths, commands, version numbers, dates

**Remember:** Undocumented changes create technical debt. Future agents (and you) will thank you.

---

## Development Workflow (MANDATORY)

**Every code change must follow this 4-phase workflow:**

### Phase 1: Planning & Decision

1. **Context Gathering** - Read PROJECT_RESUMPTION.md, INSTRUCTIONS.md, ERROR_LOG.md, examine code
2. **Requirement Analysis** - Understand what user wants, identify risks
3. **Discussion Protocol** (REQUIRED for code changes):
   - @advocate (GLM-5) proposes approach
   - @critic (GLM-5) stress-tests proposal
   - @debate-referee (GLM-5) makes final decision
   - @synthesizer relays decision to you
4. **Implementation Planning** - Break into steps, identify files, plan validation
5. **User Authorization** - Present plan, wait for explicit "yes"

### Phase 2: Implementation

6. **Pre-Implementation Baseline** - Run tests, capture baseline
7. **Execute Implementation** - Route to @executor with structured instructions
8. **Documentation Update** (CONCURRENT) - Update README, .agents/, memory/project.md as needed
9. **Post-Implementation Validation** - Run tests, classify result (CLEAN/IMPROVED/REGRESSION/NEW_FAILURE)

### Phase 3: Testing & Iteration

10. **Unit Testing** - Write tests, ensure coverage
11. **Integration Testing** - Test in target environment
12. **Self-Correction** (if needed) - One retry if validation failed, then escalate
13. **User Validation** - Present evidence, get acceptance

### Phase 4: Knowledge Preservation

14. **Update Memory** - tasks/lessons.md, memory/project.md, agent scratchpads
15. **Completion Report** - Summary, files modified, test results, next steps

### Iteration Rule

If validation fails:
- Return to Step 7 for minor fixes
- Return to Step 3 if approach was fundamentally wrong
- Repeat until validation passes or user escalates

### Critical Gates

| Gate | Requirement |
|------|-------------|
| G1 | Discussion Protocol complete (@debate-referee decision received) |
| G2 | User authorization obtained |
| G3 | Baseline captured (tests passing before changes) |
| G4 | Validation passed (no REGRESSION or NEW_FAILURE) |
| G5 | User acceptance |

**Full details:** See `.agents/WORKFLOW.md` and `.agents/INSTRUCTIONS.md`

---

## Related Documentation

| Document | Purpose | Location |
|----------|---------|----------|
| **INSTRUCTIONS.md** | Agent workflow and standards | `.agents/INSTRUCTIONS.md` |
| **WORKFLOW.md** | Reasoning pipeline and decision protocol | `.agents/WORKFLOW.md` |
| **startup.md** | Detailed session startup protocol | `.agents/startup.md` |
| **ERROR_LOG.md** | Past mistakes and prevention rules | `.agents/ERROR_LOG.md` |
| **project.md** | Project metadata and config | `memory/project.md` |

---

## External Resources

<!-- TODO: Add your external resources -->

- **Documentation site:** [URL]
- **Repository:** [URL]
- **Issue tracker:** [URL]

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| <!-- TODO: Date --> | Initial creation with full project context | <!-- TODO: Author --> |

---

## Agent Prompt for Session Initialization

When a new session starts, read this file and present the following to the user:

<!-- TODO: Update with your repository names -->

```
**Session Resumed: [PROJECT_NAME]**

**Active Repositories:**
- [repo_1]: [branch, last commit, clean/dirty]
- [repo_2]: [branch, last commit, clean/dirty]
- [repo_docs]: [branch, last commit, clean/dirty]

**High Priority TODOs:**
1. [Top priority]
2. [Second priority]
3. [Third priority]

**Current Focus:** [Current area of focus]

**What would you like to work on?**
[ ] Continue with [previous task]
[ ] New task: [your input]
[ ] Review and prioritize TODOs
[ ] Other: [your input]
```
