# Project Lessons Learned

This file tracks accumulated knowledge, lessons learned, and patterns discovered during the project lifecycle. Read this regularly to avoid repeating mistakes and to leverage proven solutions.

---

## Active Rules

These are currently-enforced standards and constraints. Rules here are actively applied to all new work.

### Format

Each rule must include:
- **ID:** Unique identifier (e.g., R001, R002)
- **Category:** Code, Architecture, Process, Testing, Documentation, Safety
- **Rule:** The specific rule to follow
- **Rationale:** Why this rule exists
- **Source:** Link to lesson or decision that created this rule
- **Created:** YYYY-MM-DD
- **Status:** Active / Under Review / Deprecated

### Active Rules List

#### R001: [Rule Title]

- **ID:** R001
- **Category:** [Category]
- **Rule:** [Specific rule statement]
- **Rationale:** [Why this rule exists]
- **Source:** [Link to lesson entry]
- **Created:** YYYY-MM-DD
- **Status:** Active

<!-- Add new rules as they are discovered -->

---

## Lessons Log

Chronological record of mistakes, corrections, and insights. Each entry documents what was learned and how to avoid repetition.

### Format

Each lesson must include:
- **ID:** Unique identifier (e.g., L001, L002)
- **Date:** YYYY-MM-DD
- **Category:** Bug, Performance, Architecture, Process, Tooling, Communication
- **Context:** What were we working on?
- **What Happened:** Description of the issue or realization
- **Root Cause:** Why did this happen?
- **Resolution:** How was it fixed or addressed?
- **Impact:** What was affected?
- **Rules Created:** Any active rules resulting from this lesson
- **Related:** Links to related lessons or documentation

### Lessons

#### L001: [Lesson Title]

- **ID:** L001
- **Date:** YYYY-MM-DD
- **Category:** [Category]
- **Context:** [What was being worked on]
- **What Happened:** [Description]
- **Root Cause:** [Why it happened]
- **Resolution:** [How it was fixed]
- **Impact:** [What was affected]
- **Rules Created:** [List any rules added]
- **Related:** [Links]

<!-- Add new lessons chronologically -->

---

## Pattern Tracker

Reusable solutions to recurring problems. When you discover a pattern that works, document it here for future reference.

### Format

Each pattern must include:
- **ID:** Unique identifier (e.g., P001, P002)
- **Name:** Descriptive name
- **Category:** Design Pattern, Code Pattern, Testing Pattern, Architecture Pattern, Process Pattern
- **Problem:** What problem does this solve?
- **Solution:** The pattern description
- **When to Use:** Applicability guidelines
- **When NOT to Use:** Anti-patterns or exceptions
- **Example:** Code or process example
- **Benefits:** Why this pattern is effective
- **Trade-offs:** What you give up
- **Related:** Similar or complementary patterns
- **Created:** YYYY-MM-DD

### Patterns

#### P001: [Pattern Name]

- **ID:** P001
- **Name:** [Pattern Name]
- **Category:** [Category]
- **Problem:** [What problem it solves]
- **Solution:** [How to solve it]
- **When to Use:**
  - [Condition 1]
  - [Condition 2]
- **When NOT to Use:**
  - [Exception 1]
  - [Exception 2]
- **Example:**
  ```[language]
  // Example code or process steps
  ```
- **Benefits:**
  - [Benefit 1]
  - [Benefit 2]
- **Trade-offs:**
  - [Trade-off 1]
- **Related:** [Links to related patterns]
- **Created:** YYYY-MM-DD

<!-- Add new patterns as they are discovered -->

---

## Category Reference

### Lesson/Rule Categories

| Category | Description |
|----------|-------------|
| Code | Language-specific issues, syntax, idioms |
| Architecture | System design, component relationships |
| Process | Development workflow, collaboration |
| Testing | Test strategies, coverage, frameworks |
| Documentation | Documentation standards, organization |
| Safety | Safety-critical concerns, validation |
| Performance | Optimization, efficiency concerns |
| Tooling | Build tools, linters, IDE issues |
| Communication | Team coordination, requirements |

---

## Usage Guidelines

### When to Update This File

1. **After fixing a bug** - Add a lesson entry explaining the root cause
2. **After a user correction** - Document what was misunderstood
3. **After discovering a better way** - Add a pattern entry
4. **After violating a rule** - Update the rule's enforcement section
5. **During retrospectives** - Synthesize insights into lessons

### Before Starting Work

1. Check Active Rules for constraints that apply to your task
2. Review relevant Lessons for similar past work
3. Look for applicable Patterns to leverage
4. Update lessons.md if you discover outdated information

### Writing Good Lessons

**Good lessons:**
- Focus on one specific insight
- Explain the "why" behind the lesson
- Include concrete examples
- Link to related documentation
- Suggest actionable prevention

**Avoid:**
- Generic advice ("write better tests")
- Vague descriptions ("something broke")
- Blame or subjective criticism
- Overly broad lessons

---

## Quick Reference

### Most Recent Lessons

<!-- Auto: Update this section with the last 3-5 lessons -->

1. L001 - [Title] (YYYY-MM-DD)
2. L002 - [Title] (YYYY-MM-DD)
3. L003 - [Title] (YYYY-MM-DD)

### Recently Added Rules

<!-- Auto: Update this section with recently added rules -->

1. R001 - [Title]
2. R002 - [Title]

### Most Used Patterns

<!-- Auto: Update this section with frequently referenced patterns -->

1. P001 - [Name]
2. P002 - [Name]

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial template creation | [Author] |

---

**Remember:** This file is a living document. Update it continuously as you learn. An undocumented lesson is a lesson that will be repeated.
