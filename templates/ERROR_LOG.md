# Agent Error Log

Lessons learned from mistakes. Read this before starting work to avoid repeating errors.

---

## Format

Each entry must include:
- **Date:** YYYY-MM-DD
- **Repo:** Which repository ([repo_1], [repo_2], [repo_docs])
- **Error:** What went wrong
- **Impact:** Consequences
- **Root Cause:** Why it happened
- **Fix:** How it was resolved
- **Prevention:** Rule to prevent recurrence

---

## Error Entries

### Template Entry

- **Date:** YYYY-MM-DD
- **Repo:** [repository name]
- **Error:** [Description of what went wrong]
- **Impact:** [What broke or what were the consequences]
- **Root Cause:** [Why did it happen - technical explanation]
- **Fix:** [How it was resolved]
- **Prevention:** [Specific rule or check to prevent recurrence]

<!-- TODO: Add actual error entries below as they occur -->

---

## Patterns to Avoid

### Pattern 1: [Pattern Name]

**Symptom:** [What you observed]
**Cause:** [Root cause]
**Solution:** [How to fix]

### Pattern 2: [Pattern Name]

**Symptom:** [What you observed]
**Cause:** [Root cause]
**Solution:** [How to fix]

### Pattern 3: [Pattern Name]

**Symptom:** [What you observed]
**Cause:** [Root cause]
**Solution:** [How to fix]

<!-- TODO: Add common error patterns specific to your project -->

---

## Critical Reminders

### [Repository 1] Development

<!-- TODO: Add repository-specific critical reminders -->

- [ ] [Reminder 1]
- [ ] [Reminder 2]
- [ ] [Reminder 3]

### [Repository 2] Development

<!-- TODO: Add repository-specific critical reminders -->

- [ ] [Reminder 1]
- [ ] [Reminder 2]
- [ ] [Reminder 3]

### Safety-Critical Code (if applicable)

<!-- TODO: Adjust based on your safety requirements -->

- [ ] ALWAYS validate input bounds
- [ ] ALWAYS validate output bounds
- [ ] ALWAYS implement fail-safe defaults
- [ ] ALWAYS log safety-critical events

### General

- [ ] NEVER commit secrets, passwords, or API keys
- [ ] NEVER push breaking changes without user approval
- [ ] ALWAYS update error log when discovering new failure patterns
- [ ] ALWAYS verify build passes before claiming task complete

---

## Postmortem Template

For significant errors, create a postmortem in `.agents/postmortems/`:

```markdown
# Postmortem: [Incident Title]

**Date:** YYYY-MM-DD
**Duration:** [How long the issue lasted]
**Severity:** [Critical/High/Medium/Low]
**Status:** [Root cause identified/resolved/mitigated]

## Summary
[One paragraph overview of what happened]

## Timeline
- HH:MM - [Event]
- HH:MM - [Event]
- HH:MM - [Resolution]

## Root Cause
[Technical explanation of why]

## Impact
- What failed
- User-facing symptoms
- Duration of outage

## Resolution
[How it was fixed]

## Lessons Learned
1. [Lesson 1]
2. [Lesson 2]

## Action Items
- [ ] [Action] - [Owner] - [Due date]
```

---

## Example Error Entries

### Example 1: Build Configuration

- **Date:** 2024-01-15
- **Repo:** backend
- **Error:** Build failed with "Module not found" error for local package
- **Impact:** CI pipeline blocked for 2 hours
- **Root Cause:** Forgot to run `npm install` in the package directory after adding dependency
- **Fix:** Ran `npm install` in affected package and updated CI to use workspaces
- **Prevention:** Always run install after modifying package.json; CI should detect this

### Example 2: Database Migration

- **Date:** 2024-02-20
- **Repo:** backend
- **Error:** Migration failed in production due to existing data
- **Impact:** Production database in inconsistent state, required manual rollback
- **Root Cause:** Migration assumed empty table but production had data
- **Fix:** Wrote backward-compatible migration, tested on production backup
- **Prevention:** Always test migrations on production-like data; use backward-compatible changes

### Example 3: API Breaking Change

- **Date:** 2024-03-10
- **Repo:** api
- **Error:** Frontend broke after API response format change
- **Impact:** Users couldn't load dashboard for 30 minutes
- **Root Cause:** Changed response format without versioning or coordination
- **Fix:** Rolled back API change, implemented versioning (v1, v2)
- **Prevention:** API changes require versioning; coordinate with frontend team

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| <!-- TODO: Date --> | Initial template creation | <!-- TODO: Author --> |
