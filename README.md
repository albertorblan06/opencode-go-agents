# OpenCode Go Agents

A multi-agent system for [OpenCode](https://opencode.ai) that uses OpenCode Go models ($10/month) for most work and falls back to Claude Opus 4.6 via GitHub Copilot only when needed. Designed for the best economy-to-performance ratio.

Features:
- **Discussion Protocol** -- subagents debate (advocate -> critic -> synthesizer) before every implementation
- **Persistent Memory** -- project knowledge and per-agent scratchpads that accumulate across sessions
- **Self-Correction** -- automated retry with error context when post-validation detects regressions
- **Pre/Post Validation Hooks** -- baseline capture before changes, regression detection after

## Architecture

Every user message goes through the **Auto** orchestrator (Kimi K2.5), which reads relevant code, prompt-engineers structured XML instruction blocks, and routes to the best agent for the job. GLM-5 only fires when code actually needs to be written.

```
User message
    |
    v
 Auto (Kimi K2.5) -- reads code, reasons, engineers structured prompt
    |
    |-- Simple question? ----------> Answers directly (no GLM-5 cost)
    |-- Need exploration? ----------> @mapper (Kimi K2.5)
    |-- Need deep analysis? --------> @reasoner (Kimi K2.5)
    |-- Need complex planning? -----> @planner (Kimi K2.5)
    |-- Need code work? ------------> @executor / @architect / @debugger (GLM-5)
    |-- Need code review? ----------> @auditor (GLM-5, read-only)
    |-- Need validation? -----------> @verifier (Kimi K2.5, read-only)
    |-- Documentation? -------------> @docs-manager (MiniMax M2.5)
    |-- Git operations? ------------> @git-manager (MiniMax M2.5)
    |-- Ambiguous approach? --------> DISCUSSION PROTOCOL (see below)
    |
    v
 Failed? --> Tab to switch --> Auto-Fallback (Claude Opus 4.6, handles directly)
```

## Agents

| Agent | Model | Role | Tool Access |
|-------|-------|------|-------------|
| **auto** | Kimi K2.5 | Prompt-engineering orchestrator | Read-only |
| **auto-fallback** | Claude Opus 4.6 | Direct fallback handler | Full |
| **architect** | GLM-5 | System design, API contracts | Full |
| **executor** | GLM-5 | Code implementation, tests, refactoring | Full |
| **debugger** | GLM-5 | Bug investigation and fixing | Full |
| **auditor** | GLM-5 | Code review, security, performance audit | Read-only |
| **planner** | Kimi K2.5 | Task breakdown, structured instruction output | Read-only, no bash |
| **mapper** | Kimi K2.5 | Codebase exploration, structure mapping | Read-only, no bash |
| **reasoner** | Kimi K2.5 | Deep analysis, trade-off evaluation | Read-only, no bash |
| **verifier** | Kimi K2.5 | Requirement verification, test validation | Read-only |
| **advocate** | GLM-5 | Proposes best approach in debates | Read-only, no bash |
| **critic** | GLM-5 | Stress-tests proposals, finds weaknesses | Read-only, no bash |
| **synthesizer** | Kimi K2.5 | Final decision-maker in debates | Read-only, no bash |
| **docs-manager** | MiniMax M2.5 | Documentation writing and maintenance | Full |
| **git-manager** | MiniMax M2.5 | Git operations, commits, branches, PRs | Full |

## How It Works

### Prompt Engineering Pipeline

The core idea: cheap reasoning (Kimi) crafts precise instructions so expensive code models (GLM-5) get it right on the first try.

1. User sends a message (e.g., "add auth to the API")
2. Auto (Kimi) reads relevant source files to understand context
3. Auto engineers a structured `<executor-instructions>` block with exact file paths, steps, code examples, validation commands, and guardrails
4. Auto forwards the block to @executor (GLM-5), which follows it precisely
5. Auto calls @verifier to validate the result

For complex tasks, Auto calls @planner first for deep breakdown, or @architect for design work before implementation.

### Instruction Blocks

The planner and auto orchestrator produce XML-tagged instruction blocks:

- `<architect-instructions>` -- design tasks (sent to @architect)
- `<executor-instructions>` -- implementation tasks (sent to @executor)
- `<debugger-instructions>` -- bug fixes (sent to @debugger)
- `<auditor-instructions>` -- code reviews (sent to @auditor)
- `<verifier-instructions>` -- validation tasks (sent to @verifier)
- `<test-instructions>` -- test writing (sent to @executor)
- `<needs-investigation>` -- unknown context (routed to @mapper or @reasoner, then retried)
- `<discussion-topic>` -- ambiguous decisions (triggers Discussion Protocol debate)

### Discussion Protocol (Subagent Debates)

**Every task that results in code changes goes through the Discussion Protocol first.** Two GLM-5 agents debate the approach from opposing perspectives, then a Kimi K2.5 agent synthesizes the final decision. This catches bad approaches before they waste implementation tokens.

```
 Any task requiring code changes
    |
    v
 @advocate (GLM-5)
    |  Examines code, evaluates options, champions best approach
    |  Produces: <proposal> with arguments, trade-offs, implementation sketch
    |
    v
 @critic (GLM-5)
    |  Independently verifies claims, stress-tests the proposal
    |  Finds weaknesses, checks dismissed alternatives
    |  Produces: <critique> with verdict (STRONG_SUPPORT / CONDITIONAL_SUPPORT / MAJOR_CONCERNS / OPPOSE)
    |
    v
 @synthesizer (Kimi K2.5)
    |  Weighs both sides cheaply, resolves disputed claims
    |  Produces: <decision> with final approach, implementation steps, guardrails
    |
    v
 Auto merges <decision> into instruction blocks -> routes to @architect / @executor
```

**What skips Discussion (read-only operations):**
- Simple questions about code
- Codebase exploration (@mapper)
- Code review (@auditor)
- Git operations (@git-manager)
- Documentation (@docs-manager)

**Cost:** 2 GLM-5 calls + 1 cheap Kimi call per discussion. The cost of debating is small compared to the cost of implementing the wrong approach and redoing it.

### Persistent Memory System

Agents accumulate knowledge across sessions via markdown files in the `memory/` directory:

```
memory/
  project.md               -- Project-level memory (tech stack, conventions, decisions, pitfalls)
  scratchpad-executor.md   -- Executor's patterns, past mistakes, codebase knowledge
  scratchpad-architect.md  -- Architect's design patterns, past decisions
  scratchpad-debugger.md   -- Debugger's bug patterns, past investigations, fragile areas
  scratchpad-auditor.md    -- Auditor's risk areas, recurring issues, standards
```

**How it works:**
1. Auto reads `memory/project.md` at the start of every conversation
2. Before routing to an agent, Auto reads the agent's scratchpad and injects relevant entries into the instruction CONTEXT
3. After task completion, agents write new discoveries to their scratchpad
4. Over time, the system builds a knowledge base that prevents repeated mistakes and enforces learned conventions

**What gets recorded:**
- Tech stack, build/test/lint commands (auto-discovered)
- Architectural decisions and their rationale
- Implementation mistakes and their corrections
- Codebase patterns, fragile areas, and coupling relationships
- Security/quality findings from audits

### Self-Correction Loop

When post-implementation validation detects a regression:

```
Post-validation fails (build error, test regression, lint failure)
    |
    v
Auto diagnoses the specific failure from error output
    |
    v
Auto re-engineers instructions with the error context and fix strategy
    |
    v
Same agent retries with targeted fix instructions
    |
    v
Post-validation runs again
    |
    ├─ Fixed -> Report success with self-correction note
    └─ Still failing -> Escalate to user, suggest auto-fallback
```

One retry only. If self-correction fails, the user is informed with full context.

### Pre/Post Validation Hooks

Before any code change, Auto captures a validation baseline (build status, test results, lint output). After the change, the same commands run again and results are diffed:

- **CLEAN** -- nothing broke
- **IMPROVED** -- previously failing tests now pass
- **REGRESSION** -- something that worked before now fails (triggers self-correction)
- **NEW_FAILURE** -- new failures introduced (triggers self-correction)

Validation commands are auto-discovered on first use and stored in `memory/project.md` for future sessions.

### Fallback

If Go model agents fail, press Tab to switch to **auto-fallback** which uses Claude Opus 4.6 via GitHub Copilot credits. Unlike Auto, the fallback handles work directly instead of delegating.

## Installation

### Option 1: Copy to your OpenCode config directory

```bash
# Clone the repo
git clone https://github.com/albertorblan06/opencode-go-agents.git

# Copy to your project or OpenCode config
cp opencode.json /path/to/your/project/opencode.json
cp -r agents/ /path/to/your/project/agents/
cp -r memory/ /path/to/your/project/memory/
```

### Option 2: Use from `~/.config/opencode/`

```bash
git clone https://github.com/albertorblan06/opencode-go-agents.git
cp -r opencode-go-agents/agents/ ~/.config/opencode/agents/
cp -r opencode-go-agents/memory/ ~/.config/opencode/memory/
```

Then update the `opencode.json` prompt paths from `{file:./agents/...}` to `{file:~/.config/opencode/agents/...}`.

## Requirements

- [OpenCode](https://opencode.ai) installed
- [OpenCode Go subscription](https://opencode.ai/pricing) ($10/month) for GLM-5, Kimi K2.5, and MiniMax M2.5
- (Optional) GitHub Copilot access for the Claude Opus 4.6 fallback agent

## Customization

### Adding your own provider

Edit `opencode.json` to add providers (e.g., Ollama for local models):

```json
{
  "provider": {
    "ollama": {
      "models": {
        "your-model:tag": {
          "_launch": true,
          "name": "your-model:tag"
        }
      },
      "name": "Ollama (local)",
      "npm": "@ai-sdk/openai-compatible",
      "options": {
        "baseURL": "http://127.0.0.1:11434/v1"
      }
    }
  }
}
```

### Changing agent models

Swap any agent's model in `opencode.json`. For example, to run the executor on a local model:

```json
"executor": {
  "model": "ollama/your-model:tag",
  ...
}
```

### Modifying agent behavior

Each agent's prompt is in `agents/<name>.md`. Edit the markdown to change behavior, add guardrails, or adjust the instruction templates.

## License

MIT
