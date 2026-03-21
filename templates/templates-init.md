# Template Initialization Guide

Instructions for setting up a new project with the opencode agent templates.

---

## Overview

This guide walks through initializing a new project repository with the standardized agent templates. Following this process ensures consistent workflow, documentation standards, and agent behavior across all projects.

---

## Quick Start

```bash
# 1. Navigate to your new project
cd /path/to/your/new-project

# 2. Create the .agents directory
mkdir -p .agents

# 3. Copy templates from opencode configuration
cp ~/.config/opencode/templates/INSTRUCTIONS.md .agents/
cp ~/.config/opencode/templates/WORKFLOW.md .agents/
cp ~/.config/opencode/templates/PROJECT_STRUCTURE.md .agents/
cp ~/.config/opencode/templates/TESTING_METHODOLOGY.md .agents/
cp ~/.config/opencode/templates/ERROR_LOG.md .agents/
cp ~/.config/opencode/templates/startup.md .agents/
cp ~/.config/opencode/templates/PROJECT_RESUMPTION.md .agents/
cp ~/.config/opencode/templates/README.md .agents/

# 4. Create the tasks directory for lessons tracking
mkdir -p tasks
cp ~/.config/opencode/tasks/lessons.md tasks/

# 5. Create the memory directory for project state
mkdir -p memory
cp ~/.config/opencode/memory/project.md memory/
```

---

## Step-by-Step Setup

### Step 1: Copy Core Templates

Copy the following files to your project's `.agents/` directory:

| Source | Destination | Purpose |
|--------|-------------|---------|
| `~/.config/opencode/templates/INSTRUCTIONS.md` | `.agents/INSTRUCTIONS.md` | Core workflow and standards |
| `~/.config/opencode/templates/WORKFLOW.md` | `.agents/WORKFLOW.md` | Development workflow details |
| `~/.config/opencode/templates/PROJECT_STRUCTURE.md` | `.agents/PROJECT_STRUCTURE.md` | Repository organization |
| `~/.config/opencode/templates/TESTING_METHODOLOGY.md` | `.agents/TESTING_METHODOLOGY.md` | Testing standards |
| `~/.config/opencode/templates/ERROR_LOG.md` | `.agents/ERROR_LOG.md` | Error tracking and prevention |
| `~/.config/opencode/templates/startup.md` | `.agents/startup.md` | Session startup protocol |
| `~/.config/opencode/templates/PROJECT_RESUMPTION.md` | `.agents/PROJECT_RESUMPTION.md` | Context restoration |
| `~/.config/opencode/templates/README.md` | `.agents/README.md` | Template overview |

### Step 2: Create Supporting Directories

Create these directories in your project root:

```
your-project/
├── .agents/          # Agent instruction templates (copied above)
├── tasks/            # Task tracking and lessons learned
│   └── lessons.md    # Copy from ~/.config/opencode/tasks/lessons.md
├── memory/           # Persistent project state
│   └── project.md    # Copy from ~/.config/opencode/memory/project.md
└── [your source code...]
```

### Step 3: Fill in Project-Specific Details

#### 3.1 Update `.agents/README.md`

Replace the following placeholders:
- `[PROJECT_NAME]` - Your project name
- `[REPO_NAME]` - Repository name
- `<!-- TODO: -->` sections with project-specific content

#### 3.2 Update `.agents/INSTRUCTIONS.md`

Complete these sections:
- Project description and overview
- Repository structure table
- Technology stack
- Safety classification
- Build commands for each repository
- Development environment details

#### 3.3 Update `.agents/ERROR_LOG.md`

- Remove or keep the example entries as references
- Update repository names in the Critical Reminders section
- Adjust safety-critical reminders based on your domain

#### 3.4 Update `memory/project.md`

Fill in the Tech Stack section:
- Language
- Framework
- Build tool
- Test framework
- Package manager

Also update:
- Validation Commands (build, test, lint)
- Any known conventions discovered during setup

#### 3.5 Update `tasks/lessons.md`

- Replace template examples with your project context
- Set the initial creation date and author
- Keep the structure for future lessons

### Step 4: Create Repository-Specific AGENTS.md

If your project has multiple repositories, create `.agents/AGENTS.md` in each:

```markdown
# [Repository Name] Agent Context

## Overview
[Brief description of this repository]

## Architecture
[Key architectural decisions]

## Critical Rules
- [ ] Rule 1
- [ ] Rule 2

## Build Commands
```bash
# Add your build/test commands here
```

## Common Pitfalls
- Pitfall 1: [Description and prevention]
- Pitfall 2: [Description and prevention]
```

### Step 5: Verify Setup

Check that all files are in place:

```bash
# List all agent files
ls -la .agents/

# Verify lessons file exists
ls -la tasks/lessons.md

# Verify memory file exists
ls -la memory/project.md
```

---

## Customization Checklist

After copying templates, complete these customizations:

