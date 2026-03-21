# Startup Protocol

Complete this protocol at the start of every session to ensure context is current and work is prioritized correctly.

---

## On Every Session Start

### Step 1: Read Essential Files

Read these files in order before any work:

```
1. .agents/PROJECT_RESUMPTION.md - Session startup context (START HERE)
2. .agents/INSTRUCTIONS.md       - Workflow and standards
3. .agents/ERROR_LOG.md          - Past mistakes to avoid
4. .agents/WORKFLOW.md           - Reasoning pipeline
5. memory/project.md             - Project state
```

### Step 2: Check Repository Status

Run git status and git log in each repository:

<!-- TODO: Update with your repository names -->

```bash
# [repo_1]
cd [repo_1] && git status && git log --oneline -5

# [repo_2]
cd [repo_2] && git status && git log --oneline -5

# [repo_docs]
cd [repo_docs] && git status && git log --oneline -5
```

### Step 3: Check for TODOs

Search for outstanding tasks across all repositories:

<!-- TODO: Update file extensions for your tech stack -->

```bash
# Find TODOs in [repo_1]
grep -rn "TODO\|FIXME\|XXX" [repo_1]/ \
  --include="*.md" \
  --include="*.py" \
  --include="*.js" \
  --include="*.ts" 2>/dev/null | head -20

# Find TODOs in [repo_2]
grep -rn "TODO\|FIXME\|XXX" [repo_2]/ \
  --include="*.md" \
  --include="*.c" \
  --include="*.h" 2>/dev/null | head -20

# Find TODOs in [repo_docs]
grep -rn "TODO\|FIXME\|XXX" [repo_docs]/ \
  --include="*.md" 2>/dev/null | head -10
```

### Step 4: User Check-in

Present workspace status to user:

<!-- TODO: Update with your repository names -->

```
**Workspace Status:**
- [repo_1]: [clean/dirty, last commit message, branch]
- [repo_2]: [clean/dirty, last commit message, branch]
- [repo_docs]: [clean/dirty, last commit message, branch]

**Active TODOs Found:** [N items across repos]

What would you like to work on?
[ ] Continue with previous task: [description from last session]
[ ] New task: [user input]
[ ] Review TODOs and prioritize
[ ] Other: [user input]
```

---

## Before Starting Work

### Get Authorization

Never start implementation without explicit user confirmation. Present:

```
**Proposed Task: [Title]**

**Understanding:**
[What I think you want - restate in my own words]

**Approach:**
[How I plan to implement it]

**Files to modify:**
- [file path] - [what changes]
- [file path] - [what changes]

**Tests to write/run:**
- [test type] - [what it verifies]

**Estimated complexity:** [low/medium/high]

**Authorize implementation?** (yes/no/modify)
```

### Verify Baseline

Before modifying code, verify the baseline is working:

<!-- TODO: Update with your build commands -->

```bash
# [repo_1] - should build without errors
cd [repo_1]
[build_command]

# [repo_2] - should compile successfully
cd [repo_2]
[build_command]

# [repo_docs] - should pass strict build
cd [repo_docs]
[build_command]
```

**If baseline fails:** Do not proceed. Diagnose the failure first.

---

## During Work

### Checkpoint Updates

For tasks expected to take more than 30 minutes, provide progress updates:

```
**Progress Update:**
- Completed: [what's done]
- In progress: [current step]
- Blocked by: [if anything blocking]
- ETA: [estimated time remaining]
```

### On Errors

If an error occurs during implementation:

1. Log it in ERROR_LOG.md
2. Attempt self-correction (one retry with different approach)
3. If still failing, present to user with:
   - Error message (exact)
   - What I tried
   - Request for guidance

**Do not hide errors. Do not claim success when failing.**

---

## Completing Work

### Completion Checklist

Before marking a task complete:

- [ ] Implementation matches requirements
- [ ] Unit tests pass (if applicable)
- [ ] Integration tests pass (if applicable)
- [ ] No regressions in existing tests
- [ ] Build succeeds
- [ ] Documentation updated
- [ ] ERROR_LOG updated (if applicable)
- [ ] User validated

### Completion Report

Present when task is finished:

```
**Task Complete: [Title]**

**Summary:**
[What was done - concise description]

**Files Modified:**
- [file path] - [what changed]
- [file path] - [what changed]

**Verification:**
- Build: [success/failed]
- Tests: [X/Y passed]
- [Other validation results]

**Next Steps:**
[Optional: suggestions for follow-up work]
```

---

## Emergency Procedures

### If Build Breaks

1. **Stop work immediately**
2. **Diagnose:** Run `git diff` to see what changed
3. **Fix or revert:** Either fix the issue or `git checkout -- <file>`
4. **Update ERROR_LOG:** Document what went wrong
5. **Resume:** Only after build succeeds

### If User Says "Stop"

1. **Halt immediately** - Do not continue even if mid-edit
2. **Do not commit** - Leave current changes unstaged
3. **Clarify:** Ask what to do instead
4. **Wait** - Resume only after explicit new authorization

### If Confused

1. **State what I understand** - "I think this means..."
2. **Ask specific questions** - "Can you clarify whether..."
3. **Get clarification** - Wait for clear answer
4. **Proceed only when clear** - Ambiguity leads to errors

---

## Session End

### Before Ending Session

If work is incomplete:

1. Document current state in a comment or note
2. Note what remains to be done
3. Note any blocking issues
4. Confirm user is satisfied with progress

### For Incomplete Work

If session ends mid-task:

```
**Incomplete Task: [Title]**

**Status:** [percentage or phase]

**What remains:**
- [ ] [Remaining item 1]
- [ ] [Remaining item 2]

**Blockers:** [if any]

**To resume:** [brief instructions]
```

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| <!-- TODO: Date --> | Initial creation | <!-- TODO: Author --> |
