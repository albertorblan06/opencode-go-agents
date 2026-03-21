# [PROJECT_NAME] - Agent Repository Templates

<!-- TODO: Fill in [PROJECT_NAME] with your project name -->
<!-- TODO: Update the repository URLs and paths -->
<!-- TODO: Customize the technology stack section for your project -->

Generic agent instruction templates for AI-assisted software development.

## Overview

This repository contains documentation templates that establish a structured workflow for AI agents working on software projects. These templates ensure consistent quality, safety, and documentation standards across all development work.

## Template Files

| File | Purpose | When to Read |
|------|---------|--------------|
| **INSTRUCTIONS.md** | Core agent workflow and standards | Before any work |
| **WORKFLOW.md** | Complete development workflow with Discussion Protocol | When planning changes |
| **PROJECT_STRUCTURE.md** | Repository organization guide | When navigating codebase |
| **TESTING_METHODOLOGY.md** | Testing standards and frameworks | When writing tests |
| **ERROR_LOG.md** | Mistakes and lessons learned | Before starting work |
| **startup.md** | Session startup protocol | At beginning of each session |
| **PROJECT_RESUMPTION.md** | Quick context restoration | After extended breaks |

## Quick Start for New Projects

1. **Copy these templates** to your repository's `.agents/` directory
2. **Fill in project-specific details** (marked with `<!-- TODO: -->`)
3. **Customize the technology stack** section for your project
4. **Update safety classifications** based on your application domain
5. **Add repository-specific rules** in a `<repo>/.agents/AGENTS.md` file

## Repository Structure

```
[REPO_NAME]/
├── .agents/                    # Agent instructions (this directory)
│   ├── INSTRUCTIONS.md         # Main workflow and standards
│   ├── WORKFLOW.md             # Development workflow
│   ├── PROJECT_STRUCTURE.md    # Repository organization
│   ├── TESTING_METHODOLOGY.md  # Testing standards
│   ├── ERROR_LOG.md            # Error patterns and prevention
│   ├── PROJECT_RESUMPTION.md   # Session startup context
│   └── startup.md              # Startup protocol
├── memory/                     # Persistent project state
│   └── project.md              # Project metadata and config
├── README.md                   # Repository overview
└── [your source code...]
```

## Core Principles

### 1. Discussion Protocol (Mandatory for Code Changes)

All code modifications must go through the Discussion Protocol:

1. **@advocate** - Proposes the best approach, analyzes code deeply
2. **@critic** - Stress-tests the proposal, finds weaknesses
3. **@debate-referee** - Makes the final decision
4. **@synthesizer** - Relays the decision to the implementation agent

**Why this matters:** Multiple perspectives catch edge cases and prevent regressions.

### 2. Four-Phase Workflow

```
PHASE 1: Planning & Decision
- Context gathering
- Multi-perspective analysis
- Discussion Protocol
- User authorization

PHASE 2: Implementation
- Pre-implementation baseline
- Structured execution
- Concurrent documentation
- Post-implementation validation

PHASE 3: Testing & Iteration
- Unit testing
- Integration testing
- Self-correction (if needed)
- User validation

PHASE 4: Knowledge Preservation
- Memory updates
- Completion report
```

### 3. Safety-Critical Standards

<!-- TODO: Adjust based on your project's safety requirements -->

**If your project involves physical systems, safety-critical code must:**
- Validate all inputs before use
- Implement bounds checking on all outputs
- Provide fail-safe defaults
- Log all safety-relevant events
- Implement graceful degradation

### 4. Documentation is Concurrent

**Rule:** Every code change must include corresponding documentation updates.

| If you changed... | Then update... |
|-------------------|----------------|
| Code behavior | `README.md` |
| Architecture | `.agents/architecture.md` |
| Protocols/APIs | `.agents/architecture.md` |
| Build process | `.agents/INSTRUCTIONS.md` |
| Environment | `memory/project.md` |
| Error patterns | `.agents/ERROR_LOG.md` |

## Technology Stack Examples

<!-- TODO: Replace with your actual technology stack -->

### Example 1: Robotics Project
```
[repo_brain]/     - High-level autonomy (ROS 2, Python, PyTorch)
[repo_control]/   - Embedded control (FreeRTOS, C, PlatformIO)
[repo_docs]/      - Documentation (MkDocs)
```

### Example 2: Web Application
```
[repo_backend]/   - API server (Node.js, Express, PostgreSQL)
[repo_frontend]/  - Web UI (React, TypeScript)
[repo_infra]/     - Infrastructure (Terraform, Docker)
```

### Example 3: Data Pipeline
```
[repo_ingestion]/ - Data ingestion (Python, Apache Kafka)
[repo_processing]/- Stream processing (Apache Flink)
[repo_storage]/   - Data storage (ClickHouse)
```

## Customization Guide

### Step 1: Project Identification

Replace all instances of:
- `[PROJECT_NAME]` - Your project name
- `[REPO_NAME]` - Repository name
- `[TECH_STACK]` - Your technology stack

### Step 2: Safety Classification

Update the safety classification based on your domain:

| Domain | Classification | Required Standards |
|--------|---------------|-------------------|
| Web/Mobile App | Non-critical | Standard validation |
| Enterprise Software | Business-critical | Input validation, error handling |
| IoT/Embedded | Safety-aware | Bounds checking, timeouts |
| Automotive/Aerospace | Safety-critical | Full hazard analysis, formal verification |
| Medical | Life-critical | Regulatory compliance, traceability |

### Step 3: Build Commands

Update build/test commands for your stack:

```bash
# Example: Node.js
npm install
npm run build
npm test

# Example: Python
pip install -r requirements.txt
pytest

# Example: Rust
cargo build
cargo test
```

### Step 4: Repository-Specific Rules

Create `<repo>/.agents/AGENTS.md` for each repository with:
- Architecture overview
- Critical rules (e.g., "Never commit to main")
- Common pitfalls
- Build/deployment procedures

## Best Practices

1. **Never skip the Discussion Protocol** for code changes
2. **Always update documentation** concurrently with code
3. **Run tests before and after** changes (baseline comparison)
4. **Log errors** when discovered to prevent recurrence
5. **Get explicit user authorization** before implementation

## Getting Started

For new agents working on this project:

1. Read `.agents/INSTRUCTIONS.md`
2. Read `.agents/ERROR_LOG.md`
3. Check `memory/project.md` for current state
4. Read `<repo>/.agents/AGENTS.md` for repository context
5. Ask: "What would you like to work on?"

## Contributing

<!-- TODO: Add contribution guidelines -->

When improving these templates:
- Follow the 4-phase workflow
- Update all affected documentation
- Add new error patterns to ERROR_LOG.md
- Update PROJECT_RESUMPTION.md with new context

## License

<!-- TODO: Add license information -->

## Revision History

| Date | Change | Author |
|------|--------|--------|
| <!-- TODO: Date --> | Initial template creation | <!-- TODO: Author --> |

---

**Remember:** These templates are living documents. Update them as the project evolves and new patterns emerge.