### Project Identity
- [ ] Project name in all headers
- [ ] Repository names and purposes
- [ ] Technology stack documented
- [ ] Team/author information

### Workflow
- [ ] Safety classification selected (Non-critical/Business-critical/Safety-critical/Life-critical)
- [ ] Discussion Protocol participants identified
- [ ] Critical gates defined
- [ ] User consultation thresholds set

### Build & Test
- [ ] Build commands documented
- [ ] Test commands documented
- [ ] Lint commands documented
- [ ] Environment setup instructions

### Safety (if applicable)
- [ ] Input validation requirements
- [ ] Bounds checking requirements
- [ ] Fail-safe defaults documented
- [ ] Safety-critical code areas identified

### Documentation
- [ ] README.md created/updated
- [ ] Architecture diagrams added
- [ ] API documentation linked

---

## Template Reference

### What Each Template Contains

| Template | Contains | Customize |
|----------|----------|-----------|
| INSTRUCTIONS.md | Agent workflow, standards, protocols | Project details, build commands, repos |
| WORKFLOW.md | Development workflow, Discussion Protocol | Workflow specifics |
| PROJECT_STRUCTURE.md | Repository organization | Directory structure, naming conventions |
| TESTING_METHODOLOGY.md | Testing standards | Test frameworks, coverage targets |
| ERROR_LOG.md | Error patterns and prevention | Repository-specific errors |
| startup.md | Session startup protocol | Project-specific startup steps |
| PROJECT_RESUMPTION.md | Context restoration | Recent work context |
| README.md | Template overview | Project overview |

### Memory and Task Files

| File | Purpose | Location |
|------|---------|----------|
| memory/project.md | Project state and configuration | `memory/project.md` |
| tasks/lessons.md | Lessons learned tracking | `tasks/lessons.md` |

---

## Multi-Repository Projects

If your project spans multiple repositories:

### Option 1: Centralized Templates

Keep all templates in a `[project]-docs` repository:

```
project-docs/
├── .agents/        # All template files
├── shared/         # Shared configuration
└── README.md
```

Each code repository references the centralized templates.

### Option 2: Distributed Templates

Copy core templates to each repository:

```
repo-1/
├── .agents/        # Repository-specific AGENTS.md + core templates
└── ...

repo-2/
├── .agents/        # Repository-specific AGENTS.md + core templates
└── ...
```

Update each AGENTS.md with repository-specific context.

---

## Maintenance

### Keep Templates Updated

When you improve the templates:

1. Update the template in `~/.config/opencode/templates/`
2. Consider propagating improvements to existing projects
3. Document the change in the template's revision history

### Update Project Files Regularly

- **memory/project.md** - Update when tech stack or conventions change
- **tasks/lessons.md** - Add entries after discoveries or corrections
- **.agents/ERROR_LOG.md** - Add entries when new errors occur
- **.agents/PROJECT_RESUMPTION.md** - Update at end of each session

---

## Troubleshooting

### Agents Not Reading Templates

- Verify files are in `.agents/` directory
- Check file permissions (readable)
- Ensure proper markdown formatting

### Templates Too Generic

- Fill in all `<!-- TODO: -->` sections
- Add repository-specific AGENTS.md files
- Customize safety classification
- Update example entries with real ones

### Multiple Projects Confusion

- Each project should have its own `.agents/` directory
- Use project-specific AGENTS.md for repository context
- Keep memory/project.md in each project root

---

## Example Project Structure

After initialization, your project should look like:

```
my-project/
├── .agents/
│   ├── INSTRUCTIONS.md          # Core workflow
│   ├── WORKFLOW.md              # Development workflow
│   ├── PROJECT_STRUCTURE.md     # Organization guide
│   ├── TESTING_METHODOLOGY.md   # Testing standards
│   ├── ERROR_LOG.md             # Error tracking
│   ├── startup.md               # Startup protocol
│   ├── PROJECT_RESUMPTION.md    # Context restoration
│   ├── README.md                # Template overview
│   └── AGENTS.md                # [Optional] Repo-specific context
├── tasks/
│   └── lessons.md               # Lessons learned tracking
├── memory/
│   └── project.md               # Project state and config
├── src/                         # Your source code
├── tests/                       # Your tests
├── README.md                    # Project documentation
└── ...
```

---

## Next Steps

After initialization:

1. **Read the templates** - Familiarize yourself with the workflow
2. **Fill in TODOs** - Complete all placeholder sections
3. **Test the workflow** - Make a small change to verify everything works
4. **Train your team** - Ensure everyone understands the Discussion Protocol
5. **Iterate** - Improve templates based on experience

---

## Support

For questions or issues with the templates:

1. Check `.agents/README.md` for usage instructions
2. Review `.agents/INSTRUCTIONS.md` for workflow details
3. Look at example entries in `.agents/ERROR_LOG.md`
4. Consult `tasks/lessons.md` for patterns and rules

---

**Remember:** These templates are living documents. Customize them to fit your project, and update them as you learn what works best for your team.
