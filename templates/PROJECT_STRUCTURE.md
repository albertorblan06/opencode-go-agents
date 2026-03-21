# Project Structure Guide

<!-- TODO: Replace [PROJECT_NAME] with your project name -->
<!-- TODO: Customize the directory structure for your project -->

## Overview

```
[PROJECT_NAME]/
├── .agents/                    # Agent instructions and logs
│   ├── INSTRUCTIONS.md         # Main agent workflow and standards
│   ├── ERROR_LOG.md            # Mistakes and lessons learned
│   ├── PROJECT_STRUCTURE.md    # This file
│   ├── TESTING_METHODOLOGY.md  # Testing standards and frameworks
│   ├── WORKFLOW.md             # Reasoning pipeline and decision protocol
│   └── startup.md              # Session startup protocol
│
├── memory/                     # Persistent project state
│   └── project.md              # Project metadata and configuration
│
├── README.md                   # Workspace overview
│
<!-- TODO: Add your repository directories below -->
├── [repo_1]/                   # [Repository purpose]
│   ├── .agents/                # Repo-specific agent context
│   │   ├── architecture.md     # System architecture
│   │   ├── environment.md      # Environment context
│   │   ├── error_log.md        # Repo-specific errors
│   │   └── AGENTS.md           # Repo-specific rules
│   ├── src/                    # Source code
│   ├── tests/                  # Test suites
│   ├── docs/                   # In-repo documentation
│   └── README.md
│
├── [repo_2]/                   # [Repository purpose]
│   ├── .agents/                # Repo-specific agent context
│   │   └── AGENTS.md           # Development notes
│   ├── src/                    # Source code
│   ├── tests/                  # Tests
│   └── README.md
│
└── [repo_docs]/                # Documentation site
    ├── .agents/                # Repo-specific agent context
    │   └── AGENTS.md           # Documentation rules
    ├── docs/                   # Documentation source
    └── README.md
```

---

## Repository Relationships

```
<!-- TODO: Update with your system architecture -->
┌─────────────────────┐
│    [repo_1]         │  [High-level description]
│    ([tech_stack])   │
│                     │
│  [Component A] ──→  │
│  [Component B] ─────┼────────── [Interface/Protocol]
│  [Output] ──────────┘          │
└─────────────────────┘          │
                                  │
┌─────────────────────┐          │
│   [repo_2]          │◄─────────┘
│   ([tech_stack])    │  [Low-level description]
│                     │
│  [Component C] ───┼──→ [Outputs]
│  [Component D]    │
└─────────────────────┘
                                  │
┌─────────────────────┐           │
│   [repo_docs]       │           │
│   ([tech_stack])    │           │
│                     │
│  Documentation      │◄─────────┘
└─────────────────────┘
```

---

## Key Files by Purpose

### Starting Work

| Step | Action | File(s) |
|------|--------|---------|
| 1 | Read workspace instructions | `.agents/INSTRUCTIONS.md` |
| 2 | Read repo-specific context | `<repo>/.agents/AGENTS.md` |
| 3 | Review past errors | `.agents/ERROR_LOG.md` |
| 4 | Check task status | `<repo>/TODO.md`, `<repo>/.agents/tasks.md` |
| 5 | Verify baseline | Git status in each repo |

### Building

<!-- TODO: Add your build commands -->

| Repository | Command | Notes |
|------------|---------|-------|
| [repo_1] | `[build_command]` | [Notes] |
| [repo_2] | `[build_command]` | [Notes] |
| [repo_docs] | `[build_command]` | [Notes] |

### Testing

<!-- TODO: Add your test commands -->

| Repository | Command | Notes |
|------------|---------|-------|
| [repo_1] | `[test_command]` | [Notes] |
| [repo_2] | `[test_command]` | [Notes] |
| [repo_docs] | `[test_command]` | [Notes] |

---

## Directory Purpose

### `.agents/` Directory

