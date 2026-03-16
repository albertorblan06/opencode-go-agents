# Lessons Learned

This file is the system's self-improvement knowledge base. It is read by the Auto orchestrator at the start of every session and updated after every user correction, failed task, or discovered anti-pattern.

**Auto: Read this file BEFORE `memory/project.md`. Cross-reference lessons against every instruction block you engineer. If a lesson applies, inject it as a DO_NOT or GUARDRAIL.**

## Purpose

- Capture every mistake, correction, and anti-pattern so it is never repeated
- Provide concrete rules that agents can follow to prevent recurrence
- Accumulate institutional knowledge that survives across sessions
- Reduce error rate ruthlessly over time

## How to Update This File

### When to Add an Entry

Add a lesson whenever:
1. The user corrects an agent's output (wrong approach, wrong file, wrong logic)
2. A self-correction cycle fires (the mistake itself is the lesson)
3. A verifier reports FAIL and the root cause reveals a pattern
4. A Discussion Protocol debate reveals a non-obvious constraint
5. An agent repeats a mistake that was already avoidable
6. CI/CD fails due to an agent's changes

### Entry Format

```
### L-[NNN]: [Short title]
- **Date:** [YYYY-MM-DD]
- **Trigger:** [What happened -- user correction / self-correction / verifier FAIL / CI failure]
- **What went wrong:** [Specific description with file paths, function names, line numbers]
- **Root cause:** [Why the agent made this mistake]
- **Rule:** [Concrete, actionable rule to prevent recurrence]
- **Applies to:** [Which agents -- @executor, @architect, @debugger, etc.]
- **Severity:** [Critical / High / Medium / Low]
```

### Severity Guide

- **Critical**: Caused data loss, security vulnerability, or production breakage
- **High**: Caused build failure, test failure, or required user intervention to fix
- **Medium**: Caused rework or suboptimal output but no breakage
- **Low**: Minor style/convention violation caught in review

## Active Rules

<!-- These are distilled from lessons below. Auto injects applicable rules into agent instruction blocks. -->

(No active rules yet. Rules are added as lessons accumulate.)

## Lessons Log

<!-- Entries are added chronologically. Each entry follows the format above. -->
<!-- Auto: After adding a lesson, extract a rule and add it to "Active Rules" above. -->

(No lessons recorded yet.)

## Pattern Tracker

<!-- Track recurring mistake categories. If a category reaches 3+ occurrences, escalate to a Critical rule. -->

| Category | Count | Last Seen | Status |
|----------|-------|-----------|--------|
| (none yet) | 0 | - | - |

## Session Review Checklist

<!-- Auto reads this at session start and confirms each item before proceeding. -->

Before starting work on any task:
1. Read all Active Rules above
2. Check if the current task matches any known lesson categories
3. Inject applicable rules as DO_NOT or GUARDRAIL items in instruction blocks
4. If the project has changed since the last session, verify lessons still apply