Contains agent-specific documentation that is version-controlled alongside the code. This is the authoritative source for:
- Workflow procedures
- Error prevention rules
- Repository-specific context
- Task management

**Not for official project documentation** - those go in `[repo_docs]/`.

### `memory/` Directory

Contains persistent project state that spans sessions:
- Project metadata
- Known working configurations
- Development machine access
- Active decisions log

---

## File Naming Conventions

<!-- TODO: Customize for your tech stack -->

### [Language/Framework 1] ([repo_1])

| Pattern | Example | Purpose |
|---------|---------|---------|
| `*.py` | `detector.py` | Python modules |
| `*.hpp` | `protocol.hpp` | C++ headers |
| `*.cpp` | `bridge.cpp` | C++ sources |
| `*.yaml` | `config.yaml` | Configuration |

### [Language/Framework 2] ([repo_2])

| Pattern | Example | Purpose |
|---------|---------|---------|
| `*.c` | `module.c` | C source |
| `*.h` | `module.h` | C headers |
| `config.*` | `config.dev` | Configuration |

### Documentation ([repo_docs])

| Pattern | Example | Purpose |
|---------|---------|---------|
| `index.md` | `index.md` | Section landing pages |
| `README.md` | `README.md` | Directory documentation |
| `*.yml` | `mkdocs.yml` | Configuration |

---

## Build Artifacts (Do Not Commit)

<!-- TODO: Add your build artifact patterns -->

| Type | Location | Pattern |
|------|----------|---------|
| Build output | `[repo]/build/` | Generated by build system |
| Install output | `[repo]/install/` | Generated by build system |
| Logs | `[repo]/log/` | Test/build output |
| Cache | `[repo]/.cache/` | Tool caches |
| Python cache | `*/__pycache__/` | .pyc files |
| Editor configs | `*/*.swp`, `*~` | Temp files |

---

## Example: Multi-Repository Project

### Example 1: Robotics/IoT Project

```
autonomous_system/
├── .agents/                    # Agent instructions
├── memory/
├── brain/                      # High-level autonomy (ROS 2)
│   ├── src/
│   │   ├── perception/         # Object detection
│   │   ├── planning/           # Path planning
│   │   └── control/            # Control algorithms
│   ├── tests/
│   └── README.md
├── controller/                 # Embedded firmware (FreeRTOS)
│   ├── main/                   # Source code
│   ├── components/             # Modular components
│   ├── test/
│   └── README.md
└── docs/                       # Documentation (MkDocs)
    ├── docs/
    └── README.md
```

### Example 2: Web Application

```
web_platform/
├── .agents/                    # Agent instructions
├── memory/
├── backend/                    # API server (Node.js)
│   ├── src/
│   │   ├── routes/
│   │   ├── models/
│   │   └── services/
│   ├── tests/
│   └── README.md
├── frontend/                   # Web UI (React)
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   └── hooks/
│   ├── tests/
│   └── README.md
├── infra/                      # Infrastructure (Terraform)
│   ├── terraform/
│   └── README.md
└── docs/                       # Documentation
    └── README.md
```

### Example 3: Data Pipeline

```
data_platform/
├── .agents/                    # Agent instructions
├── memory/
├── ingestion/                  # Data ingestion (Python)
│   ├── src/
│   ├── tests/
│   └── README.md
├── processing/                 # Stream processing
│   ├── src/
│   ├── tests/
│   └── README.md
├── storage/                    # Data storage configs
│   └── README.md
└── docs/                       # Documentation
    └── README.md
```

---

## Repository Setup Checklist

When adding a new repository to the project:

- [ ] Create `<repo>/.agents/AGENTS.md` with repository-specific rules
- [ ] Add build commands to this file
- [ ] Add test commands to this file
- [ ] Document file naming conventions
- [ ] List build artifacts to ignore
- [ ] Update the architecture diagram

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| <!-- TODO: Date --> | Initial creation | <!-- TODO: Author --> |
